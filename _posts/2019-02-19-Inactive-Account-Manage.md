---
layout: post
title:  "휴면계정 관리 및 삭제(Manage and delete inactive user)."
date:   2019-02-19 00:39:00
author: Seungbeom Kim
categories: develop_diary
tags:	InactiveUser 휴면계정
sitemap :
  changefreq : daily
  priority : 1.0
---

클래스업(ClassUp)을 운영하면서 **휴면계정(Inactive User)**를 관리해야하는 경우가 생겼다.

일단, 우리는 휴면계정에 대한 처리는 따로 하지 않고, 무조건 1년 6개월 이상 접속이 없는 사용자는 지우기로 하였다.

### 1. 접속기록 남기기.
클래스업(ClassUp)에서는 마지막 접속기록을 남기는 방법은 아래와 같다.

```Ruby
user = User.find(...);

if Time.now.to_i - user.updated_at.to_i > 86400
  user.updated_at = Time.now
  user.save
end
```

간단하게 하루에 단 한번만 체크하는 형식으로 위와 같이 한다.

### 2. 1년 6개월 이상 미접속 사용자 찾기.
그러면 아래와 같이 검색할 수 있다. 나는 `Rails`와 `MySQL`을 연동하였고, 1년 6개월 이상 접속하지 않은 사용자를 찾는 것이기 때문에 아래와 같은 명령어로 찾을 수 있다.

1년 6개월을 timestamp로 표현하면 47260800이다.
MySQL에서 나의 updated_at의 형식은 DATETIME이기 때문에 아래처럼 `UNIX_TIMESTAMP()`를 사용하여야 한다.

```Ruby
now = Time.now.to_i
users = User.find_by_sql("SELECT id FROM users WHERE #{now} - UNIX_TIMESTAMP(updated_at) > 47260800 ORDER BY id")
```

### 3. Script로 지우기.

1. 기존 운영 서버에 영향을 주지 않아야 하기에, AWS EC2에서 스팟을 생성한다.

2. git에 올려둔 서버 쪽 코드를 일단 서버로 동기화 시킨다.

        $ git pull origin master

3. 1년 6개월 이상 지난 사용자의 id만 뽑아서 문서로 만들어 둔다.

    ```Ruby
    users = User.find_by_sql("SELECT id from users where 1550500041 - UNIX_TIMESTAMP(updated_at) > 47260800 ORDER BY id")
    File.open("/log/inactive_user.log", 'w') do |file|
      for user in users do
        file.write(user.id.to_s+"\n")
      end
    end
    ```

4. 사용자 삭제 스크립트를 만들어 지운다. 클래스업(ClassUp)은 사용자가 수업 등록, 노트 작성, 공지사항 작성 등의 여러 데이터를 남길 수 있기에 모든 관련 데이터를 지우는 코드를 작성해야 한다.

1분에 1000개 정도의 속도로 지우고 있다. 지워야 하는 수는 약 70만개... 약 11시간 조금 넘게 걸릴 것 같다.
