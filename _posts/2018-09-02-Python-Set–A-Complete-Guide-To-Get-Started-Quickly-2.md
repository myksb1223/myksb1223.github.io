---
layout: post
title:  "파이썬 Set - 빠르게 시작하기 위한 완벽 가이드 2편(Python Set – A Complete Guide To Get Started Quickly 2)"
date:   2018-09-02 04:03:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬Set PythonSet Set 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-set/)은 파이썬(Python) Set을 예제로 배우는 2번째 시간이다.


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
<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_1.png" title="Python tutorial 14 Picture 1" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-set/#attachment_6734)</figcaption>
</figure>

**"\|"** 연산자는 set에서 합집합 연산을 수행하는 연산자이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_2.png" title="Python tutorial 14 Picture 2" class="post-image">

set의 길이를 계산하기 위해 **Len()**이라는 메소드를 사용한 적이 있다. 이 예제의 결과는 아래와 같다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_3.png" title="Python tutorial 14 Picture 3" class="post-image">

**union()** 메소드를 사용하면 비슷한 결과를 얻을 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_4.png" title="Python tutorial 14 Picture 4" class="post-image">

어떤 set(예, set A or B)이든 상관없이 **union()** 메소드를 적용할 수 있다; 결과는 같다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_5.png" title="Python tutorial 14 Picture 5" class="post-image">

#### Intersection Operation(교집합 연산)

setA와 setB의 교집합은 두 Set에서 공통적인 요소를 포함하는 set을 생성할 것이다.

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_6.png" title="Python tutorial 14 Picture 6" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-set/#attachment_6735)</figcaption>
</figure>

이 연산을 수행하기 위해 파이썬(Python)의 **"&"** 연산자를 사용할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_7.png" title="Python tutorial 14 Picture 7" class="post-image">

이 예제는 아래의 결과를 도출한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_8.png" title="Python tutorial 14 Picture 8" class="post-image">

그 대신에 이 연산을 수행하는 **intersection()** 메소드를 호출할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_9.png" title="Python tutorial 14 Picture 9" class="post-image">

이 예제는 아래의 결과를 도출한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_10.png" title="Python tutorial 14 Picture 10" class="post-image">

#### Difference Operation On The Set(Set에서의 차집합 연산)

두 Set에서 차집합 연산을 수행할 때(예, <setA - setB>), 결과는 오른쪽에는 존재하지 않지만 왼쪽에는 존재하는 요소들의 set일 것이다.

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_11.png" title="Python tutorial 14 Picture 11" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-set/#attachment_6736)</figcaption>
</figure>

똑같이 <setB - setA> 연산은 setA에는 속하지 않지만 setB에는 속하는 요소를 반환할 것이다.

이 연산의 결과를 위해서는 **minus (-)** 연산자를 사용할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_12.png" title="Python tutorial 14 Picture 12" class="post-image">

사용한 set 둘다 서로에게는 존재하지 않는 3개의 유일한 요소들 있다. 아래의 결과를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_13.png" title="Python tutorial 14 Picture 13" class="post-image">

다음 예제는 **differece()** 메소드를 사용한 같은 set 연산을 보여준다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_14.png" title="Python tutorial 14 Picture 14" class="post-image">

위 예제의 실행은 아래의 결과를 도출한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_15.png" title="Python tutorial 14 Picture 15" class="post-image">

#### Symmetric Difference Operation On The Set(Set에서의 대칭 차집합 연산)

두 set의 대칭 차집합은 setA, setB 둘 다에 포함되지 않는 요소들의 set을 생성한다.

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_16.png" title="Python tutorial 14 Picture 16" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-set/#attachment_6737)</figcaption>
</figure>

파이썬(Python)에서 **caret operation (^)**의 도움을 받아 이 연산을 수행할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_17.png" title="Python tutorial 14 Picture 17" class="post-image">

결과는 아래와 같다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_18.png" title="Python tutorial 14 Picture 18" class="post-image">

**symmetric_difference()** 메소드로도 연산 결과를 얻을 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_19.png" title="Python tutorial 14 Picture 19" class="post-image">

결과는 아래와 같다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_14_20.png" title="Python tutorial 14 Picture 20" class="post-image">

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
