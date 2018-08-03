---
layout: post
title:  "Centos 커널 업데이트(Centos kernel update)"
date:   2018-08-01 17:21:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp Linux Centos 커널 Kernel 커널업데이트 UpdateKernel AWS 아마존클라우드서비스
sitemap :
  changefreq : daily
  priority : 1.0
---

아마존 [ENA(Elastic Network Adapter)](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/enhanced-networking-ena.html)활성을 위해 일단 커널 업데이트를 시도했지만 AWS에서는 Centos는 별로 사용하기를 추천하지 않는다.

왜냐하면 ENA를 활성화 해야하는 C5, C5d, F1, G3, H1, I3, m4.16xlarge, M5, M5d, P2, P3, R4, X1 타입의 인스턴스에서는 사용할 수 없기 때문이다.

아무튼 Centos 커널을 업데이트 하기위해서는... 일단. [ELRepo](http://elrepo.org/tiki/tiki-index.php)라는 곳을 이용해야 한다. 여기는 Rpm repository이다.

들어가면 Centos7이냐, 6이냐에 따라 시작하는 방법이 다른데 자신의 Centos 버전을 잘 확인하도록 하자.

    $ cat /etc/redhat-release
    // Or
    $ cat /etc/os-release

나는 **CentOS Linux release 7.5.1804 (Core)** 였다.

즉, 해당 사이트의 7버전을 따라 하면 된다. 미러 사이트를 이용하려면 **yum-plugin-fastestmirror**을 설치하면 된다. 일단 뭔가 하기전에 yum을 업데이트도 하도록 하자.

    $ yum update
    $ yum -y install yum-plugin-fastestmirror

위의 과정이 끝나면...

    $ yum repolist

라고 치면, 아래 그림과 같은 결과가 나온다.

<img src="{{ site.baseurl }}/assets/develop_diary/centos_kernel_update_1.png" title="Centos kernel update 1" class="post-image">

자, ELRepo 사이트 아래부분에 보면 **elrepo-kernel** 이라는 메뉴가 있다. 자세히 읽어보면 채널에는 long-term 지원 커널과 가장 안정적인 mainline커널이 있다. 나는 안정적인 버전 ml을 선택했다. 그리고 사용법을 보면 **/etc/yum.repos.d/elrepo.repo**에 추가하거나, **yum --enablerepo=elrepo-kernel**을 사용하라고 한다.

    $ yum --enablerepo=elrepo-kernel install kernel-ml

이렇게 치면 된다. 알아서 설치되고... 커널이 설치되는 것을 볼 수 있다.

아... 커널 업데이트 전에 내 커널이 뭔지는 알아야 한다.

    $ uname -msr

위 명령어로 확인하도록 하자.

이제 이 설치된 커널을 기본 커널로 사용해야 한다.

    $ sudo awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg

위 명령어를 치면 현재 설치된 커널을 나열해 준다. 당연히 내가 설치한 번호를 보고 선택하면 된다. 아래 그림 처럼 나는 0번이었기에...!

<img src="{{ site.baseurl }}/assets/develop_diary/centos_kernel_update_2.png" title="Centos kernel update 2" class="post-image">

    $ sudo grub2-set-default 0

자 선택을 했으면 해당 커널로 grub.cfg를 만들어야 하기에 아래와 같은 명령어를 수행한다.

    $ sudo grub2-mkconfig -o /boot/grub2/grub.cfg

<img src="{{ site.baseurl }}/assets/develop_diary/centos_kernel_update_3.png" title="Centos kernel update 3" class="post-image">

위 과정이 끝이 났다면!! 남은 것은 재부팅~~

    $ sudo reboot

이렇게 하면 완벽히 커널이 업데이트 되었다. 어려운 부분은 없다. 그냥 관련 사이트에서 시키는대로 커널 설치를 하고 grub 셋팅만 하면 끝이다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
