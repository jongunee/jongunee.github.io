---
layout: post
title: css - 스타일 바꾸기 2
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

## State

### active

```css
button:active {
  background-color: aquamarine;
}
```

- `active` state는 마우스로 클릭했을 때 상태를 의미
- 마우스 클릭했을 때 스타일 지정

### hover

```css
button:hover {
  background-color: aquamarine;
}
```

- `hover` state는 마우스로 클릭이 아닌 위에 올려두었을 때 상태의 스타일 지정

### focus

```css
input:focus {
  background-color: aquamarine;
}
```

```html
<input type="text" />
```

- 텍스트 입력창에서 입력을 하려고 할 때 강조되는 영역 상태의 스타일 지정

### visited

```css
a:visited {
  color: aquamarine;
}
```

```html
<a href="https://apple.com">Go to apple</a>
```

- 링크를 클릭하고 나서 클릭했다는 것을 표시하기 위한 상태 스타일 지정

### focus-within

```css
form {
  border: 1px solid salmon;
  display: flex;
  flex-direction: column;
  padding: 20px;
}
form:focus-within {
  border-color: green;
}
```

```html
<form>
  <input type="text" />
  <input type="text" />
  <input type="text" />
</form>
```

- `<form>` 안에 element 어떤 것이든 focus가 됐을 때 `<form>`의 스타일을 지정

## color

색상을 지정하는 방법은 여러 가지가 있다.

1. 색상 코드

    ```css
    p{
      color: #fccefc;
    }
    ```

2. RGB

    ```css
    p{
      color: rgb(252,206,0);
    }
    ```

    - 각각 red, green, blue를 의미

3. RGBA

    ```css
    p {
      background-color: rgba(252, 206, 0, 50%);
    }
    ```

    ```css
    p {
      background-color: rgba(252, 206, 0, 0.5);
    }
    ```

    - RGB에 A(투명도)가 추가


**변수 지정**

```css
:root {
  --main-color: #fcc300;
}
```

```css
p {
  background-color: var(--main-color);
}
```

- 색상을 변수로 지정해서 사용
- 변수는 위 예시 규칙을 따름

더 많은 속성들을 확인 하려면 ➡️ [링크](https://developer.mozilla.org/ko/docs/Web/CSS/Reference)


<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
