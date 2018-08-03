---
layout: post
title:  "파이썬 데이터 타입 - 기초부터 심화까지 1편(Python Data Types - Learn From Basic To Advanced 1) Boolean, Numbers"
date:   2018-07-25 00:41:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬데이터타입 PythonDataType Boolean Number
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](http://www.techbeamers.com/python-data-types-learn-basic-advanced/)은 파이썬 데이터 타입에 대한 튜토리얼이다. 너무 길어서 데이터 타입을 나눠서 올리려고 한다.

일반적으로 데이터 타입은 프로그램이 적절하게 사용할 수 있도록 해당 데이터의 하위나 상위 범위를설정한 포맷을 의미한다. 하지만 파이썬(Python) 데이터 타입은 이보다 더 넓은 의미를 가지고 있다. 파이썬(Python)에서는 변수 선언시 정확하게 데이터 타입의 선언할 필요가 없다. 이 형태는 동적 타이핑(dynamic typing)으로 알려져 있다.

파이썬(Python)은 실행시 그 구문으로 부터 즉시 타입을 결정한다. 예를 들면 -  쌍 따옴표로 선언된 문자열(string) 값, []으로 선언된 리스트(list), {}으로 선언된 딕셔너리(dictionary). 또한 소수점이 없는 숫자는 정수(integer)로 선언될 것이고 소수점이 있는 숫자는 실수(float)로 선언될 것이다.

파이썬(Python)에서 변수, 함수, 모듈을 포함하고 있는 것 전부는 객체이다. 또다른 흥미로운 사실은 그 변수들은 타입이 아니라 파이썬(Python)에서의 레이블일 뿐이다. 그것은 타입과 연관된 값이다. 더 나아가 같은 변수인 그 레이블은 다른 파이썬(Python) 데이터 타입을 참조할 수 있다.

지금까지 말이 어려운데... 다시 이야기해보면 일반적으로 변수 선언은 아래와 같고 그 값은 정해진 데이터 타입과 같은 값만 할당할 수 있다. 아래 코드를 보자.

```java
int x = 1;
x = 3;
x = "Learn Python"; (x)
```

그런데 파이썬은 데이터 타입이 정해지는 것이 아니라 단지 레이블, 즉 명명하는 것 뿐이라고 한다. 그러면 아래와 같은 것이 가능하다.

```python
x = 1;
x = 3;
x = "Learn Python"; (o)
```

즉, x에 데이터 타입이 정해진 것이 아니라 그냥 명명한 것이기 때문에 할당되는 값에 따라 명명만 바꾸면 된다.

이 부분은 약간 혼동이 올 수 있어 따로 이해한 바로 정리해 보았다. 다시 본문으로 돌아가면...

아래는 파이썬(Python)에서 흔히 사용되는 중요한 데이터 타입이다. 우리는 예제를 통해 각각에 대해 알아볼 것이다.

1. Boolean
2. Numbers
3. Strings
4. Bytes
5. Lists
6. Tuples
7. Sets
8. Dictionaries

### Python Data Types From Basic To Advanced(기초부터 심화까지의 파이썬 데이터 타입)<br>
__1. Boolean__<br>
boolean은 파이썬(Python)을 포함하여 거의 모든 언어에서 가지고 있는 데이터 타입의 한 종류이다. 파이썬(Python)에서 Boolean은 __True, False__ 두 값을 가질 수 있다. 이 두 값은 상수이고 할당이나 boolean 값을 비교할 때 사용될 수 있다. 아래 주어진 간단한 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_1.png" title="Python tutorial 5 Picture 1" class="post-image">

파이썬(Python)에서 boolean 조건을 만들 때, 명확한 비교를 생략할 수 있다. 그리고 같은 결과를 얻을 것이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_2.png" title="Python tutorial 5 Picture 2" class="post-image">

위의 코드는 이전 것과 같은 결과를 도출할 것이다. 왜냐하면

```python
if condition:
```

구문과 아래구문이 같기 때문이다.

```python
if condition == True:
```

다음은 boolean 결과를 도출할 수있는 파이썬(Python)에서의 표현식을 보자.

__For example -__ 조건 블럭에서의 표현식은 boolean 값을 도출한다. 파이썬(Python)은 표현식을 평가하기위해 boolean 문맥(context)을 생성합니다.

표현식이 무엇이든 파이썬(Python)은 그 것의 참, 거짓 값을 결정하기 위해 boolean 문맥(context)을 사용할 것입니다. 파이썬(Python)은 많은 데이터 타입을 가지고 있기에 그것들은 boolean 문맥(context)에서 결과를 도출하기 위해 해당 데이터 타입이 가진 규칙을 수행할 것입니다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_3.png" title="Python tutorial 5 Picture 3" class="post-image">

계속 context라는 말이 나오는데 말 그대로 각 데이터 타입마다 boolean 값을 도출하는 규칙이 있다는 뜻이다. 위 코드를 보면 str은 문자열(String)값이다. 하지만 len(str)을 함으로써 정수(Integer)값이 되었다. 이제 그 값은 == 12와 대응될 수 있는 것이다. 이처럼 boolean 결과를 도출하기 위해서는 해당 데이터 타입이 가진 boolean 결과를 도출할 수 있는 구문을 만들어서 도출시키라는 뜻이다.

때때로, boolean 상수 **True**와 **False**는 숫자가 될 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_4.png" title="Python tutorial 5 Picture 4" class="post-image">

위의 코드로 인해 **True는 1, False는 0**이라고 증명되었다. 그리고 수학적 계산에서 저 값은 숫자로 바뀐다.

__2. Numbers__<br>
Numbers는 파이썬(Python)에서 가장 중요한 데이터 타입 중 하나이다. 오직 정수와 실수만 가지고 있는 많은 언어와 달리 파이썬(Python)은 숫자의 새로운 타입으로 complex를 소개한다.

들어가기 전에 몇 가지 포인트가 있다.

파이썬(Python)에서 숫자는 아래 키워드로 정의된다.
__- int, float, complex__
파이썬(Python)은 변수나 값의 데이터 타입을 판단하기 위해서 **type()**이라는 이미 지정된 함수를 가지고 있다.
또다른 이미 지정된 함수 isinstance()는 객체의 타입을 테스트하는 것이다.
파이썬(Python)에서 **'j'**나 **'J'**를 숫자뒤에 쓰면 가상의 값, complex를 만들 수 있다.

**Example.**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_5.png" title="Python tutorial 5 Picture 5" class="post-image">

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_6.png" title="Python tutorial 5 Picture 6" class="post-image">

- complex 숫자를 만들려면 생성자로서 타입을 사용할 수 있다. 아래 예제를 보자.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_7.png" title="Python tutorial 5 Picture 7" class="post-image">

- 파이썬(Python)에서 정수는 메모리 제한은 메모리가 가능한 범위까지이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_8.png" title="Python tutorial 5 Picture 8" class="post-image">

- 실수(float)타입의 숫자는 소수점 15자리까지 표현할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_5_9.png" title="Python tutorial 5 Picture 9" class="post-image">

**Note -** 위 예제에서 dig는 실수에서 십진수의 최대값이다.

다음은 Strings와 Bytes입니다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
