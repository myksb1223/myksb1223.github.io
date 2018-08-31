---
layout: post
title:  "파이썬 리스트 - 빠르게 시작하기 위한 초보자 가이드 1편(Python List – A Beginners Guide To Get Started Quickly 1)"
date:   2018-08-03 03:58:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬리스트 PythonList List 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-list/)은 파이썬(Python) 리스트(List)를 예제로 배우는 시간이다. 길어서 나눠서 번역 및 글을 작성할 예정이다.

이번에는 **파이썬(Python) 리스트(List)**가 무엇이고 프로그램에서 어떻게 사용될 수 있는지를 배울 것이다. 또한 요소를 자르기, 찾기, 추가하기, 삭제하기와 같은 다양한 리스트 연산에 대해 배울 것이다.

순서가 있는 것으로 알려진 파이썬(Python)에서의 리스트(List)는 정렬된 객체의 집합이다. 어떤 타입의 데이터도 가질 수 있다: 넘버(Number), 알파벳, 문자열(String), 심지어 네스티드(Nested) 리스트(List) 또한 추가할 수 있다.

모든 요소는 리스트(List)에서 어떤 위치(예, 인덱스)를 가진다. 그 인덱스(Index)는 나중에 특정 아이템을 위치시키는데 사용할 수 있다. 인덱스(Index)의 처음은 0부터 시작한다.

파이썬(Python)에서는 **문자열(String)**과 다르게 리스트는 변경 가능하다. 이는 요소의 대치, 추가, 삭제가 가능하다는 것을 암시한다.

### Python List – Table Of Content.
- #### Create a List in Python(파이썬에서 리스트 만들기)
  - **Using [ ] to Create a List([ ]을 사용해서 리스트 만들기)**
  - **With List() Method to Create a List(List()메소드를 사용해서 리스트 만들기)**
  - **Using List Comprehension(리스트 컴프리헨션의 사용)**
  - **Creating a Multi-Dimesional List(다차원 리스트 만들기)**
  - **List Extension(리스트 확장)**
- #### Access a List in Python (Indexing)(파이선에서 리스트 접근하기(인덱싱))
  - **Using the Index Operator(인덱스 연산자 사용하기)**
  - **Reverse Indexing(역 인덱싱)**
- #### Slicing Python Lists(파이썬 리스트 자르기)
- #### Iterate a List in Python(파이썬에서 리스트 반복)
- #### Add/Update Elements in a List(리스트에서 요소 추가/업데이트)
- #### Remove/Delete Elements from a List(리스트로부터 요소 제거/삭제)
- #### Searching Elements in a List(리스트에서 요소 찾기)
- #### Sorting a List in Python(파이썬에서 리스트 정렬)
  - **List's Sort() Method(리스트의 Sort() 메소드)**
  - **Built-in Sorted() Function(이지 저장된 Sorted() 함수)**
- #### Essential List Methods(필수적인 리스트 메소드)
- #### Built-in List Functions(내장된 리스트 함수)

**확인 사항 -** 예제에서 제공되는 샘플 코드를 테스트하고 수행하기 위해서는 **파이썬(Python) IDE**(IDLE, PyDev)뿐만아니라 파이썬(Python) 쉘(Shell)이 필요하다.

### Create A List In Python(파이썬에서 리스트 만들기)
파이썬(Python)에서 리스트(List)를 만드는 다양한 방법이 있다.

### Using [ ] To Create A List([ ]을 사용해서 리스트 만들기)
첫 번째 방법은 [ ]안에 ,로 구분된 모든 요소를 위치시키는 것이다.

    L1 = [] # empty list
    L2 = [expression, ...]

리스트(List)는 몇 개의 요소든 가질 수 있고 각각은 다른 타입(integer, float, string 등)을 가질 수 있다.

    # blank list
    L1 = []

    # list of integers
    L2 = [10, 20, 30]

    # List of heterogenous data types
    L3 = [1, "Hello", 3.4]

