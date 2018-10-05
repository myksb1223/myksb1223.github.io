---
layout: post
title:  "파이썬 Tuple - 빠르게 시작하기 위한 완벽 가이드 1편(Python Tuple – A Complete Guide To Get Started Quickly 1)"
date:   2018-10-06 00:55:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬Tuple PythonTuple Tuple 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-tuple/)은 파이썬(Python) Tuple이 무엇이고, 이것으로 무엇을 할 수 있고, 프로그램에서 이를 어떻게 사용하는지 알아보는 시간이다. 더욱이, Tuple을 어떻게 생성하고 packing, unpacking, slicing, comparison, deletion 등의 다양한 연산을 어떻게 수행하는지 필수적으로 배울 것이다.

### Python Tuple – Introduction.
파이썬(Python) Tuple은 변경할수 없도록 디자인된 집합 타입의 데이터 구조이고 서로 다른 요소의 순서를 가진다. 기능은 **파이썬(Python) List**와 거의 비슷하지만 아래와 같은 차이가 있다.

- Tuple은 고정된 요소의 set을 가지고 그 List가 해당 컨텐트를 업데이트하기 위한 규정을 가지지 않는한 변경을 허용하지 않는다.
- List는 '[]'을 사용하는 반면에 Tuple은 '()'을 사용한다.

Tuple은 다른 상황에서 프로그래머들에게 아주 편리할 수 있다. 나중에 이 튜토리얼에서 그 것들에 대해 다룰 것이다.

### How To Instantiate A Tuple In Python?(파이썬에서 어떻게 Tuple을 초기화 할까?)

'()'내에 ','으로 분리된 원하는 요소들을 순서대로 위치시킴으로써 Tuple을 생성할 수 있다.

'()'없이도 Tuple을 생성할 수 있다는 것을 기억해야한다. 또한, Tuple의 요소들은 어떤 유효한 Number, String, List 등의 **파이썬(Python) 데이터 타입**이 될 수 있다.

#### 1. Simple Examples To Create A Tuple With Different Inputs.(다른 입력들을 이용해 Tuple 생성하는 간단한 예제.)

```python
# create an empty tuple
py_tuple = ()
print("A blank tuple:", py_tuple)

# create a tuple without using round brackets
py_tuple = 33, 55, 77
print("A tuple set without parenthesis:", py_tuple, "type:", type(py_tuple))

# create a tuple of numbers
py_tuple = (33, 55, 77)
print("A tuple of numbers:", py_tuple)

# create a tuple of mixed numbers
# such as integer, float, imaginary
py_tuple = (33, 3.3, 3+3j)
print("A tuple of mixed numbers:", py_tuple)

# create a tuple of mixed data types
# such as numbers, strings, lists
py_tuple = (33, "33", [3, 3])
print("A tuple of mixed data types:", py_tuple)

# create a tuple of tuples
# i.e. a nested tuple
py_tuple = (('x', 'y', 'z'), ('X', 'Y', 'Z'))
print("A tuple of tuples:", py_tuple)
```

위의 코드를 실행하면 아래의 결과를 도출한다.

```python
# output
A blank tuple: ()
A tuple set without parenthesis: (33, 55, 77) type: <class 'tuple'>
A tuple of numbers: (33, 55, 77)
A tuple of mixed numbers: (33, 3.3, (3+3j))
A tuple of mixed data types: (33, '33', [3, 3])
A tuple of tuples: (('x', 'y', 'z'), ('X', 'Y', 'Z'))
```
#### 2. Using The Built-In Function **"Tuple()"** To Create A Tuple.(**"Tuple()"**이라는 내장 함수를 사용해서 Tuple 생성하기.)

Tuple 함수를 실행할 수 있고 원하는 결과를 얻을 수 있다. 아래의 예를 보자.

```python
# creating a tuple from a set
>>> py_tuple = tuple({33, 55 , 77})
>>> type(py_tuple)
<class 'tuple'>
>>> py_tuple
(33, 77, 55)
# creating a tuple from a list
>>> py_tuple = tuple([33, 55 , 77])
>>> type(py_tuple)
<class 'tuple'>
>>> py_tuple
(33, 55, 77)
```

#### 3. Create A Tuple Of Size One.(크기가 1인 Tuple 생성하기.)

아래의 예제에서 단일 요소로 Tuple을 생성을 시도하고 있다. 보는 것처럼 결과를 얻기 쉽지 않다.

```python
# A single element surrounded by parenthesis will create a string instead of a tuple
>>> py_tuple = ('single')
>>> type(py_tuple)
<class 'str'>
# You need to place a comma after the first element to create a tuple of size "one"
>>> py_tuple = ('single',)
>>> type(py_tuple)
<class 'tuple'>
# You can use a list of one element and convert it to a tuple
>>> py_tuple = tuple(['single'])
>>> type(py_tuple)
<class 'tuple'>
# You can use a set of one element and convert it to a tuple
>>> py_tuple = tuple({'single'})
>>> type(py_tuple)
<class 'tuple'>
```

