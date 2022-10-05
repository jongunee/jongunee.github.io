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

# css - 스타일 바꾸기 2

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

<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
