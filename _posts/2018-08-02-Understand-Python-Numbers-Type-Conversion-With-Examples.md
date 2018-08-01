---
layout: post
title:  "파이썬 넘버 & 타입 변환 예제로 이해하기(Understand Python Numbers & Type Conversion With Examples)"
date:   2018-08-02 03:52:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬넘버 PythonNumber Number 파이썬데이터타입 PythonDataType
---

[이번 단원](https://www.techbeamers.com/python-numbers/)은 파이썬(Python) 넘버(Number)를 예제로 배우는 시간이다.

파이썬(Python) 넘버(Number)는 네가지 데이터타입(plain integer, long integer, floating-point, complex number)의 그룹이다. 단순한 수학적 계산뿐만 아니라 양자 계산에서 복소수로 사용될 수 있다. 이 튜토리얼에서는 각각을 예제로 설명한다.

**파이썬(Python)에는 무슨 넘버**가 있을지 보도록 하자. 파이썬(Python)에서 다른 타입들과 같이 넘버(Number)도 객체이다. 이는 정수, 실수 또는 합성수를 저장할 수 있다. 파이썬(Python) 넘버(Number)는 변경 불가능한 객체이므로 값에 의한 변화는 새로운 객체 생성가 생성된다. 일반적으로 변수에 숫자 값을 할당하면 생성된 넘버(Number) 객체를 얻을 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_1.png" title="Python tutorial 9 Picture 1" class="post-image">

**Table of Content.(목차)**

- **Types of Numbers in Python(파이썬에서의 넘버 타입)**
- **Key Points to Remember(기억해야할 주 포인트)**
- **Type Conversion in Python(파이썬에서 타입 변경)**
- **External Number Classes in Python(파이썬에서 외부 넘버 클래스)**
- **Python Mathematics(파이썬 수학)**

### Python Numbers - Types Of Numeric Data(파이썬 넘버 - 숫자 데이터의 타입)
흥미롭게도 파이썬(Python) 2.x에서는 숫자를 표현하기 위한 네 가지(int, long, float, complex) 이미 지정된 타입이 있다. 나중에 파이썬(Python) 3.x에서는 long을 타입을 없애고 제한없는 길이를 가지는 int타입으로 확장했다.

### The Int Type
int 타입은 **파이썬에서의 데이터 타입**인 기본적은 정수을 표현한다. 파이썬(Python) 2.x에서의 기본 정수는 **sys.maxint**의 값이 최대 크기이다.

반면에 3.x에서의 int 타입은 제한없는 길이를 가지기 위해 승격되었고 그로인해 long은 사라졌다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_2.png" title="Python tutorial 9 Picture 2" class="post-image">

### The Long Type
제한길이 없는 숫자 넘버(Number). 파이썬(Python) 2.x의 마지막까지 정수(integer)는 오버플로우(overflow)가 허용되었고 실수(float) 타입으로 변경되었다. 이 int가 long으로 바뀌는 형태는 3.0부터 바뀌었다.

예제에서는 2.x 밖에 없어서 실제 3.x로 테스트 해보았다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_3.png" title="Python tutorial 9 Picture 3" class="post-image">
<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_4.png" title="Python tutorial 9 Picture 4" class="post-image">

### The Float Type
실수는 2진 부동 소수점을 표현한다. 표현식에서 실수 변수를 사용하면 자동적으로 인접한 int, long이 float로 바뀐다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_5.png" title="Python tutorial 9 Picture 5" class="post-image">

### The Complex Types
이 타입이 넘버(Number)는 실제와 상상의 부분을 가진다. 예를 들면 - 표현식 **(n1+n2j)**는 n1, n2 둘 다 상대적으로 실제와 상상의 부분을 나타내는 부동 소수점 수인 복합 타입을 표현한다.

한국어로 번역하면 뭔 말인지 모르겠지만... 아래 그림을 보면 바로 이해된다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_6.png" title="Python tutorial 9 Picture 6" class="post-image">

### Python Number - Key Points
1. 넘버(Number) 타입은 자동적으로 아래처럼 상위 캐스팅이 된다.
    **Int** -> **Long** -> **Float** -> **Complex**
2. 파이썬(Python) 3.x에서 integer는 어떤 길이든 가능하지만, float 타입은 오직 정확히 소수점 15자리까지 이다.
3. 일반적으로 10진법에 기반한 넘버(Number)로 작업한다. 하지만 때때로 2진법, 16진법, 8진법 등과 같은 다른 넘버(Number)의 사용이 필요하다.
    파이썬(Python)에서는 적절한 앞머리를 사용해서 이런 넘버(Number)를 처리할 수 있다. 아래 그림을 보자.

    <figure>
    <img src="{{ site.baseurl }}/assets/python/python_tutorial_9_7.png" title="Python tutorial 9 Picture 7" class="post-image">
    <figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-numbers/#key-points)</figcaption>
    </figure>

    <img src="{{ site.baseurl }}/assets/python/python_tutorial_9_8.png" title="Python tutorial 9 Picture 8" class="post-image">

4. 파이썬에서 넘버(Number)의 클래스 타입을 테스트하고 싶으면 isinstance() 함수를 사용하면 된다.

        isinstance(object, class)

    여기 예제가 있다.

    <img src="{{ site.baseurl }}/assets/python/python_tutorial_9_9.png" title="Python tutorial 9 Picture 9" class="post-image">
5. 표현식에서 데이터 타입을 섞어 사용한다면 모든 피연산자(결과 값)는 가장 복잡한 타입이 사용된 형태로 바뀔 것이다.
    <img src="{{ site.baseurl }}/assets/python/python_tutorial_9_10.png" title="Python tutorial 9 Picture 10" class="post-image">
6. 파이썬(python)에서 integer를 나눌 때 조심해야 한다.
    파이썬(Python) 2.x에서는 / 을 사용하면 몫을 결과로 반환할 것이다.

    <img src="{{ site.baseurl }}/assets/python/python_tutorial_9_11.png" title="Python tutorial 9 Picture 11" class="post-image">

    파이썬(Python) 3.x에서는 / 을 사용하면 실수 값 그대로를 반환할 것이다.

    <img src="{{ site.baseurl }}/assets/python/python_tutorial_9_12.png" title="Python tutorial 9 Picture 12" class="post-image">

7. // 은 몫을 반환하고 %은 나머지를 반환한다. 하지만 divmod() 함수를 이용하면 둘 다 얻을 수 있다.
    <img src="{{ site.baseurl }}/assets/python/python_tutorial_9_13.png" title="Python tutorial 9 Picture 13" class="post-image">

### Type Conversion (Casting) In Python(파이썬에서 타입변환, 캐스팅)
파이썬(Python)에서 어떤 숫자 데이터 타입을 다른 것으로 바꾸기 쉽다. 파이썬(Python) 용어로 이 과정을 coercion이라고 부른다.

만약 하나의 피연산자가 float라면 덧셈, 뺄셈같은 기본 연산은 암시적으로 integer를 float로 강제한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_14.png" title="Python tutorial 9 Picture 14" class="post-image">

위의 예제에서 덧셈할 때 정수 2가 실수 2.0으로 바뀌었고 결과 역시 소수점을 가진 수이다.

하지만 파이썬(Python) 명확하게 타입을 변경할 수 있는 int(), float(), complex()와 같은 수많은 이미 저장된 함수를 제시하고 있다. 이 함수들은 심지어 문자열(String)을 넘버(Number)로 바꿀 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_15.png" title="Python tutorial 9 Picture 15" class="post-image">

기억해야 할 것은 실수를 정수로 변환했다면 그 숫자는 잘린다.(예, 0에 가까운 정수) 즉, 이 말은 올림, 반올림이 아닌 그냥 **내림**한다는 것이다.

### External Classes To Handle Python Numbers(파이썬 넘버를 다루기 위한 외부 클래스)
위에서 읽었듯 파이썬(Python)의 이미 저장된 float 클래스는 소수점 15자리까지 정확도를 제어하는데 제한이 있다. 하지만 소수점 수에 대한 컴퓨터 수행에 전적으로 의존하기에 다른 제한 역시 있다. 예를 들면 아래 10진수 소수점 이슈를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_16.png" title="Python tutorial 9 Picture 16" class="post-image">

이런 종류의 이슈를 해결하기 위해 파이썬(Python)에서 10진 모듈을 사용한다.

**Python Decimal**
10진 모듈은 대부분의 사람들에게 친숙한 고정 및 소수점 연산을 제공한다. 소수점 15자리 까지의 정확도를 가진 소수점 넘버(Number)와 다르게 10진 모듈은 사용자에 의해 정의된 값을 채택한다. 이는 심지어 숫자에서 유효숫자 자리수까지 저장할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_17.png" title="Python tutorial 9 Picture 17" class="post-image">

**Output -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_18.png" title="Python tutorial 9 Picture 18" class="post-image">

**Python Fractions(파이썬 분수)**
파이썬(Python)은 분수 넘버(Number)를 다루기 위해 'fractions'라는 이름의 모듈을 가진다.

분수는 분자와 분모를 가지고 있다; 둘다 정수형 타입이다. 이 모듈은 유리수 산술 연산이 가능하다.

여기 fraction 타입의 객체를 생성하고 사용하는 간단한 예제가 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_19.png" title="Python tutorial 9 Picture 19" class="post-image">

**Output -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_20.png" title="Python tutorial 9 Picture 20" class="post-image">

### Python Mathematics(파이썬 수학)
파이썬(Python)은 간단한 수학적 계산을 다루기 위한 몇 개의 이미 저장된 함수가 있다.

예를 들면 - abs(), cmp(), max(), min(), round().

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_21.png" title="Python tutorial 9 Picture 21" class="post-image">

**Output -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_22.png" title="Python tutorial 9 Picture 22" class="post-image">

위의 메소드 제외하고도 파이썬(Python)에서는 수학 모듈을 사용할 수 있다. 아래처럼 사용할 수 있는 일반적인 함수를 제공한다.

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_23.png" title="Python tutorial 9 Picture 23" class="post-image">
<figcaption style="text-align: center;">출처 : [TechBeamers](https://www.techbeamers.com/python-numbers/#python-mathematics)</figcaption>
</figure>

여기 ceil() 함수를 사용한 몇 개의 예시가 있다.

**Example-1**
<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_24.png" title="Python tutorial 9 Picture 24" class="post-image">

**Output -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_25.png" title="Python tutorial 9 Picture 25" class="post-image">
**Example-2**
<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_26.png" title="Python tutorial 9 Picture 26" class="post-image">

**Output -**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_9_27.png" title="Python tutorial 9 Picture 27" class="post-image">

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
