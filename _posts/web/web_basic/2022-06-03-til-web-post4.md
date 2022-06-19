---
layout: post
title: Javascript 기초
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - web_basic
---

# Javascript 기초

* toc
{:toc .large-only}

## 자바스크립트란?

> 프로그래밍 언어 중 하나로 브라우저가 알아들을 수 있는 언어

- 다른 언어로 구현할 수도 있지만 자바스크립트로 설계하도록 약속한 표준
- JAVA, Javascript와의 관련은 없음

## JS 기초 문법

__head - script 구현__

```js
<script>
    function hey(){
        alert('안녕!');
    }
</script>
```

__버튼에 함수 호출__

```javascript
<div class="mytitle">
    <h1>내 생애 최고의 영화들</h1>
    <button onclick="hey()">영화 기록하기</button>
</div>
```

브라우저 개발자 도구 또는 우클릭 - 검사 - console 탭에 개발자용 툴이 제공이 된다. 이 탭에서 빠르게 자바스크립트를 테스트 할 수 있다. 
- __윈도우:__ `F12`
- __맥:__ `alt` + `cmd` + `i`

## 변수 선언

```js
let a = 1;
let b = 2;
let first_name = 'jongwon';
let last_name = 'Park';
```

## 리스트

```js
let a_list = [];
let b_list = ['수박', '참외', '배'];

b_list.push('사과');
b_list.length;
```

## 딕셔너리

```js
let a_dict = {'name':'bob','age':28};
a_dict['name']  // 'bob' 출력

a_dict['height'] = 180; // 추가 가능
```

## 기본 함수

```js
let myemail = 'jejeoppa@naver.com';
myemail.split('@')[1];   //naver.com 출력
myemail.split('@')[1].split('.')[0]; //naver 출력
```

## 함수 만들기

```js
<script>
    function sum(a, b){
        alert('계산해 보자');
        return a + b;
    }
    let result = sum(2, 3);
    alert(result);
</script>
```
- '계산해 보자' 알람 -> '5' 알람

```js
<script>
    function sum(a, b){
        console.log('계산해 보자');
        return a + b;
    }
    let result = sum(2, 3);
    console.log(result);
</script>
```
- 알람 대신 console에 log로 띄울 수 있음

## 조건문

```js
function is_audult(age){
    if(age > 20){
        alert('성인입니다.');
    }
    else{
        alert('청소년입니다.');
    }
}
```

## 반복문

```js
let a_list = ['사과', '배', '감', '딸기'];
for(let i = 0; i < a_list.length; i++){
    console.log(a_list[i]);
}
```

```js
let scores = [
    {'name': '철수', 'score': 90},
    {'name': '영희', 'score': 85},
    {'name': '민수', 'score': 70},
    {'name': '형준', 'score': 50},
    {'name': '기남', 'score': 68},
    {'name': '동희', 'score': 30},
]
for (let i = 0; i < scores.length; i++) {
    if (scores[i]['score'] < 70) {
        console.log(scores[i]['name']);
    }
}
```
- '형준', '기남', '동희' 출력



끝!