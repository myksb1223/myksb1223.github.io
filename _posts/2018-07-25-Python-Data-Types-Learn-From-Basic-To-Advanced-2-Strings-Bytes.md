---
layout: post
title:  "파이썬 데이터 타입 - 기초부터 심화까지 2편(Python Data Types - Learn From Basic To Advanced 2) Strings, Bytes"
date:   2018-07-25 10:30:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬데이터타입 PythonDataType
---

이번 단원은 파이썬 데이터 타입에 대한 튜토리얼 두 번째이다.

**3. Strings**<br>
', " 을 이용하여 하나 또는 그 이상의 문자의 나열을 포함시킨 것을 파이썬(Python)에서는 문자열(String)이라고 한다. 어떤 문자든 숫자 또는 심볼은 string의 부분이 될 수 있다.

파이썬(Python)은 처음과 끝에 세 개의 "을 사용하여 다중 행 문자열도 지원한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_6_1.png" title="Python tutorial 6 Picture 1" class="post-image">

또한 파이썬(Python)에서의 문자열(string)은 변경가능하다. 이는 메모리에 한번 할당되고 재사용됨을 의미한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_6_2.png" title="Python tutorial 6 Picture 2" class="post-image">

두 번째 문자열(string)은 처음과 같은 메모리를 공유하는 것을 볼 수 있다.

파이썬(Python)은 **2.7과 3.4**의 두 가지 유명한 버전이 있다. 전세계의 대부분의 개발자는 이 두 가지 중 하나를 사용한다. 파이썬(Python) 2에서 문자열(string)은 기본적으로 유니코드가 아닌(non-Unicode) 아스키코드(ASCII)이지만 유니코드(Unicode)도 지원한다.

반면에 파이썬(Python) 3은 전부 유니코드(Unicode)이다.

**Strings in Python 2(파이썬 2에서의 문자열).**<br>
<img src="{{ site.baseurl }}/assets/python/python_tutorial_6_3.png" title="Python tutorial 6 Picture 3" class="post-image">

**Strings in Python 3(파이썬 3에서의 문자열).**

<img src="{{ site.baseurl }}/assets/python/python_tutorial_6_4.png" title="Python tutorial 6 Picture 4" class="post-image">

파이썬(Python)은 부분 문자열(substring)을 추출하기 위해 []을 이용하여 문자열(string)을 나눌 수 있습니다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_6_5.png" title="Python tutorial 6 Picture 5" class="post-image">

**4. Bytes**<br>
파이썬(Python)에서 바이트(byte)는 변경가능한 타입이다. 0~255 범위의 바이트 나열을 저장할 수 있다. 배열(array)과 비슷하게 인덱스(index)를 이용하여 단일 바이트의 값을 지정할 수 있다. 하지만 그 값을 변경할 수는 없다.

여기에 바이트(byte)와 문자열(string)의 몇 가지 차이점이 있다.

바이트(byte) 객체는 바이트(byte)의 나열을 포함할 수 있지만 문자열(string)은 문자(character)의 나열을 저장한다.
바이트(byte)는 기계가 읽을 수 있는(machine-readable) 객체지만 문자열(string)은 단지 사람이 읽을 수 있는(human-readable) 형태이다.
바이트(byte)는 기계가 읽을 수 있기(machine-readable)때문에 디스트(disk)에 바로 저장될 수 있다. 하지만 문자열(string)은 디스크(disk)에 저장하기 전에 먼저 인코드(encode)되어야 한다.

<img src="{{ site.baseurl }}/assets/python/python_tutorial_6_6.png" title="Python tutorial 6 Picture 6" class="post-image">

바이트(byte)가 중요한 한가지 시나리오는 버퍼링(buffering)이 가능한 입출력(I/O)작업을 다룰 때이다. 예를 들면, 네트워크를 통해 데이터를 지속적으로 받는 프로그램을 가지고 있다. 해당 스트림(stream)에서 메시지 해더와 종료자가 나타날 때까지 기다린 후 날짜를 파싱한다. 이는 버퍼에 들어오는 바이트(byte)를 계속 붙인다.

위의 말 뜻을 간단히 풀어보면 네트워크에서 데이터를 받을 때, 한번에 전체의 값을 받는 것이 아닌 보낼 수 있는 양만큼 나눠서 보낸다. (3GB가 되는 양을 한번에 보낸다고 생각해보자... 말도 안된다...) 그렇기 때문에 어떤 데이터가 4개로 나눠진 후 보내졌다면, 버퍼(buffer)에 순서대로 4개를 쌓아야지 하나의 데이터로 파싱(parsing)할 수 있다는 것이다.

파이썬(Python) 바이트(byte) 객체를 사용하면 위의 내용을 수도 코드(pseudo code)를 사용하여 쉽게 프로그램할 수 있다.

```python
buf = b''
while message_not_complete(buf):
    buf += read_from_socket()
```

튜토리얼의 이후 단락에서 바이트(byte)를 문자열(string)으로 문자열(string)을 바이트(byte)로 바꾸는 방법을 볼 것이다.

다음은 Lists와 Tuple입니다.
질문 및 잘못된 부분, 수정 부분있으면 댓글, 쪽지 주세요~!!!