### Python List – List() Method To Create A List(파이썬 리스트 - List() 메소드로 리스트 만들기)
파이썬(Python)은 내장된 **list()** 메소드를 포함하고 있다.

이는 시퀀스(Sequence)나 튜플(Tuple)을 요소로 받아들이고 이를 파이썬(Python) 리스트(List)로 변경한다.

예제로 어떤 파라미터도 없이 리스트를 생성을 시작해 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_1.png" title="Python tutorial 10 Picture 1" class="post-image">

**주의-** **len() 함수**는 리스트의 크기를 반환한다.

**list() 함수**의 인풋 파라미터로 기본적이거나 네스티드(Nested) 시퀀스(Sequence)를 넣을 수 있다.

    theList = list([n1, n2, ...] or [n1, n2, [x1, x2, ...]])

아래 파이썬(Python) 예제를 수행해보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_2.png" title="Python tutorial 10 Picture 2" class="post-image">

### Using List Comprehension – An Intuitive Way To Create Lists(리스트 컴프리헨션 사용 - 직관적 방법으로 리스트 만들기)
파이썬(Python)은 "**List Comprehension**"이라는 개념을 지원한다. 이는 완벽하게 자연스럽고 쉬운 방법으로 리스트를 구조화하는데 도움을 준다.

list comprehension은 다음의 문법을 따른다:

    #Syntax - How to use List Comprehension

    theList = [expression(iter) for iter in oldList if filter(iter)]

이는 for-in 구문과 0개 이상의 if 구문이 뒤따르는 표현식을 그룹화한 [ ]을 가진다.

첫 번째 간단한 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_3.png" title="Python tutorial 10 Picture 3" class="post-image">

간단한 리스트(List)를 시작하기 쉽지 않았는가?

여기에 리스트(List) 만들기에서 **List Comprehension** 결과의 좀더 복잡한 예제가 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_4.png" title="Python tutorial 10 Picture 4" class="post-image">

List Comprehension은 오직 특정한 조건을 만족하는 멤버들을 리스트(List)에 추가하기 위해 심지어 if 구문도 허용한다:

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_5.png" title="Python tutorial 10 Picture 5" class="post-image">

이제 list comprehension에서 if문이 어떻게 작용하는지 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_6.png" title="Python tutorial 10 Picture 6" class="post-image">

또다른 복잡한 예제는 List Comprehension 문법으로 홀수 달을 포함한 리스트(List)를 만드는 것이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_7.png" title="Python tutorial 10 Picture 7" class="post-image">

### Creating A Multi-Dimensional List(다차원 리스트 만들기)
각 요소에 대한 최초 값을 정함으로써 크기가 이미 정해진 시퀀스(Sequence)를 만들 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_8.png" title="Python tutorial 10 Picture 8" class="post-image">

위의 개념을 사용하면 이차원 리스트(List)도 만들 수 있다.

    two_dim_list = [ [0]*3 ] *3

위의 구문은 작동하지만 파이썬(Python)은 분리된 객체들을 생성하는 대신에 하위리스트로서의 참조값(reference)만 오직 생성한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_9.png" title="Python tutorial 10 Picture 9" class="post-image">

첫 행의 세 번째 아이템의 값을 변경했다. 하지만 다른 행의 같은 컴럼의 값들도 전부 영향을 받았다.

그러므로 위의 이슈를 해결하기 위해서 list comprehension을 사용해야 한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_10.png" title="Python tutorial 10 Picture 10" class="post-image">

### List Extension(리스트 확장)
파이썬(Python)은 리스트(List)의 크기를 변경할 수 있는 많은 방법을 허용한다. 단순히 두개나 그 이상을 서로 더하면 된다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_11.png" title="Python tutorial 10 Picture 11" class="post-image">

아니면 **extend() 메소드**를 사용해서 리스트(List)를 합칠 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_12.png" title="Python tutorial 10 Picture 12" class="post-image">

아니면 **append() 메소드**를 호출해서 리스트(List)에 값을 추가할 수 있다. 아래 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_10_13.png" title="Python tutorial 10 Picture 13" class="post-image">

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
