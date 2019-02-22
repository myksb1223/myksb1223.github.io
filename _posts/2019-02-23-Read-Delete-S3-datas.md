---
layout: post
title:  "S3 데이터 읽기와 삭제(Read and delete S3 datas)."
date:   2019-02-23 01:21:00
author: Seungbeom Kim
categories: develop_diary
tags:	아마존 AWS S3 서버 Server Backend RubyOnRails Ruby
sitemap :
  changefreq : daily
  priority : 1.0
---

[클래스업(ClassUp) 휴먼계정 처리](https://myksb1223.github.io/develop_diary/2019/02/19/Inactive-Account-Manage.html)시 S3에 존재하는 이미지도 지워야 한다.

이 때, 어떻게 지울 것인가에 대한 몇 가지 시나리오를 구상했고, 이 과정에서 지우지 말아야 할 데이터까지 지워버려서... 다시는 실수하지 않기위해 블로그에 글을 쓰기로 했다.

나는 `Ruby`를 사용하기 때문에 [Ruby-AWS 도큐먼트](https://docs.aws.amazon.com/sdkforruby/api/Aws/S3/Client.html)를 참고했다.

### 시나리오 1.
1. `list_objects`를 사용한여 관력 객체들을 부른다.
2.  반복문을 이용하여 내부의 컨텐츠를 다 지운다. (당연히 `delete_objects`을 사용하여 모아서 지워도 상관없다.)

```Ruby
resp = $s3.list_objects({bucket: bucket_name, prefix: prefix })
if resp != nil
 for content in resp.contents
   $s3.delete_object({ bucket: bucket_name, key: content.key })
 end
end
```

이 시나리오를 사용해서 지우지 말아야 할 데이터를 지웠다.
이유는 무엇이었을까?

바로 `list_objects`의 `prefix` 속성이다.
말 그대로 `prefix`였다.

예를 들어 사용자 id가 1223이라고 가정하고, 아래와 같은 상황을 생각해보면...

    images/1223/1223_1502493949_background.jpeg
    images/12231223/12231223_1502493949_background.jpeg

이렇게 두 형태의 파일이 존재할 때, 당연히 1223번을 지우려고 아래와 같이 코딩하였다.

```Ruby
resp = $s3.list_objects({bucket: bucket_name, prefix: "images/#{user_id}" })
```

단순히 생각하면 내 디렉토리만 불러올 것 같지만... `prefix`라는 속성에 의해 1223으로 시작되는 모든 디렉토리를 불러온다. 이 때문에 삭제시... 1223으로 시작된 모든 디렉토리 내의 데이터를 지우게 된다.

이 때문에... 지우지 않아도 될 데이터를 몽땅 지웠다... 다행히 크게 영향은 없는 데이터여서 운이 좋았다.

그러므로 아래의 시나리오를 따르는 것이 좋다.

### 시나리오 2.

클래스업(ClassUp)은 기본적으로 디렉토리 내에 이미지를 올릴 경우에는 보통 Database에 이미지 수, 이미지 파일네임을 위한 값도 같이 저장했다. 그렇기 때문에 이미지의 이름을 완벽히 알 수 있다.

1. 이미지 수를 이용하여 파일의 경로 및 네임을 직접 만든다.
2. 배열에 넣는다.
3. `delete_objects`를 이용해 한꺼번에 지운다.

```Ruby
# bucket_name : 버킷의 이름.
# pic_cnt : 이미지 갯수.
# user_id : 사용자 아이디.
# timestamp : 이미지가 저장된 시간.

images = [] # delete_objects를 사용하기 위한 배열.
count = 0
while count < pic_cnt do
  image_request = Hash.new
  if count == 0
    image_request[:key] = "images/#{user_id}/#{user_id}_#{timestamp}_test.jpeg"
  else
    image_request[:key] = "images/#{user_id}/#{user_id}_#{timestamp}_test_#{count}.jpeg"
  end
  images << image_request
end

resp = $s3.delete_objects({bucket: bucket_name, delete: { objects: images, quiet: false } })
```

위와 같이하면... 깔끔하게 원하는 것만 지울 수 있다. 당연히 그 외의 다양한 방법도 많겠지만... 내가 사용한 것은 이 두개의 시나리오다.

역시... 코딩은 항상 주의해야한다...