### How Can You Access A Tuple In Python?(파이썬에서 어떻게 Tuple에 접근할 수 있을까?)

파이썬(Python)은 튜플로 부터 단일 또는 범위의 요소에 접근할 수 있는 다양한 직관적인 메커니즘을 제공한다.

#### 1. Via Indexing(인덱싱을 통해)

가장 간단한 방법은 Tuple로 부터 아이템을 가져오는 인덱스 연산자 '[]'인 직접 접근 메소드를 사용하는 것이다. 인덱스는 0부터 시작한다.

Tuple이 10개의 요소를 가지고 있다면 인덱스는 0-9까지라는 것을 뜻한다. Tuple의 범위를 벗어나면 IndexError를 도출할 것이다.

인덱스는 항상 integer이다. float나 인덱싱 목적을 위한 어떤 다른 형태의 Number는 사용할 수 없다. 사용하면 TypeError를 도출한다.

다음으로 Tuple이 그 요소로 다른 Tuple을 포함하고 있다면 각 Tuple마다 인덱스할 필요가 있다.

```python
vowel_tuple = ('a','e','i','o','u')
print("The tuple:", vowel_tuple, "Length:", len(vowel_tuple))

# Indexing the first element
print("OP(vowel_tuple[0]):", vowel_tuple[0])

# Indexing the last element
print("OP(vowel_tuple[length-1]):", vowel_tuple[len(vowel_tuple) - 1])

# Indexing a non-existent member
# will raise the IndexError
try:
print(vowel_tuple[len(vowel_tuple)+1])
except Exception as ex:
print("OP(vowel_tuple[length+1]) Error:", ex)

# Indexing with a non-integer index
# will raise the TypeError
try:
print(vowel_tuple[0.0])
except Exception as ex:
print("OP(vowel_tuple[0.0]) Error:", ex)

# Indexing in a tuple of tuples
t_o_t = (('jan', 'feb', 'mar'), ('sun', 'mon', 'wed'))

# Accessing elements from the first sub tuple
print("OP(t_o_t[0][2]):", t_o_t[0][2])

# Accessing elements from the second sub tuple
print("OP(t_o_t[1][2]):", t_o_t[1][2])
```

위의 코드를 실행하면 아래의 결과를 볼 수 있다.

```python
# output
The tuple: ('a', 'e', 'i', 'o', 'u') Length: 5
OP(vowel_tuple[0]): a
OP(vowel_tuple[length-1]): u
OP(vowel_tuple[length+1]) Error: tuple index out of range
OP(vowel_tuple[0.0]) Error: tuple indices must be integers or slices, not float
OP(t_o_t[0][2]): mar
OP(t_o_t[1][2]): wed
```

#### 2. Via Reverse Indexing(역 인덱싱을 통해)

파이썬(Python) Tuple은 역 인덱싱을 지원한다. 예를 들면 '-' 인덱스 값을 사용해 값에 접근하는 것.

역 인덱싱은 아래의 규칙에서 작동한다.

- 인덱스 -1은 마지막 아이템을 나타낸다.
- 인덱스 -2는 뒤에서 두번째 아이템을 나타낸다.

좀 더 명확히 하기위해 아래의 예제를 살펴보자.

```python
>>> vowels = ('a','e','i','o','u')
>>> vowels
('a', 'e', 'i', 'o', 'u')
>>> vowels[-1]
'u'
>>> vowels[-2]
'o'
>>> vowels[-5]
'a'
>>> vowels[-6]
Traceback (most recent call last):
File "<pyshell#64>", line 1, in <module>
vowels[-6]
IndexError: tuple index out of range
```

#### 3. Via Slicing Operator(자르기 연사자를 통해)

Tuple에서 하나가 아닌 여러 요소에 접근할 필요가 있다면 파이썬(Python)의 자르기 연산자를 사용할 수 있다.

':'는 파이썬(Python)에서 자르기 연산자를 나타낸다.

어떻게 ':'이 아래 예제에서 사용되는지 보자.

```python
>>> weekdays = ('mon', 'tue', 'wed' ,'thu', 'fri', 'sat', 'sun')
>>> weekdays
('mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun')
# accessing elements leaving the first one
>>> weekdays[1:]
('tue', 'wed', 'thu', 'fri', 'sat', 'sun')
# accessing elements between the first and fifth positions
# excluding the ones at the first and fifth position
>>> weekdays[1:5]
('tue', 'wed', 'thu', 'fri')
# accessing elements after the fifth position
>>> weekdays[5:]
('sat', 'sun')
# accessing the first five elements
>>> weekdays[:5]
('mon', 'tue', 'wed', 'thu', 'fri')
# accessing elements that appears after
# counting five from the rear end
>>> weekdays[:-5]
('mon', 'tue')
# accessing five elements from the rear
>>> weekdays[-5:]
('wed', 'thu', 'fri', 'sat', 'sun')
# accessing elements from the start to end
>>> weekdays[:]
('mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun')
```

다음은 Tuple 수정에 관해 배우도록 한다.

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
