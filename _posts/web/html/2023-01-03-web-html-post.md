---
layout: post
title: Kokoa talk 클론 코딩 - Friends 페이지
description: >
  노마드코더 코코아톡 클론코딩 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - html
---

# Kokoa talk 클론 코딩 - Friends 페이지

- toc
{:toc .large-only}

## 시작전 최적화

css 파일을 하나에서 모두 설정할 수도 있지만 가독성을 위해 아래 그림과 같이 분류하는 것이 좋다.

![그림1](/assets/img/html/folders.png)

**다음 페이지와의 연동**

```html
<form action="friends.html" method="get" id="login-form">
  ...
</form>
```

- **friends.html** 파일 생성
- Log In 버튼을 클릭하면 Get요청으로 **frineds.html** 페이지로 이동

**friends.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <link rel="stylesheet" href="css/style.css" />
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Friends - Kokoa Clone</title>
  </head>
  <body>
    <div class="status-bar">
      <div class="status-bar__column">
        <span>No Service</span>
        <i class="fa-solid fa-wifi"></i>
      </div>
      <div class="status-bar__column">
        <span>18:43</span>
      </div>
      <div class="status-bar__column">
        <span>51%</span>
        <i class="fa-solid fa-battery-half fa-lg"></i>
        <i class="fa-sharp fa-solid fa-bolt"></i>
      </div>
    </div>

    <script
      src="https://kit.fontawesome.com/340f2fb345.js"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

**index.html** 페이지 코드를 복사해 상태바 부분만 남기고 지워준다.

## nav 생성

**friends.html**

```html
<nav class="nav">
  <ul class="nav__list">
    <li class="nav__btn">
      <a class="nav__link" href="friends.html"
        ><i class="fa-solid fa-user fa-2x"></i
      ></a>
    </li>
    <li class="nav__btn">
      <a class="nav__link" href="#"
        ><i class="fa-regular fa-comment fa-2x"></i
      ></a>
    </li>
    <li class="nav__btn">
      <a class="nav__link" href="#"
        ><i class="fa-solid fa-magnifying-glass fa-2x"></i
      ></a>
    </li>
    <li class="nav__btn">
      <a class="nav__link" href="#"
        ><i class="fa-solid fa-ellipsis fa-2x"></i
      ></a>
    </li>
  </ul>
</nav>
```

- 단축어로 쉽게 생성 가능
  ```html
  nav>ul>li*4>a
  ```
- 아이콘 지정 및 클래스 이름 지정

**css 설정**

```css
.nav {
  position: fixed;
  bottom: 0;
  width: 100%;
  background-color: #f9f9fa;
  padding: 20px 50px;
  box-sizing: border-box;
  border-top: 1px solid rgba(121, 121, 121, 0.3);
}
.nav__list {
  display: flex;
  justify-content: space-between;
}
```

- 위치 및 크기 지정
- **position: fixed:** 위치를 고정시켜 항상 하단에 위치하도록 함

**결과**

![그림2](/assets/img/html/nav.png)

## 채팅 알림 표시

**html**

```html
<li class="nav__btn">
  <a class="nav__link" href="#">
    <span class="nav__notification">1</span>
    <i class="fa-regular fa-comment fa-2x"></i>
  </a>
</li>
```

- 채팅 알림 1을 표시하기 위한 html

**css**

```css
.nav__link {
  position: relative;
  color: #2e363e;
}
```

```css
.nav__notification {
  background-color: tomato;
  width: 30px;
  height: 30px;
  border-radius: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: 600;
  position: absolute;
  left: 15px;
  bottom: 15px;
}
```

- notification을 absolute로 지정하면 조상에 해당되는 요소에 따라 위치를 지정
- 별도의 지정이 없으면 **body**를 기준으로 하기 때문에 위에서는 **nav__link**에서 relative로 지정





<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
