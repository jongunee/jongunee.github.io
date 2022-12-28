---
layout: post
title: css - 스타일 바꾸기 3
description: >
  노마드코더 코코아톡 클론코딩 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - html
---

# css - 스타일 바꾸기 3

- toc
{:toc .large-only}

## transition

```css
a {
  color: wheat;
  background-color: tomato;
  text-decoration: none;
  padding: 3px 5px;
  border-radius: 5px;
  font-size: 55px;
  transition: background-color 10s ease-in-out;
}
a:hover {
  color: tomato;
  background-color: wheat;
}
```

- `transition`은 state에 따라 바뀌는 property가 있을때 사용
- `hover` 상태가 되었을 떄 글자는 바로 배경색으로 바뀌지만 배경색은 `transition` 설정으로 10초에 걸쳐 바뀌기 때문에 글자가 사라지는 효과가 생김
- `transition`은 처음 생겨난 element에 위치해야 함

### ease

```css
transition: background-color 10s ease-in-out;
```

- `ease-in-out`은 효과 발생 속도에 차이를 줌

```css
transition: background-color 10s cubic-bezier(0.785, 0.135, 0.150, 0.860);
```

- 다른 효과를 줄 수도 있음

다음 링크에서 테스트 가능 ➡️ [링크](https://matthewlein.com/tools/ceaser)

## transform

```css
img {
  border: 10px solid black;
  height: 200px;
  width: 200px;
  border-radius: 50%;
  transform: rotateY(30deg);
}
```

- `transform`에 `rotateY(30deg)`를 주면 3d로 y축 기준 30도 회전한 효과
- `transfrom`은 3D로 다른 층에서 동작하기 때문에 box element를 변형시키지 않음
- `scaleX()`로 배율 설정하는 등 다양한 옵션 제공

```css
img {
  border: 10px solid black;
  height: 200px;
  width: 200px;
  border-radius: 50%;
  transition: transform 5s ease-in-out;
}
img:hover {
  transform: rotateZ(90deg);
}
```

- `transition`과 `transform`을 조합해서 사용 가능

다음 링크에서 다양한 옵션들 확인 가능 ➡️ [링크](https://developer.mozilla.org/ko/docs/Web/CSS/transform)

## 애니메이션

**from-to**

```css
@keyframes coinFlip{
  from{
    transform: rotateY(0);
  }
  to{
    transform: rotateY(360deg);
  }
}
```
- `transform`을 주어 y축 기준으로 0도에서 360도까지 회전
- 애니메이션을 만들기 위해서는 `@keyframes` @규칙을 사용

```css
img {
...
  animation: coinFlip 5s ease-in-out;
}
```
- 위에서 생성한 애니메이션을 적용

**0% to 100%**

```css
@keyframes coinFlip{
  0%{
    transform: rotateY(0);
  }
  50%{
    transform: rotateY(180deg) translateY(100px);
  }
  100%{
    transform: rotateY(0deg);
  }
}
```
- `%`로 수치를 주어 각 단계마다 원하는 애니메이션 설정 가능
- 앞에 숫자를 바꿔 3단계 뿐만 아니라 더 다양한 애니메이션 설정 가능 

## Media Queries

```css
div {
  background-color: teal;
  width: 200px;
  height: 200px;
}
```
- 평상시 해당 div의 색상은 teal

```css
@media screen and (max-width: 600px) {
  div {
    background-color: tomato;
  }
}
```
- 너비가 600px 밑으로 내려가면 색상이 tomato 색상으로 바뀜
- 반드시 `div`와 같이 element를 지정해주어야 함
- element 지정 없이 그냥 쓸 수는 없음

```css
@media screen and (min-width: 700px) and (max-width: 1200px) and (orientation: landscape){
  ...
}
```
- 너비가 700px과 800px 사이에 있을 경우로 조건을 줄 수도 있음
- 화면이 landscape 모드(가로 모드)인지 조건도 줄 수 있음


<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
