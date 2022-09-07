---
layout: post
title: NestJS E2E Testing
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# NestJS E2E Testing

- toc
{:toc .large-only}

## E2E Test

- E2E Test는 Unit Test로 애매하거나 전체 테스트를 할 경우 하는 테스팅
- `app.e2e-spec.ts` 파일을 기반으로 테스팅

```ts
it.todo('PATCH');
```

- `todo` 메서드로 추후에 테스팅할 것을 미리 정의만 해놓을 수 있음

**기본 E2E Testing 명령어**

```cmd
npm run test:e2e
```

## 기본 E2E Test

테스트시에 각각 테스트 할 때마다 새로운 어플리케이션이 실행되기 때문에 `beforeEach`에서 `beforeAll` 메서드로 바꿔준다.

```ts
it('/ (GET)', () => {
  return request(app.getHttpServer())
    .get('/')
    .expect(200)
    .expect('Welcome to my Movie API');
});
```

- 루트 경로에 Get 요청
- 요청한 페이지리를 제공했다는 의미의 `200` 상태 코드 확인
- Return 요청한 텍스트 확인

## Get Test

**getAll**

```ts
it('GET', () => {
  return request(app.getHttpServer())
  .get('/movies')
  .expect(200)
  .expect([]);
});
```

- `/movies` 경로에 Get 요청
- 요청한 페이지리를 제공했다는 의미의 `200` 상태 코드 확인
- 배열 형태 반환 확인

**getOne**

```ts
it('GET 200', () => {
  return request(app.getHttpServer()).get('/movies/1').expect(200);
});
```

- `movies/id` 경로에 Get 요청
- `200` 상태코드를 예상했지만 `404` 상태 코드 반환
- 그 이유는 테스팅 서버에서 `getOne` 메서드에서의 movieId를 `String` 형으로 반환하기 때문
- `String` 형을 `number` 형으로 반환하지 않는 이유는 파이프를 올리지 않고 transform이 작동하지 않았기 때문

```ts
const movie = this.movies.find((movie) => movie.id === id);
```

따라서 다음과 같이 실제 애플리케이션처럼 작동하도록 설정하면 잘 작동

```ts
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,
    forbidNonWhitelisted: true,
    transform: true,
  }),
);
```

## Post Test

```ts
it('POST 201', () => {
  return request(app.getHttpServer())
    .post('/movies')
    .send({
      title: 'Test',
      year: 2000,
      genres: ['test'],
    })
    .expect(201);
});
```

- `/movies` 경로에 Post 요청
- 리소스가 잘 생성됐다는 의미의 `201` 상태 코드 확인

```ts
it('POST 400', () => {
  return request(app.getHttpServer())
    .post('/movies')
    .send({
      title: 'Test',
      year: 2000,
      genres: ['test'],
      other: 'other thing',
    })
    .expect(400);
});
```

- 잘못된 형태의 데이터로 Post 요청
- 잘못된 요청이라는 의미의 `400` 상태 코드 확인

## Patch Test

```ts
it('PATCH 200', () => {
  return request(app.getHttpServer())
    .patch('/movies/1')
    .send({ title: 'Updated Test' })
    .expect(200);
});
```

- `movies/id` 경로에 Patch 요청
- 요청을 잘 처리했다는 의미의 `200` 상태 코드 확인
- Patch 테스트 전에 Delete 테스트를 하면 Patch 시 테스트할 Movie가 없기 때문에 Delete Test 위치를 가장 뒤에 실행하도록 함 


## Delete Test

```ts
it('DELETE 404', () => {
  return request(app.getHttpServer())
  .delete('/movies')
  .expect(404);
});
```

- `/movies` 경로에 Delete 요청
- 리소스가 존재하지 않는다는 의미의 `404` 상태 코드 확인

```ts
it('DELETE 404', () => {
  return request(app.getHttpServer())
  .delete('/movies/1')
  .expect(404);
});
```

- `/movies/id` 경로에 Delete 요청
- 리소스가 존재하지 않는다는 의미의 `404` 상태 코드 확인





<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
