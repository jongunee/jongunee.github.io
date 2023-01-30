---
layout: post
title: React State
description: >
  [참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기
sitemap: true
hide_last_modified: true
categories:
  - web
  - react
---

# React State

* toc
{:toc .large-only}

## State

**count 버튼 생성**

```js
const root = document.getElementById("root");
let counter = 0;
function counterUp() {
  counter = counter + 1;
  render();
}
function render() {
  ReactDOM.render(<Container />, root);
}
function Container() {
  return (
    <div>
      <h3>Total clicks: {counter}</h3>
      <button onClick={counterUp}>Click me</button>
    </div>
  );
}
render();
```

- `ReactDOM.render(<Container />, root)`: 함수형 컴포넌트 `Container`를 `root` 위치에 뿌림
- 자동으로 전체를 재생성하는 것이 아니라 React가 바뀐 부분만 업데이트를 하도록 도와줌

**setState**

위 코드를 아래와 같이 간단하게 간소화 시킬 수가 있다.

```js
const root = document.getElementById("root");
function App() {
  const [counter, setCounter] = React.useState(0);
  const onClick = () => {
    setCounter(counter + 1);
  };
  return (
    <div>
      <h3>Total clicks: {counter}</h3>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}
ReactDOM.render(<App />, root);
```

- `React.useState(0)`: 해당 함수는 state 변수와 해당 변수를 갱신할 함수를 반환
- 자동으로 새로운 값을 가지고 리렌더링이 되기 때문에 버튼 클릭 함수에 `render` 함수를 호출할 필요가 없음 

위 `setCounter(counter + 1)`은 오류 방지를 위해 다음과 같이 함수 형태로 전달할 수도 있다.

```js
const onClick = () => {
  setCounter((current) => current + 1);
};
```

## 분/시 변환기

```js
function App() {
  const [amount, setAmount] = React.useState();
  const [inverted, setInverted] = React.useState(false);
  const onChange = (event) => {
    setAmount(event.target.value);
  };
  const reset = () => setAmount(0);
  const onFlip = () => {
    reset();
    setInverted((current) => !current);
  };
  return (
    <div>
      <h1>Super Converter</h1>
      <div>
        <label htmlFor="minutes">Minutes</label>
        <input
          value={inverted ? amount * 60 : amount}
          id="minutes"
          placeholder="Minutes"
          type="number"
          onChange={onChange}
          disabled={inverted}
        />
      </div>
      <div>
        <label htmlFor="hours">Hours</label>
        <input
          value={inverted ? amount : Math.round(amount / 60)}
          id="hours"
          placeholder="Hours"
          type="number"
          onChange={onChange}
          disabled={!inverted}
        />
      </div>
      <button onClick={reset}>Reset</button>
      <button onClick={onFlip}>Flip</button>
    </div>
  );
}
```

- `const [amount, setAmount] = React.useState()`: 입력된 `amount` 값을 받아 함수로 리렌더링 실행
- `value={flipped ? amount * 60 : amount}`: `inverted` 상태에 따라 Hour를 계산해서 출력
- Flip을 통해 분 변환 <-> 시 변환 

**결과 화면**

![그림1](/assets/img/react/super_converter.png)

## 옵션 선택

옵션을 주어 컨버터를 선택 리렌더링 하도록 한다.
위 코드는 별도에 `MinutesToHours` 명의 함수로 바꿔주고 추가로 `KmToMiles` 함수를 만든 후 `app` 에서 호출

```js
function MinutesToHours(){...}
function KmToMiles(){...}
function App() {
  const [index, setIndex] = React.useState("xx");
  const onSelect = (event) => {
    setIndex(event.target.value);
  };
  return (
    <div>
      <h1>Super Converter</h1>
      <select value={index} onChange={onSelect}>
        <option value="xx">Select your units</option>
        <option value="0">Minutes & Hours</option>
        <option value="1">Km & Miles</option>
      </select>
      <hr />
      {index === "xx" ? "Please select your units" : null}
      {index === "0" ? <MinutesToHours /> : null}
      {index === "1" ? <KmToMiles /> : null}
    </div>
  );
}
```

- 위 방식과 동일하게 옵션 선택시 발생하는 이벤트에 따라 처리
- `{index === "0" ? <MinutesToHours /> : null}`: **Minutes & Hours** 선택시 `index` 상태변수에 0이 들어가고 해당되는 컴포넌트를 뿌려줌
- 중괄호가 들어간 이유는 텍스트가 아닌 js를 사용하기 위함


**결과**

![그림2](/assets/img/react/final_super_converter.png)




<span style="font-size:70%">[참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기</span>

끝!