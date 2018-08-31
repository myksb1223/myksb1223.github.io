---
layout: post
title:  "파이썬 Set - 빠르게 시작하기 위한 초보자 가이드 1편(Python Set – A Beginners Guide To Get Started Quickly 1)"
date:   2018-09-01 01:29:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬Set PythonSet Set 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-set/)은 파이썬(Python) Set이 무엇이고, 이 것으로 무엇을 할 수 있고, 프로그램에서 이를 어떻게 사용하는지 알아보는 시간이다. 더욱이, Set을 어떻게 생성하고 그 안에 요소를 추가하고 삭제하고, 파이썬(Python)에서 Set을 이용해서 수행할 수 있는 모든 다른 연산들을 필수적으로 배울 것이다.

### Python Set – Introduction.
set은 수학에서 나온 용어이다. 이는 서로 다른 **데이터 타입(data typs)**의 요소를 저장할 수 있지만 그 요소들에 특정 정렬에 대한 순서가 없는 집합 타입이다.

파이썬(Python) Set은 아래의 속성을 가진다.

- 요소는 특정 정렬을 가지지 않고 더욱이 무작위 형태로 존재한다.
- 각각의 아이템은 Set에서 유니크하고 그래서 중복될 수 없다.
- 요소는 변경 불가능하고 한번 추가되면 변화를 줄 수 없다.
- set은 그 자체로 변경가능하고 아이템을 추가 및 삭제가 가능하다.

Set을 사용하면 합집합(Union), 교집합(Intersection), 대칭차집합(Symmetric Difference), 여집합(Complement)과 같은 몇몇의 수학적 연산을 수행할 수 있다.

### How To Instantiate A Set In Python?(파이썬에서 어떻게 Set을 초기화 할까?)

파이썬(Python) Set을 생성을 위해서 아래의 두가지 방법 중 어떤 것이든 적용할 수 있다.

i. 만약 고정된 set의 요소를 가지고 있다면, ','로 구분하여 그룹화 할 수 있고 '{}'내에 그 그룹을 포함시키면 된다.
ii. 또다른 방법은 런타임 중 요소를 추가하는데 사용될 수 있는 내장 함수인 **'set()'**을 호출하면 된다.

또한, 요소는 정수(integer), 실수(float), 튜플(tuple), 문자열(strings) 등과 같은 어떤 것도 될 수 있다. 단 한가지 예외라면 **list**나 **set**, **dictionary**와 같은 변할 수 있는 아이템은 저장할 수 없다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_1.png" title="Python tutorial 13 Picture 1" class="post-image">

위의 코드를 수행하면 아래의 결과가 나온다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_2.png" title="Python tutorial 13 Picture 2" class="post-image">

좀 더 명확히 이해하기 위해 아래 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_3.png" title="Python tutorial 13 Picture 3" class="post-image">

위의 코드를 수행 후 결과를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_4.png" title="Python tutorial 13 Picture 4" class="post-image">

하나 더 테스트 해보자. 빈 파이썬(Python) Set을 생성하기 위한 노력은 하지 않을 것이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_5.png" title="Python tutorial 13 Picture 5" class="post-image">

이는 위의 코드의 설명이다.

첫 구문은 set을 생성하는 대신에 dictionary 객체를 생성하는 결과를 낼 것이다. 단지 '{}'를 사용하면 결과로 'Set' 예상할 수 없다.

다음의 non-print 구문에서는 **set()** 함수를 사용했지만 어떤 인자도 전달하지 않았다. 이는 결과적으로 빈 Set 객체를 반환할 것이다.

아래의 마지막 예제의 결과를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_6.png" title="Python tutorial 13 Picture 6" class="post-image">

### How To Modify A Set In Python?(파이썬에서 Set은 어떻게 수정할까?)

파이썬(Python) Set은 변경가능한 객체이다. 하지만, 어떤 인덱싱(indexing)도 사용하지 않고 어떠한 정렬도 없다.

이는 또한 인덱스(index)나 조각(slicing)으로 접근하여 그 요소를 변경할 수 없음을 뜻한다.

**update()** 메소드는 심지어 인자로 tuple, list, string 그리고 다른 set을 가질 수 있다. 하지만, 중복된 인자는 자동적으로 제외될 것이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_7.png" title="Python tutorial 13 Picture 7" class="post-image">

위의 예제에서, 첫 줄은 set은 인덱싱(indexing)을 허용하지 않는다는 것을 증명하고 있다. **try-except** 블럭을 코드 내에 넣어서 에러를 체크하고 출력하고 남은 부분을 수행하게끔 하고 있다.

다음 단락의 예제에서 동작에서의 Set의 **add()**와 **update()** 메소드를 볼 수 있다.

이제, 위의 파이썬(Python) Set 예제의 결과를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_8.png" title="Python tutorial 13 Picture 8" class="post-image">

### How To Delete Single Or Multiple Elements From A Set In Python?(파이썬에서 단일 또는 다중 요소를 Set으로 부터 어떻게 삭제할까?)

Set에서 요소를 지우기 위해서는 아래의 Set 메소드를 사용하면 된다.

i. **Discard() method**
ii. **Remove() method**

이 두 메소드의 연산 방법에 약간의 차이가 있다. **discard()** 메소드는 지우려는 아이템이 set에 존재하지 않아도 어떤 에러도 발생시키지 않는다.

반면에, 이 경우 **remove()** 메소드는 **"KeyError"**라는 에러를 발생시킨다.

좀 더 명확하게 하기위해 아래 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_9.png" title="Python tutorial 13 Picture 9" class="post-image">

위의 코드는 아래의 결과를 생성한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_10.png" title="Python tutorial 13 Picture 10" class="post-image">

이런 메소드와는 별개로 지금까지 봐왔듯이 요소를 제거하는 **pop()** 메소드가 있다.

또한, Set은 인덱싱(indexing)를 사용하지 않기에 어떤 아이템이 지워질지 확신할 수 없다. 이는 랜덤하게 요소하나를 골라 제거한다.

**clear()**라는 Set 객체로 부터 모든 요소를 제거하는 메소드 또한 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_11.png" title="Python tutorial 13 Picture 11" class="post-image">

이 예제는 아래의 결과를 도출한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_12.png" title="Python tutorial 13 Picture 12" class="post-image">

다음 부분에선 파이썬 기본 연산에 대해 알아본다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
