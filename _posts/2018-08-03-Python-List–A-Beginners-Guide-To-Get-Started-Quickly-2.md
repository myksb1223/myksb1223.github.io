---
layout: post
title:  "파이썬 리스트 - 빠르게 시작하기 위한 초보자 가이드 2편(Python List – A Beginners Guide To Get Started Quickly 2)"
date:   2018-08-03 18:17:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬리스트 PythonList List 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-list/#Access-a-list-in-Python)은 파이썬(Python) 리스트(List)를 예제로 배우는 2번째 시간이다.

### Access A List In Python (Indexing The List)(파이선에서 리스트 접근(리스트 인덱싱))
파이썬(Python) 리스트(List)에서 요소를 인덱싱하고 접근하는 많은 방법을 찾을 수 있다.

### Using The Index Operator(인덱스 연산자 사용)
가장 간단한 것은 리스트(List)로부터 요소접근을 위한 **인덱스 연산자([ ])**를 사용하는 것이다. 리스트(List)는 첫 인덱스로 0부터 시작하기때문에 리스트(List) 사이즈가 10이라면 0부터 9까지의 인덱스를 가진다.

이 범위를 벗어난 아이템에 접근하려고 하면 **IndexError**라는 결과를 반환한다. 인덱스(Index)는 항상 integer이다. 다른 타입의 값을 사용하면 **TypeError**가 발생한다.

또한, 네스티드 리스트(Nested list)는 네스티드 인덱싱(Nested indexing)을 따른다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_1.png" title="Python tutorial 11 Picture 1" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_2.png" title="Python tutorial 11 Picture 2" class="post-image">

### Reverse Indexing(역 인덱싱)
파이썬(Python)은 시퀀스(Sequence) 데이터 타입에 대해 역(또는 음수) 인덱싱이 가능하다. 그래서 반대 순서로 파이썬 리스트(Python list) 인덱스(Index)를 위해서는 - 를 이용하여 인덱스를 설정하면 된다.

"-1"의 리스트(List) 인덱싱은 마지막 요소를 "-2"는 마지막 바로 전의 값을 반환할 것이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_3.png" title="Python tutorial 11 Picture 3" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_4.png" title="Python tutorial 11 Picture 4" class="post-image">

### Slicing A Python List(파이썬 리스트 자르기)
파이썬(Python)은 문자열(String), 튜플(Tuple) 같은 **다른 데이터 타입**의 객체를 조각하기 위한 놀라운 자르기 연산자를 가진다. 그리고 파이썬 리스트(Python list)에서도 똑같이 작용한다.

아래 보는 것과 같이 놀라운 문법이 있다.

    #The Python slicing operator syntax

    [start(optional):stop(optional):step(optional)]

    Start -> The starting index (By default included in the slice output)

    Stop  -> The closing index (Excluded from the slice output)

    Step  -> Tells how many values to exclude from the end. The default is 1.

integer의 리스트(List)를 보면서 고민해보자.

    >>> theList = [1, 2, 3, 4, 5, 6, 7, 8]

아래 나오는 예제로 수많은 다양한 리스트(List) 자르기를 테스트할 수 있다.

### Return The Three Elements, I.E. [3, 4, 5] From The List.(리스트로 부터 [3, 4, 5] 반환하기)

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_5.png" title="Python tutorial 11 Picture 5" class="post-image">

파이썬 리스트(Python list)는 0부터 시작하는 규칙을 가지기 때문에 첫 인덱스(Index)는 0이다.

시작 인덱스(Starting index)를 '2'로 했기 때문에 기본적으로 잘랐을 때 '3'을 포함한다.

마지막 인덱스(Ending index)를 '5'로 했다는 것은 4번째 인덱스(4th index)까지 포함함을 의미한다.

### Get The Slice Containing [3, 5] Without Changing Any Of The Starting Or Ending Index.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_6.png" title="Python tutorial 11 Picture 6" class="post-image">

예제에서 step 카운트를 1 증가시켜 중간 값을 제외시켰다. 즉, step을 어떻게 증가시키냐에 따라 몇 번째 요소마다 값을 포함시킬지 알 수 있다. 아래 그림은 내가 따로 테스트 해보았다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_7.png" title="Python tutorial 11 Picture 7" class="post-image">

