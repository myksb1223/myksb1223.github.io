---
layout: post
title:  "파이썬 리스트 - 빠르게 시작하기 위한 초보자 가이드 3편(Python List – A Beginners Guide To Get Started Quickly 3)"
date:   2018-08-06 03:21:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬리스트 PythonList List 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-list/#Add-Update-elements-to-a-list)은 파이썬(Python) 리스트(List)를 예제로 배우는 3번째 시간이다.

### Add/Update Elements To A List(리스트에서 요소 추가 및 업데이트)
문자열(String)과 튜플(Tuple)과 다르게 파이썬 리스트(Python list)는 변경가능한 객체이기에 각 인덱스(Index)의 값은 수정될 수 있다.

**\=** 을 사용해서 요소나 범위의 아이템을 수정할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_1.png" title="Python tutorial 12 Picture 1" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_2.png" title="Python tutorial 12 Picture 2" class="post-image">

또한 리스트를 업데이트를 위해 **리스트의 추가/업데이트** 섹션을 참조할 수 있다.

더욱이 **insert() 메소드**를 사용해서 원하는 위치에 한개의 아이템을 추가할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_3.png" title="Python tutorial 12 Picture 3" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_4.png" title="Python tutorial 12 Picture 4" class="post-image">

여러 아이템을 넣기 위해서는 자르기(slice) 할당(assignment)를 사용할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_5.png" title="Python tutorial 12 Picture 5" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_6.png" title="Python tutorial 12 Picture 6" class="post-image">

### Remove/Delete Elements From A List(리스트로부터 요소 지우기/삭제하기)
리스트로부터 하나 이상의 아이템을 지우기위해서는 'del' 키워드를 사용할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_7.png" title="Python tutorial 12 Picture 7" class="post-image">

너는 **remove() 메소드**를 사용해서 주어진 요소를 삭제할 수 있고 **pop() 메소드**로 요구된 인덱스(index)로 부터의 아이템을 꺼낼 수 있다.

**pop() 메소드**는 인덱스(index)를 지정하지 않으면 마지막 아이템을 반환하고 지운다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_8.png" title="Python tutorial 12 Picture 8" class="post-image">

마지막으로 요소의 자르기(slice)에 [ ]을 할당해서 아이템을 지울 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_9.png" title="Python tutorial 12 Picture 9" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_10.png" title="Python tutorial 12 Picture 10" class="post-image">

### Searching Elements In A List(리스트에서 요소 찾기)
파이썬(Python)에서 **'in' 연산자**를 사용하여 리스트(List)에 현재 아이템이 존재하는지를 확인할 수 있다.

    if value in theList:
        print("list contains", value)

파이썬(Python)의 **index() 메소드**를 사용하면 처음으로 매칭된 아이템의 위치를 알 수 있다.

    loc = theList.index(value)

index() 메소드는 리니어(Linear) 검색을 수행하고 처음 매칭된 아이템에 위치한 후 빠져나온다. 만약 결과없이 검색이 끝난다면 **ValueError** 예외처리를 발생시킨다.

    try:
        loc = theList.index(value)
    except ValueError:
        loc = -1 # no match

만약 모든 매칭된 아이템에 맞는 인덱스(index)를 얻고 싶다면 반복문 내의 **index()**에 두 개의 파라미터를 넘기면 된다. - 찾을 값, 시작 인덱스(index).

    loc = -1
    try:
        while 1:
            loc = theList.index(value, loc+1)
            print("match at", loc)
    except ValueError:
        pass

위의 코드의 나은 버전은 함수내에 검색 로직을 감싸고 반복문에서 그 함수를 부르는 것이다.

### See The Example Below.(예제를 보자.)

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_11.png" title="Python tutorial 12 Picture 11" class="post-image">

**결과 -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_12.png" title="Python tutorial 12 Picture 12" class="post-image">

파이썬 리스트(Python list)는 **min(List)**과 **max(List)**를 제공한다. 이를 사용하면 최소와 최대 값을 찾을 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_13.png" title="Python tutorial 12 Picture 13" class="post-image">

### Sorting A List In Python(파이썬에서 리스트 정렬)
### List’s Sort() Method
파이썬 리스트(Python list)는 위치한 각 요소를 정렬하기 위해서 **sort()** 메소드를 수행한다.

    theList.sort()

정렬 알고리즘은 결과를 저장할 임시 변수(예, new list)가 필요없는 만큼 더 효과적이다.

기본적으로 **sort()** 함수는 오름차순 정렬을 수행한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_14.png" title="Python tutorial 12 Picture 14" class="post-image">

만약 내림차순으로 정렬하고 싶다면 아래 예제를 참조하자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_15.png" title="Python tutorial 12 Picture 15" class="post-image">

### Built-In Sorted() Function(내장된 Sorted() 함수)

요소가 정렬된 리스트의 복사본을 위해서는 **내장된 sorted()** 함수를 사용할 수 있다.

    newList = sorted(theList)

기본적으로 오름차순이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_16.png" title="Python tutorial 12 Picture 16" class="post-image">

**"reverse" 플래그를 "True"**로 바꾸면 내림차순으로 정렬할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_17.png" title="Python tutorial 12 Picture 17" class="post-image">

### Python List Methods(파이썬 리스트 메소드)

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_18.png" title="Python tutorial 12 Picture 18" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-numbers/#list-methods)</figcaption>
</figure>

### Python List Built-In Functions(파이썬 내장 함수)

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_12_19.png" title="Python tutorial 12 Picture 19" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-numbers/#Python-list-built-in-functions)</figcaption>
</figure>
