---
layout: post
title:  "아마존 클라우드 서비스에서 ENA(ElasticNetworkAdapter) 만들기(Make ENA(ElasticNetworkAdapter) in AWS)"
date:   2018-08-01 18:11:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp ENA ElasticNetworkAdapter AWS 아마존클라우드서비스 AMI
sitemap :
  changefreq : daily
  priority : 1.0
---

C5, C5d, F1, G3, H1, I3, m4.16xlarge, M5, M5d, P2, P3, R4, X1 타입의 인스턴스를 사용하기 위해서는 [ENA(ElasticNetworkAdapter)](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/enhanced-networking-ena.html)를 활성화된 AMI를 사용해야 한다. Centos는 해도 괜찮지만 ENA활성화 했을 때 M5로 인스턴트 생성 시, 작동하지 않았다. 하지만 이 과정을 사용하는 사람이 있을 것 같아서 기록을 남긴다.

Centos에서 ena 모듈이 설치하기 위해서는 일단 [커널 업데이트](https://myksb1223.github.io/develop_diary/2018/08/01/Centos-kernel-update.html)가 필요하다. 해당 링크에서 확인하면 된다.

Centos에서는 [다른 Linux 배포판에서 ENA를 사용하여 향상된 네트워킹 기능 활성화
](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/enhanced-networking-ena.html#enhanced-networking-ena-linux) 부분을 참조해야 한다.

자세한 [설치 방법](https://github.com/amzn/amzn-drivers/blob/master/kernel/linux/rpm/README-rpm.txt)은 링크를 참조하도록 하고, 설치가 안된 것 같은 결과가 나오는데

    $ modinfo ena

라고 치면 결과가 나온다.

그리고 이제 dkms를 설치하면 되는데 많은 방식 중 제일 쉬운 방법은 아래와 같다.

    $ sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    $ sudo yum -y install dkms

dkms가 설치 완료되었으면 이제 ENA사이트의 5-7단계를 수행하면 된다.

    $ sudo dkms add -m amzn-drivers -v 1.0.0
    $ sudo dkms build -m amzn-drivers -v 1.0.0
    Error! echo
    Your kernel headers for kernel 4.17.11-1.el7.elrepo.x86_64 cannot be found at
    /lib/modules/4.17.11-1.el7.elrepo.x86_64/build or /lib/modules/4.17.11-1.el7.elrepo.x86_64/source.

위와 같이 에러가 발생했다. 해결법은 아래와 같다.

    $ wget http://elrepo.org/linux/kernel/el7/x86_64/RPMS/kernel-ml-devel-4.17.11-1.el7.elrepo.x86_64.rpm
    $ sudo rpm -Uvh kernel-ml-devel-4.17.11-1.el7.elrepo.x86_64.rpm

이렇게 한 후 사이트의 나머지 부분을 따라하면 해결이 된다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
