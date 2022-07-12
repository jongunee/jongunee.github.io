---
layout: post
title: 자바스크립트 연산자, 함수, 클래스
description: >
  스파르타코딩클럽 JavaScript 문법 뽀개기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - javascript
---

# 연산자, 함수, 클래스

* toc
{:toc .large-only}

## 연산자

```js
console.log('My' + ' car') // My car 출력
console.log('1' + 2) // 12를 출력
```
- \+ 연산자로 문자열을 이어붙일 수 있음
- 문자열과 숫자를 이어붙이면 숫자를 문자로 인식

__템플릿 리터럴__

```js
const name = 'Jongwon Park'
console.log(`제 이름은 ${name}입니다`)
```
- ``백틱을 사용해서 간결하게 문자열을 붙여 표현 가능

__산술 연산자__

```js
console.log(10 + 1)
console.log(10 - 1)
console.log(10 / 2)
console.log(10 * 2)
console.log(10 % 3)
console.log(10 ** 2)
```

- **은 거듭제곱을 의미한다

__증감 연산자__
```js
let count = 1
console.log(++count)
```

__일치연산자__

```js
console.log(1 === "1") // false 출력
console.log(1 == "1" // true 출력
```
- `===`는 비교하는 두 값의 데이터타입과 값 자체가 정확히 일치해야만 true 리턴
- `==`는 데이터타입이 일치하지 않으면 데이터타입을 자동으로 변환하기 때문에 기존 다른 언어의 일치연산자와 다름
- 실수 유발 가능성이 있기 때문에 실무에서 거의 쓰지 않음

## 함수

__함수 선언__

```js
function 함수명(매개변수){
  코드

  return 반환값
}
```

__함수 호출__

```js
const 변수명 = 선언한 함수명(매개변수)
```

<span style="font-size:70%">[참조] 스파르타코딩클럽 JavaScript 문법 뽀개기

끝!