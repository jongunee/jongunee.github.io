---
layout: post
title: JSON, Ajax
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - web-basic
---

# JSON, Ajax

* toc
{:toc .large-only}

## JSON이란?

> <span style='background-color: #f5f0ff'>데이터 교환용</span>으로 설계된 경량 텍스트 기반 개방형 표준 포맷. 자바스크립트에서 확장되었으며 데이터는 <span style='background-color: #f5f0ff'>속성-값</span> 쌍으로 표현된다.

__API__ 란 은행의 창구같이 클라이언트 요청에 따라 처리됨 

## GET 방식 데이터 전달

- __?:__ 여기서부터 전달할 데이터가 작성
- __&:__ 전달할 데이터가 더 존재  

ex) google.com/search?q=아이폰&sourceid=chrome&ie=UTF-8  

- q=아이폰 - 검색어
- sourceid=chrome - 브라우저 정보
- ie=UTF-8 - 인코딩 정보

## Ajax란?

> 자바스크립트 라이브러리 중 하나로 브라우저가 가지고 있는 XMLHTTPRequest 객체를 이용해서 전체 페이지를 새로 고침하지 않고도 <span style='background-color: #f5f0ff'>페이지 일부만을 위한 데이터를 로드</span>하는 기법

__예제 코드__

```js
$.ajax({
    type: "GET",
    url: "http://spartacodingclub.shop/sparta_api/seoulair",
    data: {},
    success: function (response) {
        let rows = response['RealtimeCityAir']['row'];
        for(let i = 0; i < rows.length; i++){
            let gu_name = rows[i]['MSRSTE_NM'];
            let gu_mise = rows[i]['IDEX_MVL'];
            console.log(gu_name, gu_mise);
        }
    }
})
```
- 서울시 미세먼지 API를 로드
- 파싱을 통해 구와, 미세먼지 수치만 뽑아와 console log로 출력

__예제 코드__

```js
function q1() {
    $('#names-q1').empty();
    $.ajax({
        type: "GET",
        url: "http://spartacodingclub.shop/sparta_api/seoulair",
        data: {},
        success: function (response) {
            let rows = response['RealtimeCityAir']['row'];
            for(let i = 0; i < rows.length; i++){
                let gu_name = rows[i]['MSRSTE_NM'];
                let gu_mise = rows[i]['IDEX_MVL'];
                let temp_html = ``;

                if(gu_mise > 90){
                    temp_html = `<li class="bad">${gu_name} : ${gu_mise}</li>`
                }
                else{
                    temp_html = `<li class>${gu_name} : ${gu_mise}</li>`
                }

                $('#names-q1').append(temp_html);
            }
        }
    })
}
```

- 조건문을 넣어서 미세먼지 수치가 높으면 빨간색으로 표기

__예제 코드__

```js
$.ajax({
    type: "GET",
    url: "http://spartacodingclub.shop/sparta_api/rtan",
    data: {},
    success: function (response) {
        let url = response['url'];
        let msg = response['msg'];

        $("#img-rtan").attr("src", url);
        $("#text-rtan").text(msg);

    }
})
```

- `attr()`: 속성 변경이 가능
- `text`: 텍스트 변경이 가능

__별 반복하기__

```js
    let star_image = '⭐'.repeat(star);
```
- star 숫자 수만큼 ⭐이 반복해서 출력된다.

끝!