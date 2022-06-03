---
layout: post
title: jQuery
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: false
categories:
  - til
  - web
---

# jQuery 기초

* toc
{:toc .large-only}

## jQuery란?

> HTML의 요소들을 조작하는 Javascript를 미리 작성해 둔 라이브러리. 따라서 `import` 필요

__Javascript VS jQuery__

숨기기 기능

Javascript
```js
document.getElementById("element").style.display = "none";
```

jQuery
```js
$('#element').hide();
```

jQuery import
```js
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
</head>
```
이미 추가 되어 있음

## jQery 예시

### input 박스 값 가져오기

```js
$('#url').val();
```
### input 박스에 값 입력하기

```js
$('#url').val('입력할 내용');

```

### div 숨기기

```js
$('#post-box').hide();
```

### div 보이기

```js
$('#post-box').show();
```

### 태그 내 html 입력

```js
let temp_html = `<button>나는 버튼</button>`;
$('#cards-box').append(temp_html);
```

만약 `temp_html`에 카드 div를 넣어주면 카드 추가가 가능해진다.

## post 보이기/숨기기

__jQuery로 함수 구현__

```js
<script>
    function open_box(){
        $('#post-box').show();
    }
    function close_box(){
        $('#post-box').hide();
    }
</script>
```

__열기 버튼__

```js
<div class="mytitle">
    <h1>내 생애 최고의 영화들</h1>
    <button onclick="open_box()">영화 기록하기</button>
</div>
```

__닫기 버튼__

```js
<div class="button-wrap">
    <button type="button" class="btn btn-dark">기록하기</button>
    <button onclick="close_box()" type="button" class="btn btn-outline-dark">닫기</button>
</div>
```


끝!