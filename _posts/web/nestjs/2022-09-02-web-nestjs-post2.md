---
layout: post
title: NestJS Controllers
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# NestJS Controllers

- toc
{:toc .large-only}

## Contollers 생성

```cmd
nest g co movies
```

- 자동으로 `movies` 폴더 하위에 `movies.controler.spec.ts`와 `movies.controller.ts` 파일 생성
- `app.module.ts` 모듈에 컨트롤러가 자동으로 등록

```ts
import { MoviesController } from './movies/movies.controller';

@Module({
  imports: [],
  controllers: [MoviesController],
  providers: [],
})
```

## Controllers 구현

**Get**

```ts
@Get('/:id')
getOne(@Param('id') movieId: string) {
  return `This will return one movie with the id: ${movieId}`;
}
```

- 파라미터를 받아서 리턴

```ts
@Get('search')
search(@Query('year') searchingYear: string) {
  return `we are searching for a movie with a title made after: ${searchingYear}`;
}
```

입력

```
http://localhost:3000/movies/search?year=2000
```

출력

```
we are searching for a movie with a title made after: 2000
```

- `@Query`로 searchingYear 값을 받아 출력해줌 

**Post**

```ts
@Post()
create(@Body() movieData) {
  console.log(movieData);
  return movieData;
}
```

**Delete**

```ts
@Delete('/:id')
remove(@Param('id') movieId: string) {
  return `This will delete a movie with the id: ${movieId}`;
}
```

**Patch**

```ts
@Patch('/:id')
patch(@Param('id') movieId: string, @Body() updateData) {
  return {
    updateMovie: movieId,
    ...updateData,
  };
}
```

- Put은 모든 리소스를 업데이트하지만
- Patch는 일부 리소스를 업데이트

**Patch 결과**

입력

```json
{
	"name": "Tenet",
	"director": "Nolan"
}
```

출력

```json
{
	"updateMovie": "12",
	"name": "Tenet",
	"director": "Nolan"
}
```

- @Body로 json 입력값을 받고 업데이트를 해주어 결과값 리턴


<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
