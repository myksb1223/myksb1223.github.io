---
layout: post
title:  "파이썬 키워드, 식별자, 변수 - 기초(Python Keywords, Identifiers And Variables – Fundamentals)"
date:   2018-07-20 23:54:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬키워드식별자변수 PythonKeywordsIdentifiersVariables
---

[이번 단원](http://www.techbeamers.com/python-keywords-identifiers-variables/)은 파이썬 키워드, 식별자, 변수의 기초에 대해서 알아본다. 이 것들은 파이썬 언어의 기본 골격을 이루는 것들이기 때문에 반드시 알아야 한다.

파이썬 키워드는 어떤 액션을 목적으로 하는 유니크한 용어이다. 33개 정도 가지고 있고 각자 하는 기능이 다르다. 파이선 언어는 이 키워드를 함께 써서 어휘를 만든다.
무슨 소리인가 하니... 예를 들어 for ~ in 구문 처럼 for, in인 키워드를 함께 써서 for문을 구성할 수 있다는 소리같다.

이 들은 파이썬 언어의 문법, 구문과 구조를 표현한다. 이 키워들은 이미 정해졌기에 변수, 클래스, 함수 명으로 사용할 수 없다.

#### Python Keywords(파이썬 키워드).<br>
키워드는 특별한 의미를 가지고 제공되는 특별한 단어이다. 파이썬은 키워드들의 집합을 가지고 있는데 이들은 프로그램에서 변수로 사용될 수 없다.

파이썬 내의 모든 키워드는 대소문자 구분을 한다. 따라서 사용할 때 주의해야 한다. 아래 사진을 첨부한다. 사진은 내가 직접 쳐보고 캡쳐해 보았다.

<figure>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_2_1.png" title="Python tutorial 2 Picture 1" class="post-image">
<figcaption style="text-align: center;">Python Keywords Display Using Python Shell</figcaption>
</figure>

한번에 외우기는 긴 리스트이다. 여기에 이것을 언급하는 이유는 너에게 가능한 키워드의 초기 아이디어를 주기위해서다. 하지만 우리는 남은 튜토리얼에서 전부 다룰 것이다. 너는 이 키워드를 외우려고 하지 말고 단계적으로 공부할 수 있도록 하자.
튜토리얼 다 하면 알아서 공부된다고 한다. 그러니까 다같이 튜토리얼을 끝까지 해 보도록 합시다!

한가지 더 이야기 할 것은 위의 리스트는 변할 수 있다는 것이다. 언어는 옛 키워드를 없앨 수 있고 새로운 키워드를 나중에 넣을 수도 있다.

더 나아가서 최신의 목록을 얻으려면 아까 사진 처럼 keywords라고 치거나, 아래 그림처럼 파이썬의 키워드 모듈을 사용할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_2_2.png" title="Python tutorial 2 Picture 2" class="post-image">

### Python Identifiers(파이썬 식별자).<br>
파이썬 식별자는 변수, 함수, 클래스, 모듈, 다른 객체를 표현하기 위한 사용자 정의된 이름이다. 만약 파이썬에서 어떤 이름을 개발가능한 요소로 지정했다면 아무것도 아니지만 기술적으로 식별자라고 부른다.

파이썬 언어는 의미있는 식별자를 만들기 위해서 개발자를 위한 규칙들의 집합을 제시한다.

__Guidelines For Creating Identifiers In Python(파이썬에서 식별자를 생성하는 방법).__
1. a~z, A-Z, 0~9, _ 를 사용할 수 있다.
2. 숫자는 맨 처음에 사용할 수 없다. (0shape -> x, shape1 -> o)
3. 키워드는 이미 정해졌기에 사용할 수 없다.

    <img src="{{ site.baseurl }}/assets/python/python_tutorial_2_3.png" title="Python tutorial 2 Picture 3" class="post-image">

4. 특수문자('.', '!', '@', '#', '$', '%')는 사용할 수 없다.

    <img src="{{ site.baseurl }}/assets/python/python_tutorial_2_4.png" title="Python tutorial 2 Picture 4" class="post-image">

5. 파이썬 도큐먼트에서는 식별자 길이제한이 없다고 하지만 이 것은 반만 맞는 이야기다.
79글자 많이 쓰면 PEP-8 기준에 의해 설정된 규칙을 어기게 된다.
즉, 79자 보다 많이 쓰지 말자.

__Testing If An Identifier Is Valid(식별자가 올바른지 테스트).__<br>
keyword.isKeyword() 함수를 사용해서 식별자가 올바른지 테스트할 수 있다. true를 반환하면 올바른 것, 아니면 false를 반환한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_2_5.png" title="Python tutorial 2 Picture 5" class="post-image">

다른 좋은 방법은 str.identifier() 함수를 사용하는 것이다. 하지만 Python3.0이상에서만 사용할 수 있다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_2_6.png" title="Python tutorial 2 Picture 6" class="post-image">

__Best Practices For Identifier Naming(식별자 이름을 위한 가장 좋은 방법).__
- 클래스 이름의 첫자는 대문자로 시작한다. 다른 모든 식별자는 소문자로 시작한다.
- private 식별자를 선언하기 위해서는 언더바('\_')를 식별자 맨 앞에 넣는다.
- 식별자에서 '\_'을 맨 앞, 맨 뒤에 사용하지 말자. 파이썬이 이미 가지고 있는 타입으로 이미 '\_'을 사용할 수도 있기 때문이다.
- 한 글자인 식별자는 피하자. 의미있게 이름을 짓자.
i = 1 보다 iter = 1이나 index = 1이 낫다.
- 여러 단어가 뭉쳐지는 식별자의 경우 '_'을 사용하자.
count_no_of_letters 와 같이...

#### Python Variables(파이썬 변수).<br>
파이썬 변수는 어떤 요구에 따라 값이 변경될 수 있는 요소를 표현한다. 개념적으로 실제 값을 가지고 있는 메모리 주소를 뜻한다. 그리고 그 요소를 코드로 부터 찾으면서 그 값을 찾을 수 있다.

하지만 그 것을 참조하기 위해서 메모리 주소와 레이블을 매칭시키길 원한다. 그리고 그 것을 프로그래밍 용어로 변수라고 한다.

파이썬 변수에 대한 핵심 사항을 보자. 효과적으로 코딩하는데 도움을 줄 것이다.

1. 변수는 선언이 필요없다. 하지만 사용전에 그 것을 초기화 하자.
test = 10
2. 위의 표현은 다음과 같은 과정이 존재한다.
- 10을 표현하기 위한 객체 생성.
- test가 존재하지 않으면 생성된다.
- 그 것이 값을 참조할 수 있도록 하기위해 객체와 변수를 연결시킨다.

    변수 test는 값 10을 참조한다.
    test -------> 10
 3. 표현이 변하면 파이썬은 그 값을 참조할 수 있는 새로운 변수를 새로운 객체와 연결시킨다.
그리고 예전 것은 가비지 컬렉터가 없앤다.

    <img src="{{ site.baseurl }}/assets/python/python_tutorial_2_7.png" title="Python tutorial 2 Picture 7" class="post-image">

4. 또한 최적화를 위해 파이썬은 작은 정수들이나 문자열들과 같은 변형가능한 객체들의 일부를 재사용 및 캐시를 한다.
5. 객체는 단지 아래의 것들을 가질 수 있는 메모리 영역일 뿐이다.
- 실제 객체 값들.
- 객체 타입을 반영하는 타입 지정자.(?)
- 그 객체를 돌려받아도 좋을 때를 결정하는 참조 카운터.
6. 그것은 변수가 아닌 타입을 가진 객체다. 하지만 변수는 필요할 때 다른 타입의 객체를 가질 수 있다.

즉, 변수는 어떤 녀석이든 다 주소값이 있으므로 그 주소를 참조하고 있는 녀석이다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_2_8.png" title="Python tutorial 2 Picture 8" class="post-image">

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
