---
layout: post
title: Effects
description: >
  [참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기
sitemap: true
hide_last_modified: true
categories:
  - web
  - react
---

# Effects

* toc
{:toc .large-only}

## Effects

**App.js**

```js
function App() {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prev) => prev + 1);
  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}
```

카운터가 증가할 때마다 증가된 값을 보여주지만 매번 App 함수를 실행시켜 재생성 된다는 문제점이 있다.

**useEffect**

```js
function App() {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prev) => prev + 1);
  console.log("i run all the time");
  const iRunOnlyOnce = () => {
    console.log("i run only once");
  };
  useEffect(iRunOnlyOnce, []);
  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}
```

- `useEffect()` 함수를 사용하면 해당 코드를 한 번만 실행될 수 있도록 보

**결과**

![그림1](/assets/img/react/effect_log.png)

**useEffect 활용**

```js
function App() {
  const [counter, setValue] = useState(0);
  const [keyword, setKeyword] = useState("");
  const onClick = () => setValue((prev) => prev + 1);
  const onChange = (event) => setKeyword(event.target.value);
  console.log("I run all the time");
  useEffect(() => {
    console.log("I run only once");
  }, []);
  useEffect(() => {
    console.log("I run when 'keyword' changes");
  }, [keyword]);
  useEffect(() => {
    console.log("I run when 'counter' changes");
  }, [counter]);
  
  ...
}
```

- `useEffect()`를 이용해서 상황에 따라 실행시킬 수 있음
- `useEffect(keywordCheck, [keyword])`: keyword 상태가 변할 때만 keywordChekck 함수를 실행

## Cleanup

**Show/Hide 버튼**

```js
function App() {
  const [showing, setShowing] = useState(false);
  const onClick = () => setShowing((prev) => !prev);
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}
```

```js
function Hello() {
  function byeFn() {
    console.log("bye :(");
  }
  function hiFn() {
    console.log("hi :)");
    return byeFn;
  }
  useEffect(hiFn, []);
  return <h1>Hello</h1>;
}
```

- 생성될 때는 `hiFn()` 함수가 실행되고 사라질 때는 `byeFn()` 함수 실행
- 하지만 자주 사용되는 형태는 아님

**수정된 Hello**

```js
function Hello() {
  useEffect(() => {
    console.log("hi :)");
    return () => console.log("bye :(");
  }, []);
  return <h1>Hello</h1>;
}
```

- 직접 선언해서 사용할 수도 있지만 더욱 길어지기 때문에 화살표 함수를 이용



<span style="font-size:70%">[참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기</span>

끝!