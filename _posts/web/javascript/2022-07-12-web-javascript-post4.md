---
layout: post
title: 타입스크립트 기본 타입
description: >
  인프런 타입스크립트 시작하기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - javascript
---

# Typescript

* toc
{:toc .large-only}

__enum 타입__

```js
enum Fruit {
  Apple,  //0
  Banana = 5, //5
  Orange  //6
}
const v1: Fruit = Fruit.Apple;
const v2: Fruit.Apple | Fruit.Banana = Fruit.Banana;
```

- 번호 따로 지정해주지 않으면 자동 0으로 설정
- 번호를 지정해주면 그 다음 요소는 +1 해서 할당됨 

```js
enum Fruit {
  Apple,
  Banana = 5,
  Orange,
}
console.log(Fruit.Banana);  //5
console.log(Fruit['Banana']); //5
console.log(Fruit[5]);  //Banana
```
양뱡향 매핑인 것을 확인할 수 있다

```js
enum Language {
  Korean = "ko",
  English = "en",
  Japanese = "jp"
}
```
- 숫자가 아닌 문자열 대입도 가능
- 단방향 매핑

__enum 함수__

```js
function getIsValidEnumValue(enumObject: any, value: number | string) {
  return Object.keys(enumObject)
    .filter((key) => isNaN(Number(key)))
    .some((key) => enumObject[key] === value);
}
```
- 특정 value가 있는지 검사하는 함수

__const enum__

```js
const enum Fruit {
  Apple,
  Banana,
  Orange
}
```
- 컴파일 결과에 `enum` 객체를 남기지 않음
- 불필요하게 파일이 커지는 것을 막음

<span style="font-size:70%">[참조] 인프런 타입스크립트 시작하기

끝!