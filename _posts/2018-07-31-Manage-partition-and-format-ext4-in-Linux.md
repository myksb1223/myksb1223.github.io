---
layout: post
title:  "리눅스 파티션 및 ext4로 포맷하기(Manage partition and fomat ext4 in Linux"
date:   2018-07-31 05:12:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업, ClassUp, 리눅스, Linux, 리눅스 파티션, Linux Partition, 파티션, Partition, ext4, AWS, 아마존 클라우드 서비스
---

최근에 아마존(Amazon) 정책이 바뀌면서 서버를 다 바꿨다. 그리고 [GDPR](https://www.eugdpr.org/)때문에 AWS에서 지원하는 스토리지 암호화도 걸어야 했다.

스토리지를 추가를 반복적으로 많이 했지만 기록이 없어서 기록을 남겨볼까 한다.

일단, **fdisk -l**을 쉘에 치면 아래와 같은 결과가 나온다.

<img src="{{ site.baseurl }}/assets/python/linux_partion_format_ext4_1.png" title="Linux partion format ext4 1" class="post-image">

그리고 새롭게 추가된 디스크를 찾은 후 **fdisk /dev/xvdba** 라고 치면 아래와 같은 창이 뜨는데... 아래 그림처럼 순서대로 수행한다.

<img src="{{ site.baseurl }}/assets/python/linux_partion_format_ext4_2.png" title="Linux partion format ext4 2" class="post-image">

설명을 조금 하자면...
- **n**은 새로운 파티션을 만들겠다는 뜻이다.
- **p**는 주파티션으로 하겠다는 뜻이다.
- **1**은 그냥 번호인데 다 만들고 나면 나의 경우에는 xvdba1이라는 숫자가 붙을 것이다.
- **w**는 앞에 있는 정보를 저장하겠다는 것이다.

이제 **ext4** 파일시스템을 입혀보자.

<img src="{{ site.baseurl }}/assets/python/linux_partion_format_ext4_3.png" title="Linux partion format ext4 3" class="post-image">

그리고 **mount**로 마운팅시키면 되는데 나는 log파일 저장용으로 쓰기에,

    $ mount /dev/xvdba1 /log/

그리고 잊지 않아야 할 것이 **/etc/fstab**에 등록시키는 것이다.

<img src="{{ site.baseurl }}/assets/python/linux_partion_format_ext4_4.png" title="Linux partion format ext4 4" class="post-image">

맨 아래에 저렇게 추가해주면 재부팅시 자동으로 마운트 된다.
