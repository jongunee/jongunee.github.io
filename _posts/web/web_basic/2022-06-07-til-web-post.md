---
layout: post
title: 프로젝트 - 화성땅 공동구매
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
categories:
  - web
  - web_basic
---

# 프로젝트 기본 세팅 - 화성땅 공동구매

* toc
{:toc .large-only}

## 프로젝트 세팅

### 1. 프로젝트 폴더

- staic 폴더 생성
- templates 폴더 생성
- app.py 파일 생성

### 2. Package 설치

- flask 설치
- pymongo 설치
- dnspython 설치
- certifi 설치 - mongodb 오류날 때
- requests 설치 - 크롤링 할 떄
- bs4 설치 - 크롤링 할 떄

### 3. 뼈대 코드

- app.py 파일 구현
- templates 폴더 안에 index.html 파일 생성 및 구현

### 4. 구현

__POST 부분__  

index.html
```js
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

function save_order() {
    let name = $('#name').val()
    let address = $('#address').val()
    let size = $('#size').val()

    $.ajax({
        type: 'POST',
        url: '/mars',
        data: { name_give: name, address_give: address, size_give: size },
        success: function (response) {
            alert(response['msg'])
            window.location.reload()
        }
    });
}
```
- 주문 버튼을 누르면 `save_order()` 함수 실행
- 이름, 주소, 평수 데이터를 jquery로 가져온다
- 데이터를 `name_give`, `address_give`, `size_give`로 전송
- 아래 '주문 완료' 메시지를 `response`로 받아 알람을 준다
- 브라우저 새로고침: `window.location.reload()`

app.py
```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

@app.route("/mars", methods=["POST"])
def web_mars_post():
    name_receive = request.form['name_give']
    address_receive = request.form['address_give']
    size_receive = request.form['size_give']
    doc = {
        'name': name_receive,
        'address': address_receive,
        'size': size_receive
    }
    db.mars.insert_one(doc)
    return jsonify({'msg': '주문 완료!'})
```
- `requests.form[]`으로 API에서 약속한대로 받아서 db에 저장
- 수행 뒤에 '주문 완료' 메시지 전송

---

__GET 부분__

app.py
```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

@app.route("/mars", methods=["GET"])
def web_mars_get():
    order_list = list(db.mars.find({}, {'_id': False}))
    return jsonify({'orders': order_list})
```
- DB에서 주문 내역을 모두 가져와서 'orders'에 담아 전송

index.html
```js
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

function show_order() {
    $.ajax({
        type: 'GET',
        url: '/mars',
        data: {},
        success: function (response) {
            let rows = response['orders']
            for(let i = 0; i < rows.length; i++){
                let name = rows[i]['name']
                let address = rows[i]['address']
                let size = rows[i]['size']

                let temp_html = `<tr>
                                    <td>${name}</td>
                                    <td>${address}</td>
                                    <td>${size}</td>
                                </tr>`
                $('#order-box').append(temp_html)
            }
        }
    });
}

```
- `orders`에 담겨져있는 name, address, size를 꺼냄
- `temp_html`에 \<tr> 구현
- 'order-box'라는 ID를 가진 \<tbody>에 jquery로 temp_html 추가

끝!