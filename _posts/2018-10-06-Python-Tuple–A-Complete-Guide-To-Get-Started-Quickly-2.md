---
layout: post
title:  "파이썬 Tuple - 빠르게 시작하기 위한 완벽 가이드 2편(Python Tuple – A Complete Guide To Get Started Quickly 2)"
date:   2018-10-06 01:34:00
author: Seungbeom Kim
categories: python
tags: 파이썬 Python 파이썬튜토리얼 PythonTutorial 파이썬강의 파이썬개발 PythonDevelopment 파이썬이란 파이썬Tuple PythonTuple Tuple 파이썬데이터타입 PythonDataType
sitemap :
  changefreq : daily
  priority : 1.0
---

[이번 단원](https://www.techbeamers.com/python-tuple/)은 파이썬(Python) Tuple을 예제로 배우는 2번째 시간이다.

### How Can You Modify A Tuple In Python?(파이썬에서 어떻게 Tuple을 수정할 수 있을까?)

Tuple은 변경할 수 없기때문에 변경할 방법이 없어 보인다.

일단 Tuple에 요소들의 입력하면 파이썬(Python)은 변경을 허용하지 않는다. 하지만 한가지 옵션이 있다. 넣은 그 아이템들이 변경할수 있다면 어떨까?

그러한 경우들이 있다면 직접적으로 Tuple을 변경하는 것 보다 그 요소들을 변경할 수 있다.

더욱이 다른 값들을 가지게 하는 Tuple을 설정할 수 있다. 아래 예제를 보자.

```python
py_tuple = (22, 33, 55, 66, [88, 99])
print("Tuple before modificaton:", py_tuple)

# Let's try to modify py_tuple
# It'll return a TypeError
try:
py_tuple[0] = 11
except Exception as ex:
print("OP(py_tuple[0]) Error:", ex)

# We can change the values of mutable
# elements inside the py_tuple i.e. list
py_tuple[4][0] = 77
py_tuple[4][1] = 88
print("Tuple after modificaton:", py_tuple)

# We can assign a tuple with new data
py_tuple = ('mon', 'tue', 'wed')
print("Tuple after reassignment:", py_tuple)
```

실행 후 아래의 결과를 도출한다.

```python
# output
Tuple before modificaton: (22, 33, 55, 66, [88, 99])
OP(py_tuple[0]) Error: 'tuple' object does not support item assignment
Tuple after modificaton: (22, 33, 55, 66, [77, 88])
Tuple after reassignment: ('mon', 'tue', 'wed')
```

더욱이 **'+'(concatenation)**, **'*'(repeat)** 연산자를 사용하면 Tuple의 동작을 확장할 수 있다.

'+' 연산자는 두개의 다른 Tuple을 합치는데 도움을 준다.

```python
>>> first_tuple = ('p', 'y', 't')
>>> second_tuple = ('h', 'o', 'n')
>>> full_tuple = first_tuple + second_tuple
>>> full_tuple
('p', 'y', 't', 'h', 'o', 'n')
>>>
```

그리고 '*'은 정해진 횟수 만큼 Tuple내의 요소를 반복하는데 도움을 준다.

```python
>>> init_tuple = ("fork", )
>>> fork_tuple = init_tuple * 5
>>> fork_tuple
('fork', 'fork', 'fork', 'fork', 'fork')
>>>
```

### How Can You Delete A Tuple In Python?(파이썬에서 어떻게 Tuple을 지울 수 있을까?)

Tuple의 불변함은 파이썬(Python) 프로그램에서 그것을 지우는 것 역시 막는다. Tuple은 직접적으로 지울 수 없지만 도움을 주는 무엇인가가 여기 있다.

파이썬(Python)의 del 키워드는 Tuple을 지울 수 있게 해준다. 아래의 예제를 보자.

```python
py_tuple = ('p', 'y', 't', 'h', 'o', 'n')

# you can't delete a particular item from a tuple
try:
del py_tuple[0]
except Exception as ex:
print("OP(del py_tuple[0]) Error:", ex)

# but you can delete a whole tuple
del py_tuple
try:
print(py_tuple)
except Exception as ex:
print("print(py_tuple) => Error:", ex)
```

위의 코드를 실행하면 아래의 결과를 보여준다.

```python
# output
del py_tuple[0] => Error: 'tuple' object doesn't support item deletion
print(py_tuple) => Error: name 'py_tuple' is not defined
```

첫 줄은 Tuple로 부터 특정한 아이템은 지울 수 없다는 것을 알려주는 결과이다. 하지만 두번째 줄은 이미 지워졌고 정의되지 않은 객체에 접근을 하고 있다고 알려주고 있다.

### Miscellaneous Tuple Operations(기타 Tuple 조작)

#### 1. Testing Membership In Pyhton Tuple(파이썬 Tuple에서 멤버쉽 테스트)

파이썬(Python) Set에서 했던 것과 같이 'in' 키워드는 Tuple의 멤버쉽 테스트를 도와 줄 것이다.

```python
>>> py_tuple = ('p', 'y', 't', 'h', 'o', 'n')
>>> print("First Test: Does 'p' exist?", 'p' in py_tuple)
First Test: Does 'p' exist? True
>>> print("Second Test: Does 'z' exist?", 'z' in py_tuple)
Second Test: Does 'z' exist? False
>>> print("Third Test: Does 'n' exist?", 'n' in py_tuple)
Third Test: Does 'n' exist? True
>>> print("Last Test: Does 't' not exist?", 't' not in py_tuple)
Last Test: Does 't' not exist? False
>>>
```

#### 2. Traversing In A Pyhton Tuple(파이썬 Tuple에서 순회)

for 반복을 구성할 수 있고 Tuple에서 모든 요소에 하나씩 접근할 수 있다.

```python
>>> py_tuple = ('p', 'y', 't', 'h', 'o', 'n')
>>> for item in py_tuple:
	print("Item:", item)

Item: p
Item: y
Item: t
Item: h
Item: o
Item: n
```

### Usage Of Python Tuples(파이썬 Tuple의 사용)

#### 1. Used For Grouping Data(데이터 그룹화를 위해 사용)

Tuple은 데이터의 그룹화와 정렬의 빠른 방법을 제공한다. 얼마의 요소든 하나의 유닛으로 합칠 수 있다.

직원 기록과 같은 레코드의 형태의 정보를 표현하는데 도움을 준다. Tuple은 관련된 정보를 그룹화 하고 하나로 사용할 수 있게 해준다.

```python
>>> emp_records = ('john', 'hr', 2010, 'robert', 'account', 2015, 'bill', 'mis', 2018)
>>> emp_records[3]
'robert'
```

#### 2. Assign To A Tuple(Tuple에 할당)

파이썬(Python) Tuple은 "Tuple assignment"로 알려진 매우 직관적인 형태를 지원한다. 왼쪽 부분의 Tuple의 변수들이 오른편의 Tuple로 부터 초기화될 수 있게 한다.

```python
>>> emp_records = ('john', 'hr', 2010, 'robert', 'account', 2015, 'bill', 'mis', 2018)
>>> (emp_name, emp_dept, emp_join_date) = emp_records[0:3]
>>> emp_name
'john'
>>> emp_dept
'hr'
>>> emp_join_date
2010
```

#### 3. Using Tuples In Functions As Return Vlaues(함수에서 반환값으로 Tuple 사용)

보통 함수는 하나의 값만 반환한다. 하지만 함수의 반환값으로 Tuple과 Set을 생성할 수 있다.

다시 말해 여러 값들을 합치고 Tuple에 저장한 후 반환할 수 있다. 작업에서 소모된 시, 분, 초를 알고 싶을 때나 서로 다른 타입의 액세서리들의 갯수를 구하거나 특정 저자에 의해 쓰여진 다양한 책들의 가격과 같은 상황에서 아주 편리하다.

```python
def square(n1, n2):
    return (n1*n1, n2*n2)

print(type(square(2, 3)))

# output
<class 'tuple'>
```

#### 4. Mixed Data Structures In The Form Of Tuples(Tuple의 형태에서 혼합된 데이터 구조)

Tuple은 요소로 또 다른 Tuple을 포함할 수 있는 컨테이너 타입이다. 이를 Nested Tuple이라고 부른다.

이는 보다 넓은 수준에서 정보를 시각화하는데 도움을 준다. 예를 들면 이름, 직위, 월급과 함께 각 부서마다 직원 수를 유지해야 한다면 Nested Tuple이 효율적으로 도와줄 것이다.

```python
employes = [
   ("HR", 2, [('david', 'manager', 100000), ('bruno', 'asst. manager', 50000)])
   ("IT", 2, [('kirk', 'team lead', 150000), ('matt', 'engineer', 45000)])
   ("Sales", 2, [('billy', 'sales lead', 250000), ('tom', 'executive', 95000)])
   ]
```

질문이나 수정부분은 댓글또는 [인스타그램](https://www.instagram.com/monseungmon/), [트위터](https://twitter.com/kim_seungbeom) 등으로 연락부탁드려요!
