---
layout: post
title:  "Rails 프로젝트에서 MySQL 떼어내는 방법(How to detach or remove MySQL in existing Rails project)"
date:   2018-07-31 05:52:00
author: Seungbeom Kim
categories: develop_diary
tags: 클래스업 ClassUp 레일즈 Rails 루비온레일즈 RubyOnRails MySQL
---

클래스업(ClassUp)은 크게 서버가 메인서버, 노트전용서버, 검색전용서버, 데이터베이스 서버 이렇게 4개로 나뉘어져 있다.

예전에는 **DynamoDB**를 사용하지 않고 MySQL만 사용했었는데 이제는 노트서버에 아예 MySQL을 사용하지 않는다. 하지만 이미 프로젝트 생성시에 MySQL을 부착시켜서... 무엇부터 바꿔야하는지 기억이 하나도 안났다...

일단, **[multi_db](https://github.com/schoefmann/multi_db)** gem부터 지웠다. 이는 MySQL에서 서버 분산할 때 master, slave 구조를 사용할 수 있게 해주는 라이브러리다.

안된다... mysql2 gem도 지워도 보았다... 안된다...

**database.yml**이라는 파일을 전부 주석처리 했는데도 안된다....ㅠㅠ

그래서 고민하다가 일단 급해서 해당 서버 자체의 **MySQL**을 키고 내부로 돌렸다.

완벽한 방법은 아니지만... 일단 급한 불부터 껐는데 조금 더 찾아봐야한다.

혹시, 방법 제대로 아시는 분은 댓글이나 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
