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

## 정적 타입 언어 VS 동적 타입 언어

| 정적 타입 언어 | 동적 타입 언어 |
|--- | --- |
| 진입 장벽이 높음 | 진입 장벽이 낮음
| 코드 양이 많을 때 생산성이 높음 | 코드 양이 적을 때 생산성이 높음 |
| 타입 오류가 컴파일 시 발견 | 타입 오류가 런타임 시 발견 |

## 타입스크립트를 사용하는 이유

- 자바스크립트를 사용하면 IDE가 Type을 모르기 때문에 에러를 발견하기 어려움
- 타입스크립트를 사용하면 IDE가 Type을 알기 때문에 잘못된 속성 이름 입력시 오류 발생
- 리팩토링 시에 객체에 해당되는 변수만 수정 가능
- 코드 이동이 간편

## 컴파일 및 실행

typescript 파일을 javascript로 변환해서 컴파일함

```bash
npx tsc
node src/1.js
```

VScode - CodeRunner extensions 사용하면 편리
- `ctrl` + `alt` + `n`

TypeScript - Playground 에서 컴파일 가능

```ts
export {};  //코드 범위를 제한시켜 변수 충돌 방지

const a = 123;
const b = 'abc';
```

## 기본 타입

```ts
const size: number = 123;
const isBig: boolean = size >= 100;
const msg: string = isBig ? "크다" : "작다";

const values: number[] = [1, 2, 3];
const values2: Array<number> = [1, 2, 3];
values.push("a"); //문자기 때문에 에러 발생

const data: [string, number] = [msg, size];
data[0].substr(1);
data[1].substr(1);  //number에는 substr 메서드가 없어 에러 발생

console.log("typeof 123 =>", typeof 123);
console.log('typeof "abc" =>', typeof "abc");
console.log("typeof [1, 2, 3] =>", typeof [1, 2, 3]);
```

```js
let v1: undefined = undefined;
let v2: null = null;
v1 = 123; //v1은 undefined만 가능한 타입이기 때문에 에러 발생

let v3: number | undefined = undefined; //다른 타입과 함께 사용 가능
v3 = 123;

console.log("typeof undefined =>", typeof undefined);
console.log("typeof null =>", typeof null);
```

__리터럴 타입__

```js
let v1: 10 | 20 | 30;
v1 = 10;
v1 = 15;  //15는 없기 때문에 에러 발생

let v2: "경찰관" | "소방관";
v2 = "의사";  //"의사"가 없기 때문에 에러 발생
```

```js
let value: any;
value = 123;
value = "456";
value = () => {};
```
- `any` 타입은 함수를 포함안 모든 타입의 값을 넣어줄 수 있음
- javascript에서 typescript로 포팅하는 경우 유용

__함수 반환 타입__

```js
function f1(): void {
  console.log("hello");
}
function f2(): never {
  throw new Error("some error");
}
function f3(): never {
  while (true) {
    // ...
  }
}
```
- 무한루프 때문에 종료되지 않는 함수 반환 타입은 `never` 타입으로 정의
- 자주쓰지는 않음

__객체 타입__

```js
let v: object;
v = { name: "abc" };
console.log(v.prop1); //객체 속성정보가 없기 때문에 에러 발생
```

__union, intersection 타입__

```js
let v1: (1 | 3 | 5) & (3 | 5 | 7);
v1 = 3;
v1 = 1; //3, 5만 가능하기 때문에 에러 발생
```

__타입 별칭__

```js
type Width = number | string;
let width: Width;
width = 100;
width = "100px";
```

<span style="font-size:70%">[참조] 인프런 타입스크립트 시작하기

끝!