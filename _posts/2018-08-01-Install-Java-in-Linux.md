---
layout: post
title:  "리눅스 자바 설치하기(Install Java in Linux)"
date:   2018-08-01 06:41:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 자바 Java 리눅스 Linux 엘라스틱서치 ElasticSearch
---

엘라스틱서치(ElasticSearch)를 사용하기 위해서는 Java의 설치가 필수적이다.

가장 최신 버전을 [오라클(Oracle)](http://www.oracle.com/technetwork/java/javase/downloads)로 가서 다운받는다. JDK로 받아야 한다.

    $ cd /opt/
    $ sudo tar xvfz jdk-10.0.2_linux-x64_bin.tar.gz //다운 받은 jdk 압출 해제.
    $ cd jdk-10.0.2/
    $ sudo alternatives --install /usr/bin/java java /opt/jdk-10.0.2/bin/java 2 //설치된 자바가 없으면 무조건 1이다.
    $ sudo alternatives --config java
    $ sudo alternatives --install /usr/bin/jar jar /opt/jdk-10.0.2/bin/jar 2
    $ sudo alternatives --set jar /opt/jdk-10.0.2/bin/jar
    $ sudo alternatives --set javac /opt/jdk-10.0.2/bin/javac
    $ java -version
    java version "10.0.2" 2018-07-17
    Java(TM) SE Runtime Environment 18.3 (build 10.0.2+13)
    Java HotSpot(TM) 64-Bit Server VM 18.3 (build 10.0.2+13, mixed mode)

아래 그림과 같다.

<img src="{{ site.baseurl }}/assets/develop_diary/install_java_in_linux_1.png" title="Install Java in Linux 1" class="post-image">

마지막에는 .bash_profile에 아래 처럼 작성해 준다.

    export JAVA_HOME=/opt/jdk-10.0.2
    export JRE_HOME=/opt/jdk-10.0.2/jre
    export PATH=$PATH:/opt/jdk-10.0.2/bin:/opt/jdk-10.0.2/jre/bin


    $ . .bash_profile

로 설정을 적용해준다.

<img src="{{ site.baseurl }}/assets/develop_diary/install_java_in_linux_2.png" title="Install Java in Linux 2" class="post-image">

그럼 끝~

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
