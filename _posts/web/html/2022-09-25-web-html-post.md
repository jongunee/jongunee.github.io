---
layout: post
title: css - 기본
description: >
  노마드코더 코코아톡 클론코딩 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - html
---

# css - 기본

- toc
{:toc .large-only}

## css로 꾸미기

html 파일을 꾸미는 방법에는 두 가지 방법이 있다.

- 하나의 html 파일 내에서 꾸미기
```html
  <head>
    <title>Home - My first website.</title>
    <style>
      여기에 코드 작성
    </style>
  </head>
```
- css 파일 분리해서 꾸미기 - 추천

```html
<link href="style.css" rel="stylesheet" />
```

`style.css` 파일을 생성해 코드를 작성하고 html 파일에서 링크를 해주면 됨

**css - Cascading**

- 브라우저는 css의 의미처럼 코드를 순차적으로 읽음
- 따라서 같은 대상의 스타일을 적용시키면 순서상 마지막 코드가 적용됨

**block & inline**

- `block`: `<div>`처럼 옆에 아무것도 올 수 없음
- `inline`: `<span>`처럼 옆에 다른 요소들이 올 수 있음

```css
span {
  display: block;
  background-color: turquoise;
}
```
- default 값으로 `<span>`은 `inline`으로 설정
- `display` 속성으로 `block`으로 바꿀 수 있으며 그 반대도 가능
- `inline`은 `height`나 `weight` 속성을 가지지 않음 주의

### margin & padding

> margin은 box의 border로부터 바깥에 있는 공간을 의미

브라우저는 따로 지정하지 않더라도 기본적으로 몇 가지 속성을 적용하는데 `margin`을 8px로 지정 

> `padding`은 box의 border로부터 안쪽에 있는 공간을 의미

```css
#firstDiv {
  background-color: whitesmoke;
  padding: 10px;
}
```
```html
<body>
  <div id="firstDiv"></div>
</body>
```

- `<div>`가 여러 개 있으면 id를 지정해 스타일 적용 가능
- id는 앞에 #을 넣어주어야 함
- `padding`으로 안쪽 여백 지정 가능

### border

border 스타일 적용

```css
<style>
  * {
    border: 2px solid black;
  }
</style>
```
- border 속성: `<line-width>` || `<line-style>` || `<color>` 


```css
span {
  background-color: teal;
  padding: 20px;
  margin: 30px;
}
```

- `margin`에 값을 넣어주어도 양쪽으로만 적용됨
- 이유는 `inline`은 높이 속성을 갖지 않기 떄문

### classes

`<span>`에 다른 스타일을 적용하기 위해

```html
  <body>
    <span>hello</span>
    <span id="tomato">hello</span>
    <span>hello</span>
    <span id="tomato2">hello</span>
    <span>hello</span>
    <span id="tomato3">hello</span>
</body>
```

각각 id를 지정하고

```css
<style>
  #tomato,
  #tomato2,
  #tomato3 {
    background-color: tomato;
  }
</style>
```

묶어서 정의를 해준다. 하지만 각각 다른 id를 지정하는 것은 비효율적

```html
<body>
  <span>hello</span>
  <span class="tomato">hello</span>
  <span>hello</span>
  <span class="tomato">hello</span>
  <span>hello</span>
  <span class="tomato">hello</span>
</body>
```

`class`로 지정 해주고

```css
<style>
  .tomato{
    background-color: tomato;
  }
</style>
```

`.<class이름>`으로 스타일을 적용하면 됨 

```html
<span class="btn tomato hello">hello</span>
```

- `id`와 다르게 하나의 `<span>`에 여러 `class`를 지정하는 것도 가능

<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
