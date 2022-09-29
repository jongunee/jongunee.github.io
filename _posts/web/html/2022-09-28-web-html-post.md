---
layout: post
title: css - 스타일 바꾸기
description: >
  노마드코더 코코아톡 클론코딩 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - html
---

# css 꾸미기

- toc
{:toc .large-only}

## css

### display

**inline-block**

```css
<style>
  div{
    display: inline-block;
    width: 50px;
    height: 50px;
    background-color: teal;
  }
</style>
```
- `<div>`는 `inline`이기 떄문에 `width`나 `height` 속성을 가질 수 없지만 `inline-block`으로 설정해서 속성을 줄 수 있음
- 하지만 `inline-block`은 반응형 디자인을 지원하지 않음

**flex**

```css
<style>
  body {
    height: 100vh;
    margin: 20px;
    display: flex;
    justify-content: space-evenly;
    flex-direction: column;
    align-items: center;
  }
</style>
```

- `vh`는 viewpoint height로 화면 전체를 의미
- `flex` 속성은 자식에게 직접 명시하는 것이 아니라 부모에게 명시
- `<div>`에 명시하는 것이 아니라 `<body>`에 명시
- `space-evenly`는 빈 공간을 같은 크기로 나누어 고르게 배치
- `flex-directions`는 default로 `row`로 설정되어 있어 `column`으로 기준을 바꿀 수 있음
- `align-items`에 `center`로 주어 중앙으로 정렬 가능

### fixed

```css
body {
  height: 1000vh;
  margin: 20px;
}
div {
  position: fixed;
  width: 300px;
  height: 300px;
  color: white;
  background-color: teal;
}
```

- `<body>`에 `height`를 길게 설정해두고 `<div>`를 `fixed`로 설정해 주면 스크롤을 내리더라도 같은 위치에 고정
- 고정되는 위치는 처음 생성된 위치에서 고정
- `top`, `left` 등 하나라도 위치를 정해주면 다른 Layer로 바뀌어 최상단 기준으로 위치가 고정됨

  ```css
  #different {
    top: 5px;
    position: fixed;
    background-color: wheat;
    width: 350px;
  }
  ```

### relative

```css
.green {
  background-color: teal;
  height: 100px;
  position: relative;
  top: -10px;
  width: 100px;
}
```

- `position`을 `relative`로 설정해 주면 상대 위치로 지정이 가능
- `absolute`로 설정하면 가장 가까운 relative한 부모를 찾아 그 위치를 기준으로 함
- `position`의 default 값은 `static`

### Pseudo Selectors

```css
div:last-child {
  background-color: teal;
}
```
- Pseudo Selectors는 별도 id나 class를 지정해주지 않고 마지막 위치에 있는 tag 영역을 꾸미는 등의 상황에서 사용

```css
span:nth-child(2),
span:nth-child(4) {
  background-color: teal;
}
```

- 2, 4 번째 `<span>` 의 배경색 변경

```css
span:nth-child(even) {
  background-color: teal;
}
```

또는

```css
span:nth-child(2n) {
  background-color: teal;
}
```

- 일일이 쓰지 않고 짝수 번째마다 적용되도록 할 수도 있음

```css
p span {
  color: teal;
}
```

```html
<p>My name is <span>JW Park</span></p>
```

- `<p>` 내부에 있는 `<span>` 지정해서 스타일 적용

```css
div > span {
  text-decoration: underline;
}
```

- `<span>`이 여러개가 있을 때 `<div>` 바로 아래에 있는 `<span>`을 지정해서 스타일 적용

```css
p + span {
  text-decoration: underline;
}
```

- `<span>`이 여러개가 있을 때 `<p>` 바로 뒤에 있는 `<span>`을 지정해서 스타일 적용

```css
p ~ span {
  text-decoration: underline;
}
```

- `<span>`이 여러개가 있을 때 `<p>` 뒤에 있는 `<span>`을 지정해서 스타일 적용
- 꼭 바로 뒤에 있지 않아도 됨

```css
input[placeholder~="name"] {
  background-color: pink;
}
```

```html
<input type="text" placeholder="First name" />
<input type="text" placeholder="Last name" />
```

- `placehoder`가 name을 포함하고 있는 모든 `text` 타입에 스타일 적용  

<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
