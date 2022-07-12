---
layout: post
title: 자바스크립트 시작, 변수
description: >
  스파르타코딩클럽 JavaScript 문법 뽀개기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - javascript
---

# Javascript

* toc
{:toc .large-only}

## Javascript란?

> 프로그래밍 언어 중 하나로 주로 HTMl과 CSS로 만들어진 웹페이지를 동적으로 변경해주는 언어

__사용 환경__

1. 웹 브라우저  
    - 웹 페이지 동적 변경
    - ex) `alert` 함수를 이용한 알림 기능

2. 탈 웹
    - 웹 서버 등
    - node.js 처럼 웹 브라우저 뿐만 아니라 서버를 개발하는 데에도 사용

3. SpreadSheet
    - Google Apps Script와 같은 스프레드시트에서도 사용됨


## Hello world 출력

```js
console.log('Hello World!!')
```

__실행 방법__
- 터미널 실행

```bash
node <파일명>.js
```

__주석 표기:__
```js
//주석 표기 방법
```

## 변수

자바스크립트 변수 선언 방법
```js
let <변수이름> = <값>
const <변수이름> = <값>
```
- `const`는 변수가 고정된 값을 계속 갖고 있어 값을 재할당할 필요 없을 경우 사용
- Javascript는 세미콜론(;) 생략 가능

```js
const myAge = 28
const yourAge = 45
const firstName = 'Jongwon'
const lastName = 'Park'
const isMan = true
const isWoman = false
```

```js
let name1 = null
console.log(name) // null 출력
let name2
console.log(name2) // undefined 출력
```
- `null`은 비어있는 값 의미
- `undefined`는 변수 선언만 하고 값 할당되지 않은 것


<span style="font-size:70%">[참조] 스파르타코딩클럽 JavaScript 문법 뽀개기

끝!