### Slice From The Third Index To The Second Last Element In The List.(리스트에서 세 번째 인덱스부터 마지막 전 요소까지 자르기)
stop 부분에 음수 값을 사용할 수 있다. 이는 마지막 인덱스(Last index)로 부터 시작한다는 것을 의미한다.

'-1'과 같은 음수값의 stop은 '총길의 - 1'과 같은 의미다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_8.png" title="Python tutorial 11 Picture 8" class="post-image">

### Get The Slice From Start To The Second Index.(시작부터 두 번째까지 자르기)
자르기에서 start 값을 그냥 놔두면 0부터 시작하겠다는 의미이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_9.png" title="Python tutorial 11 Picture 9" class="post-image">

### Slice From The Second Index To The End.(두 번째부터 끝까지 자르기)
리스트(List)를 자를 때 stop 값을 쓰지않으면 리스트(List)의 끝까지 자르기를 수행하겠다는 것을 나타낸다. 이는 마지막 인덱스(Last index)로써의 리스트(List)의 길이를 쓰는 수고를 덜어준다. 리스트(List)가 짧으면 상관없지만 길 경우에 리스트(List) 크기를 구해서 넣어야하는 불편함이 있을 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_10.png" title="Python tutorial 11 Picture 10" class="post-image">

### Reverse A Python List Using The Slice Operator.(자르기 연산자를 상용한 파이썬 리스트 리버스하기)
특별한 자르기 문법인 **(::-1)**을 사용하면 어렵지 않게 결과를 얻을 수 있다. 하지만 이 방법은 in-place reversal보다 메모리를 많이 소모한다는 것을 기억해야한다.

이는 전체 리스트를 가지고 있기 위한 길고 충분한 공간을 요구하는 파이썬 리스트(Python list)의 얕은 복사(Shallow copy)를 생성한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_11.png" title="Python tutorial 11 Picture 11" class="post-image">

여기서 잠깐 멈추고 왜 두 번째 : 뒤에 '-1'을 사용했는지 이해할 필요가 있다. 이는 매번 인덱스를 '-1'만큼 증가시키고 역방향으로 이동하도록 지시한다. 쉽게 설명하면 아까 '2', '3'을 썼을 때, [3, 5], [3, 6]을 반환한 것처럼 '-1'을 사용하면 역으로 체크한다. 따라서 [8 ... 1]이 된다. '-2'를 사용하면 [8, 6, 4, 2]가 될 것이다. 다음 부분에 바로 나온다.

### Reverse A Python List But Skipping The Values At Odd Indices.(홀수 인덱스의 값을 제외한 파이썬 리스트 리버스)
여기, 이전 예제에서 배웠던 개념을 응용할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_12.png" title="Python tutorial 11 Picture 12" class="post-image">

'-2'를 반복자로 설정하면 매번 두 번째 요소를 제외시킬 수 있다.

### Create A Shallow Copy Of The Full Python List.(전체 파이썬 리스트의 얕은 복사 만들기)

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_13.png" title="Python tutorial 11 Picture 13" class="post-image">

모든 인덱스가 옵션이기 때문에 그것들을 다 뺄 수 있다. 이는 새로운 복제된 리스트(List)를 생성할 것이다.

### Get A Copy Of The List Containing Every Other Element.(모든 다른 요소를 포함하는 복제된 리스트 얻기)

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_14.png" title="Python tutorial 11 Picture 14" class="post-image">

### Iterate A List In Python(파이썬에서 리스트 반복)
파이썬(Python)은 리스트(List) 반복을 위해 기본적인 **for-in 반복**을 지원한다. for **구문**은 리스트(List)의 값 하나하나를 정말 쉽게 처리하게 해준다.

    for element in theList:
      print(element)

만약 인덱스와 요소를 둘다 동시에 사용하고 싶으면 **enumerate() 함수**를 호출하면 된다.

    for index, element in enumerate(theList):
      print(index, element)

만약 인덱스만 필요하다면 range()와 len() 메소드를 호출하면 된다.

    for index in range(len(theList)):
      print(index)

리스트 요소(List element)는 반복자 프로토콜(iterator protocol)을 지원한다. 반복자(iterator)를 일부러 생성하려면 **이미 저장된 함수 iter**를 호출한다.

    it = iter(theList)
    element = it.next() # fetch first value
    element = it.next() # fetch second value

아래 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_15.png" title="Python tutorial 11 Picture 15" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_11_16.png" title="Python tutorial 11 Picture 16" class="post-image">
