---
layout: post
title:  "파이썬 Set - 빠르게 시작하기 위한 초보자 가이드 3편(Python Set – A Beginners Guide To Get Started Quickly 3)"
date:   2018-09-02 04:45:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬Set PythonSet Set 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-set/)은 파이썬(Python) Set을 예제로 배우는 3번째 시간이다.


### Miscellaneous Set Operations(여러가지 Set 연산)

### How To Access Data Elements In A Set?(Set에서 데이터 요소 접근은 어떻게 하는가?)

set에서 요소에 직접적으로 접근할 수 없다. 하지만 요소들 전부를 불러올 수는 있다. Set에서 특정 아이템들의 리스트(list)를 찾는 loop가 필요하다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_15_1.png" title="Python tutorial 15 Picture 1" class="post-image">

위 코드를 수행하면 아래의 결과를 볼 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_15_2.png" title="Python tutorial 15 Picture 2" class="post-image">

### How To Perform Membership Test In A Set?(Set에서 요소 확인 수행은 어떻게 하는가?)

set에 특정 요소가 포함하고 있는지 없는지 확실히 확인할 수 있다. 이 목적을 위해서는 **"in"** 키워드를 사용하면 된다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_15_3.png" title="Python tutorial 15 Picture 3" class="post-image">

위의 코드를 수행하면 아래의 결과를 볼 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_15_4.png" title="Python tutorial 15 Picture 4" class="post-image">

### Frozon Set In Python(파이썬에서의 변경 불가능 Set)

이는 변경할 수 없고 한번 할당된 후엔 그 요소를 변경할 수 없는 유일한 형태의 set이다.

이는 모든 메소드와 연산자를 지원하지만 그 요소를 변경할 수 없다.

set은 변경가능하고 그래서 unhashable이다. 그래서 파이썬(Python) dictionary을 위한 키로 사용할 수 없다. 반면에 Frozon Set은 기본적으로 hashable이라 dictionary의 키로 사용할 수 있다.

아래의 함수로 frozon set을 생성할 수 있다.

    frozenset()

또한 아래의 파이썬(Python) 메소드는 Frozon set과 함께 사용할 수 있다.

    copy()
    difference()
    intersection()
    isdisjoint()
    issubset()
    issuperset()
    symmetric_difference()
    union()

추가와 삭제 메소드 연산은 Frozon set이 변경할 수 없기 때문에 사용할 수 없다.

아래의 예제는 standard와 frozen set 사이의 차이를 보여준다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_15_5.png" title="Python tutorial 15 Picture 5" class="post-image">

아래는 그 결과이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_15_6.png" title="Python tutorial 15 Picture 6" class="post-image">

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
