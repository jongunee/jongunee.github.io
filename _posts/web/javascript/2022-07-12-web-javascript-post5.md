---
layout: post
title: 타입스크립트 함수 타입
description: >
  인프런 타입스크립트 시작하기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - javascript
---

# 함수 타입

* toc
{:toc .large-only}

## 함수 타입

```js
function getText(name: string, age: number): string {
  const nameText = name.substr(0, 10);
  const ageText = age >= 35 ? "senior" : "junior";
  return `name: ${nameText}, age: ${ageText}`;
}
```

```js
const getText: (name: string, age: number) => string = function (name, age) {
  return "";
};
```

```js
function getText(name: string, age: number, language?: string): string {
  const nameText = name.substr(0, 10);
  const ageText = age >= 35 ? 'senior' : 'junior';
  const languageText = language ? language.substr(0, 10) : '';
  return `name: ${nameText}, age: ${ageText}, language: ${languageText}`;
}
getText('mike', 23, 'ko');
getText('mike', 23);
getText('mike', 23, 123);
```
- 파라미터 뒤에 ?가 붙으면 Optional 파라미터

```js
function getText(name: string, language?: string, age: number): string {}

getText('mike', 23);
```
- Optional 파라미터가 중간에 오면 2번째 파라미터가 language 인지 age인지 알 수가 없기 때문에 에러 발생

```js
function getText(name: string, age: number = 15, language = 'korean'): string {
  return '';
}
console.log(getText('mike'));
console.log(getText('mike', 23));
console.log(getText('jone', 36, 'english'));
```
- 두 번째, 세 번째 파라미터는 Optional 파라미터


```js
function getText(name: string, ...rest: number[]): string {
  return '';
}

console.log(getText('mike', 1, 2, 3));
```
- 두 번째 매개변수부터 모두 Rest 파라미터라고 함
- 따로 지정할 필요 없이 모두 number 배열에 담겨 숫자를 입력 받음

## this

__화살표 함수 (JS)__

```js
function Counter() {
  this.value = 0;
  this.add = (amount) => {
    this.value += amount;
  };
}
```

__일반 함수 (JS)__

```js
function Counter2() {
  this.value = 0;
  this.add = function (amount) {
    this.value += amount;
    // console.log(this === global);
  };
}
```
위 동작과 동일

```js
const counter2 = new Counter2();
console.log(counter2.value);  //0
counter2.add(5);  //여기서 this는 counter2
console.log(counter2.value);  //5
const add2 = counter2.add;
add2(5);  //주체가 따로 없어 this는 전역개체 -> 전역개체의 coutner가 증가한 것
console.log(counter2.value);  //5
```

- 일반 함수에서는 주체가 따로 없으면 this가 전역개체(global)를 가리킴 ➡️동적
- 화살표 함수에서는 this가 화살표 함수가 생성될 당시 this를 가리킴 ➡️정적

__함수 (TS)__

```ts
function getParam(this: string, index: number): string {
  const params = this.split(",");
  if (index < 0 || params.length <= index) {
    return "";
  }
  return this.split(",")[index];
}
```
- this의 type을 정의해 줘야 함
- this type만 정의해 주고 매개변수는 두 번째 매개변수부터 시작함




<span style="font-size:70%">[참조] 인프런 타입스크립트 시작하기

끝!