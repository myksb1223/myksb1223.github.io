---
layout: post
title:  "파이썬 키워드, 식별자, 변수 - 기초(Python Keywords, Identifiers And Variables – Fundamentals)"
date:   2018-07-22 21:05:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란
---

[이번 단원](http://www.techbeamers.com/understand-python-statement-indentation/)은 파이썬 구문, 표현 그리고 이 둘의 차이를 배울 것이다. 이 튜토리얼은 좀 더 명확히 개념을 설명하기 위해 몇몇의 예제를 담고 있다.

그 다음, 우리는 파이썬에서 어떻게 다중 구문과 들여쓰기를 사용하는지 설명할 것이다.

물론, 우리는 '파이썬에서 왜 들여쓰기가 중요하지?', '파이썬에서 들여쓰기는 몇 칸이지?' 등과 같은 질문에 답할 것이다.

아래는 우리가 이번 튜토리얼에서 다룰 주제들이다.

Statements in Python(파이썬 구문).
- What is Statement?(구문이란?).
- What is Expression?(표현식이란?).
- Simple Assignment Statement(간단한 할당 구문).
- Augmented Assignment Statement(확장된 할당 구문).
Multi-line Statement in Python(파이썬 다중 라인 구문).
- Explicit line continuation(명확한 라인 연결).
- Implicit line continuation(암식적 라인 연결).
Python Indentation(파이썬 들여쓰기).
- Ideal Indentation Size(최적을 들여쓰기 크기).
- Why is Indentation Important?(왜 들여쓰기가 중요한가?)

한글로 풀어쓰니 무슨 말인지 하나도 모르겠다...ㅋㅋㅋ

####Python Statement(파이썬 구문)
__What Is A Statement?(구문이란?)__
파이썬에서 구문이란 파이썬 인터프리터가 읽고 실행할 수 있는 논리적 명령이다. 파이썬에서는 표현식(expression)이나 할당 구문(assignment statement)일 수 있다.

그 할당 구문은 파이썬의 기본이다. 이는 표현식이 객체를 생성하고 그것들을 보존하는 방법을 정의하고 있다.

좀 더 자세히 이 주제를 알아보자.

__What Is A Expression?(표현식이란?)__
표현식은 숫자, 문자열, 객체, 연산자들의 논리적인 순서를 포함하는 파이썬 구문의 한 형태이다. 값 자체로 유효한 표현식이며 변수다.

표현식을 사용해서 우리는 더하기, 빼기, 연결하기 등과 같은 연산을 수행할 수 있다. 결과를 반환하는 함수의 호출도 표현식일 수 있습니다.

예제

<figure>
<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_1.png" title="Python tutorial 3 Picture 1" class="post-image">
<figcaption style="text-align: center;">수식, 함수를 사용한 표현
</figcaption>
</figure>

####Simple Assignment Statement(간단한 할당 구문)
간단한 할당에서 우리는 새로운 변수를 생성하고 값을 할당 및 변경한다. 이 구문은 표현식과 그 표현식의 값을 가지고 있는 이름으로서 변수명을 제공한다. 다시말해서 variable = expression와 같은 구조라는 뜻이다.

파이썬에서의 할당 구문의 3가지 형태를 알아보고 살펴보자.

__Case-1 : Right-Hand Side (RHS) Is Just A Value-Based Expression(케이스-1 : 오른쪽 부분은 단순히 값을 기반으로 한 표현식이다).__
파이썬에서 가장 기본의 할당 형태를 보자.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_2.png" title="Python tutorial 3 Picture 2" class="post-image">

파이썬은 'Learn Python'이라는 문자열을 메모리에 생성하고, 'test'라는 이름에 그것을 할당할 것이다. id()라는 이미 지정된 함수로 주소 값을 확인할 수 있다.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_3.png" title="Python tutorial 3 Picture 3" class="post-image">

저 숫자(4357848880)은 메모리에서 그 데이터가 존재하고 있는 장소의 주소이다. 이제 여기에는 너가 알아야 하는 몇몇의 흥미로운 것이 있다.

1. 같은 값으로 다른 문자열을 생성하면, 파이썬은 새로운 객체를 생성하고 메모리의 다른 부분에 그것을 할당할 것이다. 이 규칙은 거의 모든 경우에 적용된다.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_4.png" title="Python tutorial 3 Picture 4" class="post-image">

2. 하지만, 파이썬은 아래의 두 상황에서는 같은 메모리 주소에 할당할 것이다.
- 문자열이 20자보다 짧고 공백이 없는 경우.
- -5 ~ +255사이 범위의 정수인 경우.

이 개념은 Interning으로 잘 알려져 있다. 파이썬은 메모리를 절약할 수 있다.

__Case-2 : Right-Hand Side (RHS) Is A Current Python Variable(케이스-2 : 오른쪽 부분은 현재 파이썬 변수이다).__
RHS가 현재 파이썬 변수인 할당 구문의 다음 형태를 알아보자.

```
>>> another_test = test
```

위의 구문은 메모리에서 어떤 새로운 할당을 하지 않는다. 두 변수는 같은 메모리 주소를 가르킬 것이다. 이는 존재하는 객체에 별칭(alias)를 생성하는 것과 같다. 이 것이 타당한지 id()함수를 사용해서 알아보자.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_5.png" title="Python tutorial 3 Picture 5" class="post-image">

__Case-3 : Right-Hand Side (RHS) Is An Operation(케이스-2 : 오른쪽 부분은 연산이다).__
이런 형태의 구문에서 결과는 그 연산의 결과에 결정된다. 다음 예제를 분석해 보자.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_6.png" title="Python tutorial 3 Picture 6" class="post-image">

위의 예제에서의 할당문은 'float' 변수를 생성하게 이끌었다.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_7.png" title="Python tutorial 3 Picture 7" class="post-image">

이 예제에서의 할당문은 'int' 변수를 생성하게 이끌었다.

####Augmented Assignment Statement(확장된 할당 구문)
너는 확장된 할당 구문을 수행하기 위해 할당에 수학 연산자를 함께 사용할 수 있다.
확장된 할당 구문의 아래 예제를 보자.

```
x += y
```

위의 구문은 아래의 구문을 짧게 나타낸 것이다.

```
x = x + y
```

튜플에 새로운 요소들을 추가하는 다음의 예제가 좀 더 명확하다.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_8.png" title="Python tutorial 3 Picture 8" class="post-image">

다음 예제는 모음의 리스트를 사용하고 있다. 리스트에서 빠진 모음의 추가를 보여주고 있다.

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_9.png" title="Python tutorial 3 Picture 9" class="post-image">

####Multi-line Statement In Python(파이썬에서 다중 라인 구문).
보통 모든 파이썬 구문은 개행 문자로 끝을 낸다. 하지만 우리는 라인 연결 문자(\, 백슬래쉬)를 사용하여 다중 라인으로 확장할 수 있다.

그리고 파이썬은 프로그램에서 다중 라인 구문을 가능하게 하는 두가지 방법을 준다.

####Explicit Line Continuation(명확한 라인 연결)
라인 연결 문자(\, 백슬래쉬)을 사용하여 구문을 바로 다중 라인으로 구분하고 싶을 때.

예제

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_10.png" title="Python tutorial 3 Picture 10" class="post-image">

Implicit Line Continuation(암시적 라인 연결)
소괄호(), 대괄호[], 중괄호{}를 사용했을 경우 라인을 구분할 경우를 암시적 라인 연결이라고 한다. 앞에 말한 구조를 사용하여 타겟 구문을 묶어야 한다.

예제

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_11.png" title="Python tutorial 3 Picture 11" class="post-image">

####Python Indentation(파이썬 들여쓰기)
C, C++, C#과 같은 높은 단계의 개발 언어들은 코드의 블럭을 표시하기위해 중괄호 {}를 사용한다. 파이썬은 그 것을 들여쓰기로 한다.

반복문과 함수의 몸체와 같은 코드 블럭은 들여쓰기로 시작해 처음의 들여쓰기로 끝난다. (말 그대로 처음 들여쓰기 한 만큼 들여쓰기해서 끝내라는 뜻이다.)

__How Many Space Is An Indent In Python?(파이썬에서는 들여쓰기 크기는 어느 정도인가?)__
파이썬 스타일 가이드라인(PEP 8)에서는 4를 유지하라고 쓰여있다. 하지만 구글은 그들의 개별 스타일 가이드라인은 2를 넘지 말라고 한다. 그러므로 아무거나 써도 되지만 PEP 8을 추천한다.

__Why Is Indentation So Crucial In Python?(왜 파이썬에서 들여쓰기가 중요한가?)__
대부분의 개발 언어는 더 나은 포맷을 위해 들여쓰기를 제공하고 그것을 강요하지 않는다.

하지만 파이썬에서는 들여쓰기 규칙을 지키는 것이 의무이다. 일반적으로 우리는 코드 블럭에서는 각 라인당 4의 들여쓰기를 한다.

앞 단락 예제에서 들여쓰기 없이 간단 표현식 구문을 써 왔던 것을 봤을지도 모른다.

하지만 복잡한 구문의 생성을 위해서는 들여쓰기는 정말 필요할 것이다.

예제

<img src="{{ site.baseurl }}/assets/develop_diary/python_tutorial_3_12.png" title="Python tutorial 3 Picture 12" class="post-image">

질문 및 잘못된 부분, 수정 부분있으면 댓글, 쪽지 주세요~!!!
