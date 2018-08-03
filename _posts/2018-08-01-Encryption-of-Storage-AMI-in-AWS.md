---
layout: post
title:  "아마존 클라우드 서비스 Storage, AMI 암호화(Encryption of Storage, AMI in AWS)"
date:   2018-08-01 19:40:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp AMI 스토리지 Storage 암호화 Encryption AMI암호화 AMIEncryption AWS 아마존클라우드서비스
sitemap :
  changefreq : daily
  priority : 1.0
---

[GDPR](https://www.eugdpr.org/)로 인해 보안에 좀 더 철저히 할 필요가 생겼다. 그래서 스토리지 암호화를 시도해 보았다.

일단, AWS에서 인스턴스를 처음 생성할 때는 스토리지를 암호화할 수 있다. 하지만 이미 생성된 스토리지는 암호화를 바로 적용할 수 없다. 그래서 방법을 알아보았더니

1. 볼륨(Volume)으로 스냅샷(Snapshot)을 생성
2. 스냅샷으로 이미지(Image)를 생성
3. 생성된 이미지(AMI)를 복사(Copy)
4. 복사할 때, 암호화 적용
5. 암호화된 AMI로 인스턴스 재생성

위의 5가지의 과정이 필요하다.

일단, AWS 콘솔로 들어가서 EC2를 선택하면 왼쪽 메뉴에 볼륨(Volume)이라고 보인다. 선택해서 옆으로 드래그를 쭉~ 하면...

<img src="{{ site.baseurl }}/assets/develop_diary/encryption_of_storage_ami_in_aws_1.png" title="Encryption of Storage, AMI in AWS 1" class="post-image">

위의 그림처럼 **암호화되지 않음**이라는 스토리지가 보인다. 클릭하고 스냅샷(Snapshot)을 생성해 준다. 그러면 스냅샷(Snapshot) 메뉴로 가면 아래와 같이 보인다.

<img src="{{ site.baseurl }}/assets/develop_diary/encryption_of_storage_ami_in_aws_2.png" title="Encryption of Storage, AMI in AWS 2" class="post-image">

**암호화되지 않음**의 스냅샷(Snapshot)을 클릭하고 이미지 생성을 눌러서 AMI를 만들어 준다. 이제 AMI 메뉴로 가보자.

이제 내가 생성한 AMI가 있으면 눌러서 카피를 눌러주면... 아래 그림과 같이 뜨는데 암호화에 체크를 해준다.

<img src="{{ site.baseurl }}/assets/develop_diary/encryption_of_storage_ami_in_aws_3.png" title="Encryption of Storage, AMI in AWS 3" class="post-image">

그러면 암호화된 AMI가 생성된다. 이제 그 AMI를 이용해서 인스턴스를 새롭게 시작해 주면 된다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
