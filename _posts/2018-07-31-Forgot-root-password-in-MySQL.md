---
layout: post
title:  "MySQL root 비밀번호 분실 시...(Forgot root password in MySQL)"
date:   2018-07-31 09:54:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp MySQL 루트 Root 비밀번호분실 ForgotPassword 루트비밀번호 RootPassword
sitemap :
  changefreq : daily
  priority : 1.0
---

[이전에 쓴 글](https://myksb1223.github.io/develop_diary/2018/07/31/How-to-detach-or-remove-MySQL-in-existing-Rails-project.html)에서 Rails에 MySQL을 떼어내기 위해서 결국 내부 MySQL을 사용하기로 우회함으로써 급한 불을 껐는데, 이 때 root 비밀번호도 까먹어서 찾질 못했다.

아래에 찾는 방법을 정리해 놓을까 한다.

먼저, MySQL이 실행 중이라면 중지한다.

    $ service mysqld stop
    // Or
    $ kill -9 [process_id]

위와 같이 중지한 후,

    $ /usr/bin/mysqld_safe --skip-grant-tables &

mysqld_safe를 이용하여 패스워드 없이도 접속할 수 있는 모드로 바꾼다. 그리고 MySQL에 접속!

    $ /usr/bin/mysql -u root mysql

이 때, 5.7버전 기준으로 명령어가 조금 나뉘는데, 먼저 버전 확인은 아래와 같이 한다.

    $ mysql --version

**5.7 미만**

    mysql> UPDATE mysql.user SET password=PASSWORD('password') WHERE user='root';
    mysql> FLUSH PRIVILEGES;
    mysql> quit

**5.7 이상**

    mysql> UPDATE mysql.user SET authentication_string=PASSWORD('패스워드') WHERE user='root';
    mysql> FLUSH PRIVILEGES;
    mysql> quit

이렇게 하고 MySQL을 재시작해주면 된다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
