---
layout: post
title:  "파이썬 지침서(Python tutorial - Learn Programming Step By Step)"
date:   2018-07-18 01:45:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란
---

요즘... 파이썬이 대세 언어라서 공부를 시작하기로 마음 먹었다.
딱히, 책을 사서 공부하는 타입이 아니라서... 웹의 튜토리얼 자료를 번역하기로 했다. 당연히 내 생각도 같이 넣을거다.

[파이썬 위키(Python Wiki)](https://wiki.python.org/)에서 추천한 사이트 중에 [처음에 있는 녀석](http://www.techbeamers.com/python-tutorial-step-by-step)을 골랐다.

이 지침서의 목적은 기본부터 심화까지 다루는데, 최소의 노력, 최소의 시간을 이용해서 파이썬 개발을 할 수 있게하는 것이다.

파이썬은 가장 많이 쓰이는 언어 중에 3번째이다. 1등은 자바(JAVA), 2등은 Objective-C, 3등이 파이썬(Python)이다. multipurpose, high-level, object-oriented, interactive, interpreted and extremely user-friendly 프로그래밍 언어이다.

interactive 모드는 콘솔에서 친 매 문장마다 즉각적인 피드백을 주는 파이썬(Python) 쉘의 형태이다.
추가적으로 설명하자면...
1. 컴파일(Compile)이 필요없이 5+3 이라고 치면 8이라는 결과를 바로 보여주는 것.
2. 리눅스 쉘에서 ls나 cd 등의 명령어 치면 바로 결과가 나오는 것.
3. 루비(ruby)의 console에서 작업하는 것.
위와 같이 생각하면 이해하기 쉽다.

mulitpurpose라는 것은 말 그대로 파이썬(Python)으로 응용할 수 있는 분야가 많다는 것이다.
high-level은 low-level의 언어가 아니라는 것 같다.
interpreted는 컴파일(compile)언어가 아니라는 것이다.
user-friendly는 누구나 배우기 쉽다는 정도로 이해하면 될 것 같다.

#### 파이썬(Python) 개발의 기본적 특징

1. Code Quality(코드 질).
워낙 읽기 쉬워서 재사용, 유지하기가 좋다. 소프트웨어 개발을 폭넓게 지원한다.

2. Developer Productivity(개발자의 생산성).
깨끗하고 멋진 코드 스타일. 영어와 비슷한 문법을 사용하며 역동적인 형태. 그래서 변수 선언이 필요없다. 그렇기 때문에 C++/Java 코드보다 훨씬 작다. 이는 파이썬(Python)이 타입이 적고, 디버그가 제한적이고 유지할 것이 적다는 것을 나타낸다. 컴파일(Compile) 언어가 아니기에 개발자의 속도를 향상시킨다.

3. Code Portability(코드 호환성).
파이썬(Python)이 interpreted 언어이기 때문에 interpreter가 호환을 관리한다. 이 interpreter가 똑똑해서 다른 플랫폼에서 실행되어도 같은 결과를 반환한다. 그렇기에 코드를 바꿀 필요가 없다.

4. Built-In And External Libraries(빌트인 그리고 확장 라이브러리).
파이썬(Python)은 엄청나게 많은 양의 미리 만들어지고 호환가능한 라이브러리가 존재한다. 사용하고 싶을 때 알맞게 가져와서 사용하면 된다.

5. Component Integration(컴포넌트 통합).
파이썬(Python)은 여러 방법으로 다른 어플리케이션과의 연결을 지원한다.

6. Free To Use, Modify And Redistribute(자유로운 사용, 수정 그리고 재배포).
파이썬(Python)은 일단 무료이고 상업적 용도를 위해 수정 및 재배포해도 상관없다.

7. Object-Oriented From The Core(핵심은 객체지향)
파이썬(Python)은 객체지향을 따른다.

#### 파이썬(Python) 개발의 영역.

1. Web Application Development(웹 앱 개발).
Django같은 웹 프레임 워크를 이용한 개발을 뜻하는 것 같다.

2. Scientific And Numeric Computing(과학, 산술 계산).
파이썬(Python)의 특징 중 수정과 재배포가 자유롭기 때문에 관련된 모듈이 엄청나다고 한다.

3. GUI Programming(GUI 개발).
파이썬(Python)의 특징 중 수정과 재배포, 코드의 쉬운 점 등 때문에 GUI 라이브러리도 많다고 한다.

4. Software Prototyping(소프트웨어 프로토타입).
파이썬(Python)의 특징 중 코드가 쉽고 개발이 빨라서 프로토타입을 개발하는데도 많이 사용된다.

5.  Professional Training(전문적 훈련).
파이썬(Python)은 가르치고 훈련 목적에 적합한 언어라고 한다.

왜 우리는 파이썬을 배워야 하는가?
1. Nonrestrictive Programming Syntax(제한 없는 개발 문법).
말 그대로 문법이 파괴적이라 예전부터 코딩하는 사람들은 적응이 잘 안될 것 같다. 나 역시 적응이 쉽지 않다. 그나마 Ruby(루비)를 좀 해서 그나마 받아들이기 쉬운 것일 수도 있다...
예시를 보면 바로 알 수 있다.

```python
print('Interest Calculator:')

amount = float(input('Principal amount ?'))
roi = float(input('Rate of Interest ?'))
yrs = int(input('Duration (no. of years) ?'))

total = (amount * pow(1 + (roi/100), yrs))
interest = total - amount
print('\nInterest = %0.2f' %interest)
```

딱 봐도 간단하다...

2. No Explicit Declaration(명쾌한 정의가 없다).
변수 선언에 데이터 타입이 따로 존재하지 않는다. 위의 코드만 봐도 알 수 있다.
이 부분은 Ruby(루비)와 비슷하다.

3. State Of The Art OOP Support(객체 지향 지원의 형태).
파이썬(Python)은 객체지향 언어이다.

4.  Powerful Debugging(강력한 디버깅).
강력한 디버깅... 더 할 말이 없다. 디버깅이 좋다고 한다.

여기까지가 첫 내용이다.

질문 및 잘못된 부분, 수정 부분있으면 댓글, 쪽지 주세요~!!!
