---
layout: post
title: NestJS DI
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# NestJS 모듈

- toc
{:toc .large-only}

## NestJS 모듈 생성

**생성 명령어**

```cmd
nest g mo movies
```

**생성시 업데이트**

```ts
@Module({
  imports: [MoviesModule],
  controllers: [MoviesController],
  providers: [MoviesService],
})
```

자동으로 `app.module.ts`에 등록되어 업데이트

`AppModule`에 `AppController`와 `AppService`만 등록하고 모듈별로 분리하는 것이 가장 좋은 형태이기 때문에 다음과 같이 수정한다

**AppModule**

```ts
@Module({
  imports: [MoviesModule],
  controllers: [],
  providers: [],
})
```

**MoviesModule**

```ts
@Module({
  controllers: [MoviesController],
  providers: [MoviesService],
})
```

- Module에 import할 컨트롤러와 서비스를 등록하면 NestJS가 알아서 import
- 이를 <span style='background-color: #f5f0ff'>DI(Dependency Injection)</span>라고 함

## NestJS App controllers 생성

**AppController**

```cmd
nest g co app
```

**app.controller.ts**

```ts
@Controller('')
export class AppController {
  @Get()
  home() {
    return 'Welcome to my Movie API';
  }
}
```

## express 사용

```ts
@Get()
getAll(@Req() req, @Res() res): Movie[] {
  res.json()
  return this.moviesService.getAll();
}
```

- `@Req`, `@Res` 데코레이션을 통해 express로 직접 접근해 `req`, `res` 객체 사용이 가능
- 하지만 추천하는 방식이 아니기 때문에 최대한 사용을 지양



<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
