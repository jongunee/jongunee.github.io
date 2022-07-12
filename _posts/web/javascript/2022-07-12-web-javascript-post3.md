---
layout: post
title: 타입스크립트 시작
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


<span style="font-size:70%">[참조] 인프런 타입스크립트 시작하기

끝!