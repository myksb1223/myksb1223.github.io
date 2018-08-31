---
layout: post
title:  "파이썬 Set - 빠르게 시작하기 위한 초보자 가이드 2편(Python Set – A Beginners Guide To Get Started Quickly 2)"
date:   2018-09-01 03:21:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬Set PythonSet Set 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---


### Python Set - Native Operations(파이썬 Set - 기본 연산)

수학과 마찬가지로 set은 합집합(Union), 교집합(Intersection), 차집합(Difference), 여집합(Complement)과 같은 연산을 지원하는데 파이썬(Python)에서도 그렇다.

메소드뿐만 아니라 set 연산을 수행할 수 있게 하는 연산자도 있다.

그림 예제를 위해 다음 예제에서는 다음 두 set을 사용할 것이다.

    # We'll use the setA and setB for our illustration
    setA = {'a', 'e', 'i', 'o', 'u', 'g', 'h'}
    setB = {'a', 'e', 'z', 'b', 't', 'o', 'u'}

#### Union Operation(합집합 연산)

setA와 setB의 합집합은 두 Set으로부터 모든 요소를 합쳐 새로운 set을 만드는 것이다.

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_13.png" title="Python tutorial 13 Picture 13" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-set/#attachment_6734)</figcaption>
</figure>

**"|"** 연산자는 set에서 합집합 연산을 수행하는 연산자이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_14.png" title="Python tutorial 13 Picture 14" class="post-image">

set의 길이를 계산하기 위해 **Len()**이라는 메소드를 사용한 적이 있다. 이 예제의 결과는 아래와 같다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_15.png" title="Python tutorial 13 Picture 15" class="post-image">

**union()** 메소드를 사용하면 비슷한 결과를 얻을 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_16.png" title="Python tutorial 13 Picture 16" class="post-image">

어떤 set(예, set A or B)이든 상관없이 **union()** 메소드를 적용할 수 있다; 결과는 같다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_13_17.png" title="Python tutorial 13 Picture 17" class="post-image">



질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
