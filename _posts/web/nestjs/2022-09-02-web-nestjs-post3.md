---
layout: post
title: NestJS Services
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# NestJS Services

- toc
{:toc .large-only}

## Services 생성

```cmd
nest g s movies
```

- 자동으로 `movies` 폴더 하위에 `movies.service.spec.ts`와 `movies.service.ts` 파일 생성
- `app.module.ts` 모듈에 서비스가 자동으로 등록

```ts
@Module({
  imports: [],
  controllers: [MoviesController],
  providers: [MoviesService],
})
```

## Services 정의

**변수 정의**

```ts
@Injectable()
export class MoviesService {
  private movies: Movie[] = [];
  ...
}
```

**Get**

```ts
getAll(): Movie[] {
  return this.movies;
}
```

```ts
getOne(id: string): Movie {
  const movie = this.movies.find((movie) => movie.id === +id);
  if (!movie) {
    throw new NotFoundException(`Movie with ID ${id} not found.`);
  }
  return movie;
}
```

- movid id를 찾지 못하면 예외 처리
- `+id`로 문자열을 숫자 형태로 변환

**Delete**

```ts
deleteOne(id: string) {
  this.getOne(id);
  this.movies = this.movies.filter((movie) => movie.id !== +id);
}
```

- 삭제하기 전에 movie id를 먼저 검색해서 예외 처리

**Post**

```ts
create(movieData) {
  this.movies.push({
    id: this.movies.length + 1,
    ...movieData,
  });
}
```

- id 자동 생성

**Patch**

```ts
update(id: string, updateData) {
  const movie = this.getOne(id);
  this.deleteOne(id);
  this.movies.push({ ...movie, ...updateData });
}
```

- movie id를 먼저 검색, 예외 처리
- 해당 id 정보를 삭제 후 새로운 정보로 생성시켜 업데이트 

## Controllers 정의

**Service Constructor 생성**`

```ts
@Controller('movies')
export class MoviesController {
  constructor(private readonly moviesService: MoviesService) {}
```

- NestJS에서는 서비스를 생성자로 생성해서 정의된 함수들을 불러옴

**Get**

```ts
@Get()
getAll(): Movie[] {
  return this.moviesService.getAll();
}
```

```ts
@Get('/:id')
getOne(@Param('id') movieId: string): Movie {
  return this.moviesService.getOne(movieId);
}
```

**Post**

```ts
@Post()
create(@Body() movieData) {
  return this.moviesService.create(movieData);
}
```

**Delete**

```ts
@Delete('/:id')
remove(@Param('id') movieId: string) {
  return this.moviesService.deleteOne(movieId);
}
```

**Patch**

```ts
@Patch('/:id')
patch(@Param('id') movieId: string, @Body() updateData) {
  return this.moviesService.update(movieId, updateData);
}
```




<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
