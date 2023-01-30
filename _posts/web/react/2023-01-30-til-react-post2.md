---
layout: post
title: Props
description: >
  [참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기
sitemap: true
hide_last_modified: true
categories:
  - web
  - react
---

# Props

* toc
{:toc .large-only}

## Props

```js
function Btn({ text, big }) {
  return (
    <button
      style={{
        backgroundColor: "tomato",
        color: "white",
        padding: "10px 20px",
        border: 0,
        borderRadius: 10,
        fontSize: big ? 18 : 16,
      }}
    >
      {text}
    </button>
  );
}
```

- 기본 버튼 틀
- 인자로 받은 text 입력
- 이벤트 리스너가 아니기 때문에 prop를 가져와 직접 넣어주어야 함

```js
function App() {
  return (
    <div>
      <Btn text="Save Changes" big={true} />
      <Btn text="Continue" big={false} />
    </div>
  );
}
```

- 컴포넌트 생성 시 Props에 인자를 주어서 커스텀이 가능
- 넘겨주는 인자의 이름과 위 함수에서 받는 인자의 이름은 반드시 같아야 함 ex) text, big

**Prop - 함수 전달**

```js
function Btn({ text, onClick }) {
  return (
    <button
      onClick={onClick}
      style={{
        ...
      }}
    >
      {text}
    </button>
  );
}
```

- 함수 컴포넌트를 인자로 받음

```js
function App() {
  const [value, setValue] = React.useState("Save Changes");
  const changeValue = () => setValue("Revert Changes");
  return (
    <div>
      <Btn text={value} onClick={changeValue} />
      <Btn text="Continue" />
    </div>
  );
}
```

- 여기서 `onClick`은 그저 전달하는 변수이기 때문에 위 코드 `Btn` 함수에서 처리를 해주어야 동작이 가능

## Prop Types

자바스크립트는 type을 따로 정의하지 않기 때문에 Prop Types를 정의해 주어서 타입이 일치하지 않으면 경고를 띄울 수 있다.

```js
Btn.propTypes = {
  text: PropTypes.string.isRequired,
  fontSize: PropTypes.number,
};
```

- `text: PropTypes.string.isRequired`: text는 string 타입만 넣어줄 수 있도록 설정
- `isRequired`: 필수 인자로 설정

```js
function Btn({ text, fontSize = 20 }) {
  return (
    <button
      style={{
        backgroundColor: "tomato",
        color: "white",
        padding: "10px 20px",
        border: 0,
        borderRadius: 10,
        fontSize,
      }}
    >
      {text}
    </button>
  );
}
```

- `fontSize = 20`: 아래 코드의 2번째 버튼처럼 `fontSize`를 입력해주지 않았다면 디폴트 값으로 20 입력
- 첫 번째 버튼은 18 그대로 입력됨

```js
function App() {
  return (
    <div>
      <Btn text="Save Changes" fontSize={18} />
      <Btn text="Continue" />
    </div>
  );
}
```






<span style="font-size:70%">[참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기</span>

끝!