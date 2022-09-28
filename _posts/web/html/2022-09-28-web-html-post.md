---
layout: post
title: css - display
description: >
  노마드코더 코코아톡 클론코딩 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - html
---

# css

- toc
{:toc .large-only}

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


<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
