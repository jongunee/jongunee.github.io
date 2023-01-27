---
layout: post
title: React 시작
description: >
  [참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기
sitemap: true
hide_last_modified: true
categories:
  - web
  - react
---

# React 시작

* toc
{:toc .large-only}

## React 사용하기

html 파일에 다음 코드를 추가하여 사용

```html
<script src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
```

- React: Interactivity의 원동력
- React-Dom: React Element를 가져다가 HTML로 바꾸는 역할

**html 파일에 span 생성**

```js
const root = document.getElementById("root");
const h3 = React.createElement(
  "h3",
  {
    onMouseEnter: () => console.log("mouse enter"),
  },
  "Hello I am a span"
);
const btn = React.createElement(
  "button",
  {
    onClick: () => console.log("Clicked!!!"),
    style: {
      backgroundColor: "tomato",
    },
  },
  "Click me"
);
const container = React.createElement("div", null, [h3, btn]);
ReactDOM.render(container, root);
```

- `root` id를 가진 HTML Element 저장
- `React.createElement()`: `span` Element 생성 및 Property 설정
- `React.createElement("div", null, [h3, btn])`: `div`를 생성하는데 그 하위에 위에 생성한 h3 > btn을 가지고 생성 
- `ReactDOM.render(container, root)`: `root`에 생성한 `container` Element를 뿌려 보여주기

## JSX

위의 `h3`와 `btn` Element를 생성하는 코드를 다음과 같이 수정 가능

**h3**

```js
const Title = (
  <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
    Hello I am a title
  </h3>
);
```

**btn**

```js
const Button = (
  <button
    style={{
      backgroundColor: "tomato",
    }}
    onClick={() => console.log("Clicked!!!")}
  >
    Click me
  </button>
);
```

하지만 이렇게 코드를 작성하면 에러가 발생한다. 이 코드를 해석할 수 있는 **Babel**이 필요하다.

**Babel Import**

```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```

**Babel 적용**
```html
<script type="text/babel">
  const root = document.getElementById("root");
  const Title = () => (
    <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
      Hello I am a title
    </h3>
  );
  const Button = () => (
    <button
      style={{
        backgroundColor: "tomato",
      }}
      onClick={() => console.log("Clicked!!!")}
    >
      Click me
    </button>
  );

  const Container = () => (
    <div>
      <Title />
      <Button />
    </div>
  );
  ReactDOM.render(<Container />, root);
</script>
```

- 함수형 컴포넌트로 변경해주어야 하며 반드시 컴포넌트명은 반드시 대문자로 해준다






<span style="font-size:70%">[참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기</span>

끝!