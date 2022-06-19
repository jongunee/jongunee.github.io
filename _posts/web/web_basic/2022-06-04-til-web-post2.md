---
layout: post
title: Phython 기초
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - web_basic
---

# Python 기초 문법

* toc
{:toc .large-only}

## 변수 및 기본 연산

```py
a = 3
b = 5

print(a+b)
```

```py
first_name = jongwon
last_name = park

print(first_name + last_name)
```
- 변수 선언, `;` 필요 없음
- 들여쓰기가 굉장히 중요함

## 자료형

```py
a_list = ['사과', '배', '감']

print(a_list[1])

a_list.append('포도')   #리스트에 추가
```

```py
a_dict = {
    'name': 'bob'
    'age' : 27
}

print(a_dict['name'])   #bob 출력
```

## 함수

```py
def sum(a, b):
    return a + b

result = sum(1, 2)
print(result)
```

## 조건문

```py
def is_adult(age):
    if age > 20:
        print('성인입니다')
    else:
        print('청소년입니다')

is_adult(15)
```

## 반복문

```py
fruits = ['사과','배','배','감','수박','귤','딸기','사과','배','수박']

for fruit in fruits:
    print(fruit)
```

```py
fruits = ['사과','배','배','감','수박','귤','딸기','사과','배','수박']

count = 0
for fruit in fruits:
    if fruit == '사과':
        count += 1;

print(count)
```

```py
people = [{'name': 'bob', 'age': 20},
          {'name': 'carry', 'age': 38},
          {'name': 'john', 'age': 7},
          {'name': 'smith', 'age': 17},
          {'name': 'ben', 'age': 27}]

for person in people:
    if person['age'] > 27:
        print(person['name'])
```

끝!