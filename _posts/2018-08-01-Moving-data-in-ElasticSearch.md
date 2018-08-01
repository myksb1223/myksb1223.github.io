---
layout: post
title:  "엘라스틱 서치 데이터 옮기기(Moving data in ElasticSearch)"
date:   2018-08-01 18:55:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 엘라스틱서치 ElasticSearch 서버이전 데이터옮기기 MoveData AWS 아마존클라우드서비스 Centos ENA ElasticNetworkAdapter AMI
---

나의 가장 큰 문제점 중에 하나는 기록을 안한다는 것이었다. 여기에 기록을 함으로써 다음에는 같은 실수를 하지 않기를 바라고 있다.

일단, 이것도 같은 맥락인데 [GDPR](https://www.eugdpr.org/)과 아마존(Amazon) 정책에 의해서 서버를 옮겨야 했다. 그냥 인스턴스 타입만 바꾸면 좋았겠지만... 스토리지 암호화를 위해서는 기본 스토리지를 AMI로 만들고 그 AMI를 복사한 후, 그 복사된 AMI를 이용하여 인스턴스를 생성해야 한다. 엄청 복잡한데 이에 관한 내용이 궁금하면 AMI 암호화로 생성하기를 참조하길 바란다.

최신에 나온 인스턴스를 생성하기 위해서는 [ENA(Elastic Network Adapter)](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/enhanced-networking-ena.html)를 활성화 시켜야만 했다. 궁금하면 [ENA AMI를 만들기](https://myksb1223.github.io/develop_diary/2018/08/01/Make-ENS-ElasticNetwork-in-AWS.html)를 참조하길 바란다.

서버 이전 전까지 쓰던 OS는 Centos였다. 여기서부터 악몽이 시작되었다. Centos로 만든 ENA활성화된 AMI는 도대체 새로나온 인스턴스로 실행하면 작동을 안하는 것이었다. 이것저것 다 테스트하다가 결국 커널을 지원안한다고 판단하고 Amazon Linux로 새롭게 만들었다.

만드는 방법은 기본 root를 새롭게 만들고 원래 엘라스틱서치(ElasticSearch)의 데이터가 저장되는 볼륨을 그대로 두번째 디스크로 붙이면 된다. 다른 것 할 필요없이 원래 마운트(mount)되었던 폴더에다 마운트 시켜주면 된다.

나같은 경우는

    $ mount /dev/nvme1n1p1 /elasticsearch/

와 같이 마운트 시켰다. 바로 저장된 모든 파일이 복구된다.

엘라스틱서치(ElasticSearch)를 새롭게 설치하기 전에 일단 Java의 설치가 필요했다. Java의 설치과정은 [리눅스 자바 설치하기](https://myksb1223.github.io/develop_diary/2018/08/01/Install-Java-in-Linux.html)에 따로 포스팅하였다.

그리고 실행 전에 잊지 말아야 할 것이 몇 가지 있다.

    $ sudo sysctl -w vm.max_map_count=262144

그리고 /etc/security/limits.conf로 가서

    ec2-user hard nofile 65536
    ec2-user soft nofile 65536
    ec2-user hard nproc 65536
    ec2-user sort nproc 65536

설정해 준다.

마지막으로 .bash_prfile로 가서

    export ES_JAVA_OPTS="-Xms8g -Xmx8g"

을 추가한 후,

    $ . .bash_profile

로 설정을 적용해준다.

어쨋든 일단 엘라스틱서치(ElasticSearch)는 검색을 위해서만 사용하기 때문에 서버를 멈추어도 별로 상관이 없었다. 하지만 켜두었다. 여기서부터가 두번째 악몽의 시작이었다...

엘라스틱서치(ElasticSearch)은 같은 네트워크 내에 클러스터 이름이 같으면 샤딩(Sharding)을 시작한다...

그렇게 때문에 무조건 다른 인스턴스는 무조건 끄고 하길 추천한다.

어쨋든 결국 다시 인스턴스를 만들고 기존 인스턴스를 끄고 엘라스틱서치(ElasticSearch)를 시작하니 다시 잘 작동하였다. 이게 켜지고 셋팅하는데 시간이 오래걸려서... 또 안되는 것인가? 생각했는데... 아마 새롭게 키면 기존 데이터 관련해서 무엇인가 처리되면서 시작할 때까지 오래걸리는 느낌이다.

아마 이런 이유로 아까 첫 번째 시도가 실패한 것일 수도 있다. 어쨌든... 이렇게 엘라스틱서치(ElasticSearch)서버의 이전도 끝이 났다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
