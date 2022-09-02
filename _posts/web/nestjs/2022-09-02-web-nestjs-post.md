---
layout: post
title: NestJS
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# NestJS

- toc
{:toc .large-only}

## NestJS 특징

- 기업 프로젝트에 적합한 아키텍처와 구조를 가진 프레임워크
- Typescript 기반

## NestJS 시작

**NestJS 설치**

```cmd
npm i -g @nestjs/cli
```

**프로젝트 생성**

```cmd
nest new project-name
```

**어플리케이션 시작**

```cmd
npm run start:dev
```

**웹 확인**

```
localhost:3000
```

## NestJS 구조

**main.ts**

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
- 기본 함수 구조
- AppModule 호출
- 포트 3000번 Listen
- 항상 시작하는 기점
- 하나의 모듈에서 어플리케이션 생성

**app.module.ts**

```ts
@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

- @: 데코레이터로 클래스에 함수 기능 추가 가능
- AppModule은 root 모듈 같은 것

**app.controller.ts**

```ts
@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
```

- AppService 생성자로 이동

**app.service.ts**

```ts
@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

- AppService 생성자에서 "Hello World!" 반환

## Controllers

> NestJS Controller란 기본적으로 url을 가져오고 함수를 실행시키는 역할을 한다

- express의 router 같은 역할

```ts
@Get('/hello')
sayHello(): string {
  return 'Hello everyone';
}
```

- 컨트롤러가 url 요청을 받아서 sayHello() 함수 실행 
- http://localhost:3000/hello 에서 Return 값 확인 가능 

## Services

> NestJS에서 Services의 목적은 Controllers와 비지니스 로직을 구분하기 위함이고 Services에 함수를 정의한다.

**서비스**

```ts
@Injectable()
export class AppService {
  ...
  getHi(): string {
    return 'Hi Nest!';
  }
}
```

**컨트롤러**

```ts
@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}
  ...
  @Get('/hello')
  sayHello(): string {
    return this.appService.getHi();
  }
}
```

- 서비스에서 비즈니스 로직과 같은 함수 정의
- 서비스에 정의된 함수를 컨트롤러에서 호출


<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
