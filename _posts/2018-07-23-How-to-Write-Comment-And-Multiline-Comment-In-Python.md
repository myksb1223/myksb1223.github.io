---
layout: post
title:  "파이썬에서 주석은 어떻게 쓰는가(How to Write Comment And Multiline Comment In Python)"
date:   2018-07-23 23:47:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬주석 PythonComment
---

[이번 단원](http://www.techbeamers.com/understand-python-comment-docstring/)은 파이썬의 주석다는 방법에 대한 튜토리얼이다.

주석은 주어진 코드를 따로 설명할 필요없고 가독성을 높여주는 이정표같은 것이다. 파이썬(Python)에서는 단일행, 다중행의 주석을 추가할 수 있다. 이번 튜토리얼은 이 두 방법을 자세히 다룰 것이다. 이 글을 읽고 나면 어떻게 파이썬(Python) 주석을 추가할 것이고 어떤 스타일을 사용해야하는지 알 것이다.

주석을 다는 것은 좋은 프로그래밍 습관이다. 주석은 코드의 실행되지 않는 부분이지만 프로그램에서 꽤 필수적이다. 이는 같은 프로젝트를 작업하는 다른 개발자(programmers)를 도와줄 뿐아니라 테스터들 역시 [화이트 박스 테스팅(white-box testing)](https://en.wikipedia.org/wiki/White-box_testing)을 정확히하기 위해 참조할 수 있다.

프로그램을 만들거나 업데이트할 때 주석을 추가하는 것이 좋다. 그렇지 않으면 코드의 흐름을 잃어버릴 수 있다. 그리고 나중에 쓰여진 주석은 생각보다 효과가 없을 수도 있다.

### How To Use  Comments In Python?(파이썬에서 주석은 어떻게 사용하는가?)
How to use single line comment in Python?(파이썬에서 단일행 주석은 어떻게 사용하는가?)
How to use multiline comments in Python?(파이썬에서 다중행 주석은 어떻게 사용하는가?)
- Using hash mark for commenting(주석을 위한 해시 마크 사용)
How to use docstrings in Python?(파이썬에서 docstring을 어떻게 사용하는가?)
- Using triple-quotation mark for docstrings(docstring을 위한 트리플 따옴표의 사용)
- docstring vs comment  in Python(파이썬에서 docstring vs 주석)

역시 한국어로 번역하면 이상해 진다...ㅠㅠ

주석은 매우 높은 단계에서 프로그램이 무엇을 할 것인지에 관한 표현의 기술이다. 이는 코드의 부분에 주석을 달기위해 태그된 텍스트 행이다. 파이썬에서 우리는 두 가지 스타일의 주석을 적용할 수 있다 : 단일 행, 다중 행.

### Single-Line Python Comment(단일 행 파이썬 주석)
디버깅을 위한 빠르고 짧은 주석의 필요성이 있을 때 단일 행 파이썬 주석을 선호할 것이다. 단일 행 주석은 #으로 시작하고 자동적으로 줄의 끝(EOL)로 끝난다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_4_1.png" title="Python tutorial 4 Picture 1" class="post-image">

주석을 넣는동안 그 주석이 아래 코드와 같은 들여쓰기인지 확인해야 한다. 예를 들면, 어떤 들여쓰기도 하지 않은 함수 정의에 주석을 달 수 있다. 하지만 그 함수는 여러 단계로 들여써진 코드 블럭을 가지고 있을 수도 있다. 그러므로 내부 코드 블럭에 주석을 달 경우에는 정렬에 신경써야 한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_4_2.png" title="Python tutorial 4 Picture 2" class="post-image">

### Multiline Python Comment(다중 행 파이썬 주석)
파이썬은 다중 행으로 주석을 확장하는 것을 지원한다. 이런 주석은 다중 행, 블럭 주석이라고 알려져 있다. 좀 더 자세하게 무엇인가를 설명할 때, 이런 스타일의 주석을 사용할 수 있다.

이런 확장된 형태의 주석은 그 주석을 따라오는 부분에서 전체의 코드에 적용된다. 다중 행 파이썬 주석의 사용 예제를 보자.

**Using The Hash (#) Mark(해시 마크의 사용)**<br>
다중 행 주석을 넣기 위해서는 각 라인의 시작을 #과 공백으로 하면 된다. 주석을 단락으로 나눌 수 있다. 단지 각 단락 사이에 #을 넣은 빈 줄을 추가하면 된다.

**Note** : #은 넘버 기호로 알려져 있다. 이 말은 touch-tone keypads 프로젝트의 처음 단계를 개발하는동안 Bell Labs의 엔지니어 그룹으로 부터 나왔다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_4_3.png" title="Python tutorial 4 Picture 3" class="post-image">

### Docstring In Python(파이썬에서의 Docstring)
파이썬은 도큐먼트 스트링(docstrings) 형태를 가지고 있다. 이는 개발자(programmer)에게 모든 파이썬 모듈, 함수, 클래스, 메소드에 빠른 노트를 추가할 수 있는 간단한 방법을 제공한다.

스트링 상수로서 추가함으로써 docstring을 정의할 수 있다. 객체(모듈, 함수, 클래스, 메소드)의 정의에서 가장 처음에 서술되어야 한다.

docstring은 파이썬 주석보다 훨씬 더 넓은 범위를 가진다. 더욱이 무슨 기능인지뿐만 아니라 무엇을 하는지도 서술할 수 있다. 또한 프로그램의 모든 기능이 docstring을 가질 수 있는 좋은 연습이다.

**How To Define Docstring In Python?(파이썬에서 docstring은 어떻게 정의하는가?)**<br>
트리플 따옴표 마크를 이용하여 docstring을 정의할 수 있다. 처음 부분 그리고 마지막 부분에 추가해주면 된다. 다중 행 주석과 마찬가지로 docstring 역시 다중 행을 포함할 수 있다.

**Note** : 파이썬에서 트리플 따옴표 마크로 사용하여 정의된 문자열은 docstring이다. 하지만 일반적인 주석으로 보일 것이다.

**What Is The Difference Between A Comment And The Docstring?(주석과 docstring의 차이는 무엇인가?)**<br>
트리플 따옴표로 시작하는 문자열은 다중 행으로 퍼질 수 있다는 사실을 제외하면 여전히 일반적인 문자열이다. 이 말은 실행될 수 있는 구문이라는 뜻이다. 그리고 명명되지 않으면, 그것들은 실행되자마자 바로 가비지 컬렉터에 의해 제거된다.

파이썬 인터프리터 주석처럼 그것들을 무시하지 않을 것이다. 하지만 이런 문자열이 함수나 클래스의 정의, 모듈의 맨 위 다음에 바로 위치한다면 그것들은 docstring으로 바뀐다. 다음에 보이는 특별한 변수를 사용함으로써 접근할 수 있다.

```python
myobj.__doc__
```

<img src="{{ site.baseurl }}/assets/python/python_tutorial_4_4.png" title="Python tutorial 4 Picture 4" class="post-image">
<img src="{{ site.baseurl }}/assets/python/python_tutorial_4_5.png" title="Python tutorial 4 Picture 5" class="post-image">

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
