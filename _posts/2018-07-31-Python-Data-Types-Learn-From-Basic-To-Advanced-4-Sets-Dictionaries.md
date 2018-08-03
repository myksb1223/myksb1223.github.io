---
layout: post
title:  "파이썬 데이터 타입 - 기초부터 심화까지 4편(Python Data Types - Learn From Basic To Advanced 4) Sets, Dictionaries"
date:   2018-07-31 03:12:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬데이터타입 PythonDataType Set Dictionary
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](http://www.techbeamers.com/python-data-types-learn-basic-advanced/)은 파이썬 데이터 타입에 대한 튜토리얼 네 번째이다.

**7. Sets**<br>
파이썬(Python) 데이터 타입 중에서 Set은 union, intersection, symmetric difference와 같은 수학적 연산을 지원한다.

Set은 고유하고 변결할 수 없는 정렬되지 않은 집합이다. ,로 구별된 요소들을 가지는 {}을 이용하여 정의한다.

Set은 수학의 `집합`으로부터 파생되었기 때문에 같은 요소가 여러번 나올 수 없다.

**Why Need A Set?(Set이 필요한 이유는?)**<br>
Set 데이터 타입은 리스트보다 명확한 장점을 가진다. 컨테이너(container)가 특정한 요소를 가지고 있는지 아닌지 확인하는 엄청나게 최적화된 메소드(method)를 수행한다. 여기에 사용되는 메커니즘(machanism)은 해쉬 테이블(hash table)로 알려진 데이터 구조를 기본으로 한다.

**Creaet A Set(Set 생성하기)**<br>
Set을 생성하기 위해서 순서나 어떤 반복 가능한 객체와 함께 **set()**이라는 이미 저장된 함수를 호출한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_1.png" title="Python tutorial 8 Picture 1" class="post-image">

다른 간단한 방법은 {}안에 특정 요소들을 포함시키는 것이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_2.png" title="Python tutorial 8 Picture 2" class="post-image">

**Frozen Set(고정 Set)**<br>
고정 Set은 원래 Set의 처리된 형태이다. 그것은 변경할 수 없고 흐름에서 사용되는 고정 Set를 변경하지 않고 실행되는 오직 메소드와 연산자만 지원한다.

    # An empty frozenset
    >>> frozenset()
    frozenset()
    >>> type(cities)
    <class 'frozenset'>

이제 일반 Set과 고정 Set의 차이를 강조하는 전체 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_3.png" title="Python tutorial 8 Picture 3" class="post-image">
<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_4.png" title="Python tutorial 8 Picture 4" class="post-image">

**8. Dictionaries**<br>
파이썬(Python)에서 딕셔너리(Dictionary)는 키-밸류 쌍(key-value pair)이다. 파이썬(Python)에서 키가 값에 맵핑되는 이미 제공된 맵핑(mapping) 타입이다. 이 키-밸류 쌍(key-value pair)는 직관적으로 데이터를 저장하는 방법을 제공한다.

**Why Need A Dictionary?(왜 딕셔너리가 필요한가?)**<br>
딕셔너리(Dictionary)는 효과적으인 큰 데이터의 저장의 문제를 해결한다. 파이썬(Python)은 데이터 검색에 엄청나게 최적화된 딕셔너리(Dictionary) 객체를 가지고 있다.

**Creating A Dictionary(딕셔너리 생성하기)**<br>
딕셔너리(Dictionary)를 생성하기 위한 파이썬(Python) 구문은 각 요소가 키와 값의 쌍으로 나타나는 {}을 사용한다. 키와 값은 파이썬(Python)의 어떤 데이터 타입이든 될 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_5.png" title="Python tutorial 8 Picture 5" class="post-image">

**Accessing Dictionaries Elements With Keys(키로 딕셔너리 요소 접근하기)**
딕셔너리(Dictionary)는 데이터베이스(database)처럼 동작한다. 여기, 리스트에서 처럼 특정한 인덱스(index)의 값을 가져오기 위해서 숫자 값을 사용하지 않는다. 대신에 키로 대체하고 그 키에 해당되는 값을 가져오도록 사용한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_6.png" title="Python tutorial 8 Picture 6" class="post-image">

**Dictionaries Methods To Access Elements(요소에 접근하기 위한 딕셔너리 메소드)**
파이썬(Python)은 아래 3개의 이미 저장된 딕셔너리(Dictionary) 함수를 가지고 있다.

- **keys()** - 딕셔너리(Dictionary)로 부터 키만 분리한다.
- **values()** - 딕셔너리(Dictionary)로 부터 값만 분리한다.
- **items()** - 키-밸류 쌍(key-value pair)의 형태의 리스트(List)로 반환한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_7.png" title="Python tutorial 8 Picture 7" class="post-image">

**Modifying A Dictionary - Add/Update/Delete(딕셔너리 수정 - 추가/수정/삭제)**
딕셔너리(Dictionary) 객체는 변경이 가능하기 때문에 딕셔너리(Dictionary) 객체에 대한 추가(add), 수정(update), 삭제(delete) 수행을 할 수 있다.

딕셔너리(Dictionary)를 어떻게 수정하는지 더 명확하게 이해하기 위해 아래 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_8_8.png" title="Python tutorial 8 Picture 8" class="post-image">

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
