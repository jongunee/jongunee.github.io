---
layout: post
title: HTML, CSS, JS
description: >
  노마드코더 코코아톡 클론코딩 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - html
---

# HTML, CSS, JS

- toc
{:toc .large-only}

## HTML이란?

> 컴퓨터는 인간의 언어를 이해하지 못한다. HTML은 Markup Language로 컨셉은 브라우저에게 컨텐츠에 대한 설명을 해주는 것

## CSS란?

> Cascading Style Sheet의 약자로 컨셉은 브라우저에게 웹 사이트가 어떻게 보여야하는지에 대해 설명햐주는 것

## Javascript란?

> Javascript란 웹 사이트의 두뇌 역할을 한다. 컨셉은 동적 상호작용성을 부여하여 애니매이션 등 좀 더 동적으로 동작할 수 있도록 해주는 것

## HTML Tag

- HTML을 구성하기 위해서 태그를 사용한다
- 태그 안의 것들이 Contents
- 코드를 제대로 된 위치에 넣고 tag들을 기억하면 웹 브라우저가 tag에 맞춰 변경해서 보여주는 것

**\<h>태그**

```html
<h1>Hello! this is my web browser</h1>
<h2>Hello! this is my web browser</h2>
<h3>Hello! this is my web browser</h3>
```


- 웹 브라우저는 태그 단위로 이해를 하며 h1 태그는 이미 정의 되어있기 때문에 지정된 타이틀 스타일로 바꾸어 보여줌
- h태그는 타이틀 스타일로 숫자가 커질수록 글꼴이 작아짐

**\<ul> 태그**

```html
<ul>
  <li>김치</li>
  <li>고기</li>
  <li>콜라</li>
</ul>
```

- `<ul>` 태그에 컨텐츠들은 목록으로 변경되어 보여줌
- `<li>` 태그는 list item을 의미하는 태그
- `<ul>` 태그 대신에 `<ol>` 태그로 설정하면 ordered list로 번호를 주어 순서를 정해줌

**\<a>태그 및 \<img> 태그**

```html
<a href="http://google.com" target="_blank">Go to google.com</a>
<img src="https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png" />
```

- `<a>` 태그는 anchor로 링크를 의미하는 태그
- `href` attribute는 링크로 연결할 URL을 설정 
- `target`는 기본적으로 `_self`로 설정되어 있어 현재 탭에서 열리지만 `_blank`로 설정해 주면 새 탭에서 열리게 됨 
- attributes는 특정 태그에서만 동작하며 예를들어 위 attributes는 `<a>` 태그에서는 동작하지만 `<h1>` 태그에서는 동작하지 않음
- `<img>` 태그는 self-closing 태그이기 떄문에 `<\img>`와 같은 닫아주는 것이 없고 `<img 이미지/>`로 표현
- `src` attrubute에 이미지 주소를 지정



<span style="font-size:70%">[참조] 노마드코더 코코아톡 클론코딩

끝!
