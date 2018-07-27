---
layout: post
title:  "파이썬 데이터 타입 - 기초부터 심화까지 3편(Python Data Types - Learn From Basic To Advanced 3) Lists, Tuples"
date:   2018-07-28 04:44:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬데이터타입 PythonDataType List Tuple
---

[이번 단원](http://www.techbeamers.com/python-data-types-learn-basic-advanced/)은 파이썬 데이터 타입에 대한 튜토리얼 세 번째이다. 블로그를 이전한다고 이제서야 글을 다시 올린다.

**5. Lists**<br>
파이썬(Python) 리스트는 여러 타입의 객체를 순서가 있도록 저장하는 배열(array)과 비슷한 구조이다. 이는 매우 유연하고 고정된 크기를 가지지 않는다. 파이썬(Python)에서 리스트는 0부터 시작한다.
- 리스트는 다양한 타입의 객체의 아이템의 혼합된 집합이다. 예를 들면, 리스트 객체는 폴더에 파일로 저장할 수 있거나 회사의 직원 데이터 정보를 저장할 수 있다.

**Python List Syntax(파이썬 리스트 문법)**<br>
파이썬(Python)에서의 리스트는 []내에 ,로 요소를 배치시켜서 정의할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_1.PNG" title="Python tutorial 7 Picture 1" class="post-image">

- 리스트 객체는 변경할 수 있다. 파이썬(Python)은 리스트, 리스트의 요소를 할당시키는 것 뿐만 아니라 이미 지정된 리스트 메소드를 사용해서 수정하는 것을 허용한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_2.PNG" title="Python tutorial 7 Picture 2" class="post-image">

**Nesting Inside A List(리스트 내의 Nesting)**<br>
흥미롭게도 리스트는 다른 리스트를 포함할 수 있다. 그런 리스트를 네스티드(Nested) 리스트라고 한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_3.PNG" title="Python tutorial 7 Picture 3" class="post-image">

**Slicing A List(리스트 자르기)**<br>
리스트 역시 이전에 배운 문자열(String)처럼 자르는 것을 지원하는 파이썬(Python) 데이터 타입 중 하나다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_4.PNG" title="Python tutorial 7 Picture 4" class="post-image">

**6. Tuples**<br>
튜플은 ,로 구분된 파이썬(Python) 객체의 혼합된 집합이다. 이 말의 의미는 튜플 내에서 다른 데이터 타입의 객체가 같이 존재할 수 있다는 것이다. 튜플과 리스트는 아래 특성을 공유한다는 점에서 다소 비슷하다.
- 두 객체는 순서가 있다.
- 색인(indexing)과 되풀이(repetition)가 가능하다.
- 네스팅(Nesting)이 허용된다.
- 다른 타입의 값을 저장할 수 있다.

**Python Tuple Syntax(파이썬 튜플 문법)**<br>
()에 ,로 요소를 넣어서 튜플을 정의한다.

**Example - Define A Tuple**
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_5.PNG" title="Python tutorial 7 Picture 5" class="post-image">
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_6.PNG" title="Python tutorial 7 Picture 6" class="post-image">
**Example - Nested Tuples**
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_7.PNG" title="Python tutorial 7 Picture 7" class="post-image">
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_8.PNG" title="Python tutorial 7 Picture 8" class="post-image">
**Example - Repetition In Tuples**
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_9.PNG" title="Python tutorial 7 Picture 9" class="post-image">
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_10.PNG" title="Python tutorial 7 Picture 10" class="post-image">
**Example - Slicing In Tuples**
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_11.PNG" title="Python tutorial 7 Picture 11" class="post-image">
<img src="{{ site.baseurl }}/assets/python/python_tutorial_7_12.PNG" title="Python tutorial 7 Picture 12" class="post-image">

**Important note** - 위의 예제를 자를 때, "2"의 의미는 튜플에서는 세번째 요소가 시작임을 뜻한다 (자르기의 색인의 시작은 0이다.). "4"의 의미는 튜플에서는 다섯번째 요소를 뜻하지만 그 값은 제외한다.

**How Does A Tuple Differ From The List?(튜플은 리스트와 어떻게 다른가?)**<br>
튜플은 변경할 수 없다는 것으로 리스트와 조금 차이가 있다. 파이썬(Python)은 튜플이 생성된 후 수정되는 것을 허용하지 않는다. 나중에 값을 넣거나 뺄 수 없다. 대신에 파이썬(Python)은 요소의 수정된 순서를 가진 새로운 튜플을 만들 것을 기대한다.

**What If A Tuple Has Mutable Objects As Elements?(튜플이 요소로 변경가능한 객체가 있으면?)**<br>
여기 놀라운 것이 있다. 튜플을 변경하는 것은 금지다. 하지만 파이썬(Python)은 요소에 까지 강요하진 않는다. 이 말은 요소가 변경가능한 객체라면 수정할 수 있다는 것이다.

**Why Need Tuple As One Of The Python Data Types?(파이썬 데이터 타입 중 하나로 튜플은 왜 필요한가?)**<br>
여기 튜플을 지원하는 이유가 몇가지 있다.
- 파이썬(Python)은 함수로 부터 여러 값을 반환할 때 튜플을 사용한다.
- 튜플은 리스트보다 가볍다.
- 여러 객체를 저장할 단일 컨테이너로써의 역할한다.
- 딕셔너리(Dictionary)의 키(Key)로 사용할 수 있다.

이 부분은 매우 중요한 부분이다. 개발을 조금 해본 사람은 있겠지만 데이터가 중간에 변하지 않는 것이 매우 중요하다. 즉, 함수에서 값을 반환할 때 반환이 끝나서 역할이 끝날 때까지 값은 바뀌지 않아야 한다. 즉, `원본 객체`로서 역할이 가능하다. 딕셔너리(Dictionary)의 키(key)의 역할 역시 마찬가지이다.
