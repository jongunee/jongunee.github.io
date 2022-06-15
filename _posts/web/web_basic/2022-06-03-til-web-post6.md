---
layout: post
title: jQuery 퀴즈
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
categories:
  - web
  - web_basic
---

# jQuery 퀴즈

* toc
{:toc .large-only}

## 1. 빈칸 체크 함수

> 버튼 눌렀을 떄 입력한 글자 alert 띄우기. 아무것도 없으면 '입력하세요!' alert.

```js
function q1() {
    let txt = $('#input-q1').val();
    if(txt == ''){
        alert('입력하세요!');
    }
    else{
        alert(txt);
    }
}
```

## 2. 이메일 판별 함수

> 버튼 눌렀을 떄 이메일 도메인 띄우기. 만약 입력한 텍스트가 아니면(@가 없다면) '이메일이 아닙니다' alert.

```js
function q2() {
    let email = $('#input-q2').val();
    if(email.includes('@')){
        alert(email.split('@')[1].split('.')[0]);
    }
    else{
        alert('이메일이 아닙니다.');
    }
}
```

## 3-1. HTML 붙이기

> 이름을 입력하고 붙이기 버튼을 누르면 아래 리스트에 누적

```js
function q3() {
    let txt = $('#input-q3').val();
    let temp_html = `<li>${txt}</li>`;
    $('#names-q3').append(temp_html);
}
```

## 3-2. HTML 지우기

> 지우기 버튼을 누르면 리스트 내용 모두 지우기.

```js
function q3_remove() {
    $('#names-q3').empty();
}
```

끝!