---
layout: post
title: 타입스크립트 기본
description: >
  노마드코더 Typescript로 블록체인 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - javascript
---

# 타입스크립트 기본

* toc
{:toc .large-only}

## 기본 문법

```ts
type Age = number;
type Name = String;
```

타입 설정 가능

```ts
type Player = {
  name: Name;
  age?: Age;
};
```

`?`를 붙이면 옵션이라는 의미로 age를 써도 되고 안써도 됨

```ts
const playerMaker = (name: Name): Player => ({ name });
const jw = playerMaker("Jongwon");
jw.age = 12;
```

`Player` 타입이라고 선언해주었기 때문에 `age`를 추가할 수 있음

```ts
type Player = {
  readonly name: Name;
  age?: Age;
};

const names: readonly string[] = ["1", "2"];
```

- `readonly` 속성이기 때문에 `name` 수정 불가능
- `names[]` 배열도 `push` 등의 수정 불가능

## 기타 타입

**`any` 타입**

- `any` 타입의 변수로 선언하면 어떠한 타입의 값도 들도 넣을 수 있다
- typescript 규칙에서 벗어나 javascript 규칙으로 구현하고 싶다면 사용
- 왠만하면 권장하지 않음

**`unknown` 타입**

- 어떤 타입의 값이 들어갈지 모를 때 사용
- API 응답으로 어떤 타입이 올지 모를 때 주로 사용

**`void` 타입**

- 함수에서 리턴되는 값이 없을 때 사용되는 타입

**`never` 타입**

- 함수에서 절대로 리턴하지 않을 때 사용되는 타입
- 함수에서 예외가 발생할 때 주로 사용
- 아래에서 정상적으로 동작했다면 `name`에 `string` 또는 `number` 타입의 값이 들어올 것이기 때문에 else 조건에서 `name`은 `never` 타입이 됨

```ts
function hello(name: string | number) {
  if (typeof name === "string") {

  } else if (typeof name === "number") {

  } else {

  }
}
```

__함수 타입__

```ts
type Add = (a: number, b: number) => number;
const add: Add = (a, b) => a + b;
```

`add`가 `Add` 타입을 참조하기 때문에 매개변수의 타입을 지정해줄 필요가 없다

## 오버로딩

```ts
type Add = {
  (a: number, b: number): number
  (a: number, b: string): number
}
const add: Add = (a, b) => {
  if(typeof b === "string") return a;
  return a + b;
}
```
- 예시처럼 call signature가 여러개일 경우 사용됨

__실사용 예시__

```ts
type Config = {
  path: string,
  state: object
}
type Push = {
  (path: string): void
  (config: Config): void
}

const push: Push = (config) => {
  if(typeof config == "string") {
    console.log(config)
  }else{
    console.log(config.path, config.state)
  }
}
```

## 다형성

__Generic 타입 적용 전__
```ts
type SuperPrint = {
  (arr: number[]): void
  (arr: boolean[]): void
  (arr: string[]): void
}
const superPrint: SuperPrint = (arr) => {
  arr.forEach(i => console.log(i))
}

superPrint([1, 2, 3, 4])
superPrint([false, true, true])
superPrint(["a", "b", "c"])
```

__Generic 타입 적용 후__

```ts
type SuperPrint = {
  <TypePlaceholder>(arr: TypePlaceholder[]): void
}
const superPrint: SuperPrint = (arr) => {
  arr.forEach(i => console.log(i))
}

superPrint([1, 2, false, "abc"])
```
타입 유추가 가능해진다

```ts
type SuperPrint = (arr: any[]) => any
```
`any`를 사용해서 구현할 수도 있지만 타입을 유추하지 못한다.

```ts
type Player<E> = {
  name: string
  extraInfo: E
}
type JwExtra = {
  favFood: string
}
type JwPlayer = Player<JwExtra>
cost JwPlayer = {
  name: "jongwon"
  extraInfo: {
    favFood: "Kimchi"
  }
}
```

<span style="font-size:70%">[참조] 노마드코더 Typescript로 블록체인 만들기

끝!
