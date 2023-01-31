--
layout: post
title: React App 생성
description: >
  [참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기
sitemap: true
hide_last_modified: true
categories:
  - web
  - react
---

# React App 생성

* toc
{:toc .large-only}

## create-react-app

> create-react-app은 초기앱 구성을 위한 스크립트들과  사전 설정들을 담고 있어 편리하게 프로젝트 생성이 가능하다.

1. node.js 설치
2. npx 설치
3. create-react-app 시작
    ```cmd
    npx create-react-app my-app
    ```

## 기본 기능

**버튼 생성**

`src` 디렉토리 아래에 `Button.js` 파일 생성

```js
function Button({ text }) {
  return <button>{text}</button>
}
export default Button
```

- 다른 파일에서 가져다 쓸 수 있도록 export

**import**

```js
import Button from "./Button";
```

```js
function App() {
  return (
    <div>
      <h1>Welcome back!</h1>
      <Button text={"Continue"} />
    </div>
  );
}
```

- text 입력하여 생성

## Prop Types

**Prop Types 설치**

```cmd
npm i prop-types
```

**impot**

```js
import PropTyes from "prop-types"
```

**적용**

```js
Button.propTypes = {
  text: PropTyes.string.isRequired,
}
```

- text는 String 타입의 필수 Element

## css

**Button.module.css**

```js
.btn {
  color: white;
  background-color: cadetblue;
}
```

- css 모듈 형태로 정의함으로써 모든 요소에 적용되는 것을 방지할 수 있

**import**

```js
import styles from "./Button.module.css";
```
```js
function Button({ text }) {
  return <button className={styles.btn}>{text}</button>;
}
```

- button이 사용되는 파일에서 import 해서 사용

**App.module.css**

```js
.title {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-size: 18px;
}
```

**App.js**

```js
import styles from "./App.module.css";
```

```js
<h1 className={styles.title}>Welcome back!</h1>
```




<span style="font-size:70%">[참조] 노마드 코더 ReactJS로 영화 웹 서비스 만들기</span>

끝!