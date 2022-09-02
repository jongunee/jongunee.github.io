---
layout: post
title: 데이터 유효성 검증
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# 데이터 유효성 검증

- toc
{:toc .large-only}

## Class validator 설치

```cmd
npm i class-validator class-transformer
```

## DTO 생성 및 적용

**create-movie.dto.ts**

```ts
export class CreateMovieDto {
  @IsString()
  readonly title: string;

  @IsNumber()
  readonly year: number;

  @IsString({ each: true })
  readonly genres: string[];
}
```

- `@IsString`, `@IsNumber` 데코레이터를 사용해서 유효성 검사
- `each` 옵션은 하나 하나 검사한다는 의미

**Service - Post**

```ts
create(movieData: CreateMovieDto) {
  this.movies.push({
    id: this.movies.length + 1,
    ...movieData,
  });
}
```

- 파라미터 데이터 형식을 `CreateMovieDto`로 설정

**Controller - Post**

```ts
@Post()
create(@Body() movieData: CreateMovieDto) {
  return this.moviesService.create(movieData);
}
```

- Post할 Body 데이터 형식을 `CreateMovieDto`로 설정

**main.ts**

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      forbidNonWhitelisted: true,
    }),
  );
  await app.listen(3000);
}
bootstrap();
```

- `ValidationPipe` 설정
- `whitelist`를 true로 설정하게 되면 decorator가 없는 Property를 모두 거름
- `forbidNonWhitelisted`를 true로 설정하게 되면 잘못된 Property를 메시지로 알려줌
- 서비스와 컨트롤러의 id 형식을 모두 number로 설정하고 `transform`을 true 설정해줘서 movieId 형식을 자유롭게 변환


<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
