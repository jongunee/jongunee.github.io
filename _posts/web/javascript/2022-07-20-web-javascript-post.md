---
layout: post
title: 타입스크립트 추상클래스, 인터페이스
description: >
  노마드코더 Typescript로 블록체인 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - javascript
---

# 타입스크립트 추상클래스, 인터페이스

* toc
{:toc .large-only}

## 클래스

```ts
class Player{
  constructor(
    private firstName: string,  //JS: this.firstName = firstName;
    private lastName: string,
    private nickName: string
  ){}
} 

const jw = new Player("jongwon", "Park", "종원")
```
타입스크립트에서는 선언만 해주면 자바스크립트처럼 따로 값을 넣어주지 않더라도 자동으로 구현 해줌

## 추상클래스

```ts
abstract class User{
  constructor(
    private firstName: string,  //JS: this.firstName = firstName;
    private lastName: string,
    private nickName: string
  ){}

  abstract getNickName(): void
  getFullName(){
    return `${this.firstName} ${this.lastName}`
  }
} 
class Player extends User{}

const jw = new Player("jongwon", "Park", "종원")
```
추상클래스는 `new` 키워드를 통해 직접 인스턴스화 할 수 없다

__추상메서드__

```ts
abstract getNickName(): void
```
추상클래스는 선언만 되어있기 때문에 인스턴스화 할 때 구현을 해야한다 

__사전 구현__

```ts
type Words = {
  [key: string]: string
}

class Dict{
  private words: Words
  constructor() {
    this.words = {}
  }
  add(word: Word){
    if(this.words[word.term] === undefined){
      this.words[word.term] = word.def;
    }
  }
  def(term: string){
    return this.words[term]
  }
}

class Word{
  constructor(
    public term: string,
    public def: string
  ){}
}

const kimchi = new Word("kimchi", "한국 음식")
const dict = new Dict()

dict.add(kimchi)
dict.def("kimchi")
```

## 인터페이스

__타입 표현__

```ts
type Team = "red" | "blue" | "yellow"
type Health = 1 | 5 | 10

type Player = {
  nickName: string,
  team: Team,
  health: Health
}
```

__인터페이스 표현__

```ts
interface Player {
  nickName: string,
  team: Team,
  health: Health
}
```

```ts
interface User{
  name: string
}
interface User{
  lastName: string
}
interface User{
  health: number
}

const jw: User = {
  name: "jongwon",
  lastName: "park",
  health: 10
}
```
- 타입스크립트는 인터페이스를 나눠서 각각 정의해도 알아서 합쳐준다
- `type`으로는 불가능 - 에러 발생

인터페이스는 어떻게 설계해야하는지 방향성을 알려주는 설계도이기 때문에 `new` 키워드로 인스턴스화 되는 것이 아니라 `extends` 키워드로 상속시켜 사용한다
- 다중 상속 가능

```ts
class Player implements User, Human{
  ...
}
```
- 인터페이스를 타입으로 사용도 가능

```ts
function makeUser(user: User){
  ...
}
makeUser({
  firstName: "jongwon"
  ...
})
```


<span style="font-size:70%">[참조] 노마드코더 Typescript로 블록체인 만들기

끝!
