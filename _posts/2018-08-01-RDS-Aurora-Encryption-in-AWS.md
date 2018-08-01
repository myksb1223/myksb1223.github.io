---
layout: post
title:  "아마존 클라우드 서비스 RDS 오로라 암호화(RDS Aurora Encryption in AWS)"
date:   2018-08-01 19:56:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp RDS 데이터베이스 Database 오로라 Aurora  암호화 Encryption AWS 아마존클라우드서비스
---

이전 글인 [스토리지, AMI 암호화](https://myksb1223.github.io/develop_diary/2018/08/01/Encryption-of-Storage-AMI-in-AWS.html)와 마찬가지로 GDPR](https://www.eugdpr.org/)로 인해 보안에 좀 더 철저히 할 필요가 생겼다. AWS RDS인 오로라(Aurora)에도 적용해 보았다.

오로라(Aurora)는 쉽다. 복제본 생성을 누르면 아래 그림처럼 뜨는데

<img src="{{ site.baseurl }}/assets/develop_diary/rds_aurora_encryption_in_aws_1.png" title="RDS Aurora Encryption in AWS 1" class="post-image">

암호화를 시켜주고 복제본이 생성되었으면 기존 오로라(Aurora)에 연결 되었던 서버들을 복제본으로 연결을 변경하고 기존 데이터베이스를 삭제해주면 된다.

데이버베이스를 멈출 때는 꼭 웹, 앱서버를 먼저 종료하자. 즉 관련된 서비스를 먼저 종료하고 데이터베이스를 종료하자.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
