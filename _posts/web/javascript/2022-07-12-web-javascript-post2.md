---
layout: post
title: 자바스크립트 클래스
description: >
  스파르타코딩클럽 JavaScript 문법 뽀개기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - javascript
---

# 클래스

* toc
{:toc .large-only}

## 클래스

```js
class Notebook {
  constructor(name, price, company) {
    this.name = name
    this.price = price
    this.company = company
  }
}
```

1. class 키워드와 클래스명
2. 생성자
3. this와 속성

__객체 생성__

```js
const 변수명 = new 클래스명(생성자에서 정의한 매개변수)
const notebook1 = new Notebook('MacBook', 2000000, 'Apple')
```

__메서드__

```js
class Product{
    constructor(name, price){
        this.name = name
        this.price = price
    }

    printInfo(){
        console.log(`name: ${this.name}, price ${this.price}`)
    }
}
```

__객체 리터럴__

```js
const computer = {
    name: 'Macbook',
    price: 2000000,
    printInfo: function(){
        console.log(`name: ${this.name}, price: ${this.price}`)
    }
}
```
- 객체 리터럴을 활용해서 빠르게 객체를 만들 수 있음

<span style="font-size:70%">[참조] 스파르타코딩클럽 JavaScript 문법 뽀개기

끝!