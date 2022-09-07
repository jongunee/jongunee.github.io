---
layout: post
title: NestJS Unit Testing
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# NestJS Unit Testing

- toc
{:toc .large-only}

## spec.ts 기본

**기본 Unit Testing 명령어**

```cmd
npm run test
npm run test:watch
npm run test:cov
```

**describe**

```ts
describe('MoviesService', () => {
  ...
}
```

- 테스트 묘사

**beforeEach**

```ts
beforeEach(async () => {
  ...
});
```

- 테스트를 하기 전에 실행

**it**

```ts
it('should be 4', () => {
  expect(2 + 2).toEqual(4);
});
```

- 테스트하는 메서드가 입력되는 부분

## getAll 메서드 Unit Testing

```ts
describe('getAll', () => {
  it('should return an array', () => {
    const result = service.getAll();

    expect(result).toBeInstanceOf(Array);
  });
});
```

- getAll 메서드 호출시 배열이 return 되는지 테스트

## getOne 메서드 Unit Testing

```ts
it('should return a movie', () => {
  service.create({
    title: 'Test Movie',
    genres: ['test'],
    year: 2000,
  });
  const movie = service.getOne(1);
  expect(movie).toBeDefined();
  expect(movie.id).toEqual(1);
});
```

- Test Movie 생성
- 등록이 잘 되었는지 확인
- 등록된 MovieId 확인

```ts
it('should throw 404 error', () => {
  try {
    service.getOne(999);
  } catch (e) {
    expect(e).toBeInstanceOf(NotFoundException);
    expect(e.message).toEqual('Movie with ID 999 not found.');
  }
});
```

- 404 에러 확인
- 첫 Movie 등록시 id는 1이기 때문에 id 999를 get 요청했을 때 `NotFoundException` 에러가 발생하는지 확인
- 에러 메시지 제대로 출력되는지 확인

## deleteOne 메서드 Unit Testing

```ts
it('deletes a movie', () => {
  service.create({
    title: 'Test Movie',
    genres: ['test'],
    year: 2000,
  });
  const beforeDelete = service.getAll().length;
  service.deleteOne(1);
  const afterDelete = service.getAll().length;
  expect(afterDelete).toBeLessThan(beforeDelete);
});
```

- Test Movie 생성
- 삭제 전 Movie 개수와 삭제 후 Movie 개수 비교

```ts
it('should return a 404', () => {
  try {
    service.deleteOne(999);
  } catch (e) {
    expect(e).toBeInstanceOf(NotFoundException);
  }
});
```

- 404 에러 확인
- 등록되어 있지않는 id 999를 get 요청했을 때 `NotFoundException` 에러가 발생하는지 확인


## create 메서드 Unit Testing

```ts
it('should create a movie', () => {
  const beforeCreate = service.getAll().length;
  service.create({
    title: 'Test Movie',
    genres: ['test'],
    year: 2000,
  });
  const afterCreate = service.getAll().length;
  expect(afterCreate).toBeGreaterThan(beforeCreate);
});
```

- Test Movie 생성
- 생성 전 Movie 개수와 생성 후 Movie 개수 비교

## update 메서드 Unit Testing

```ts
it('should update a movie', () => {
  service.create({
    title: 'Test Movie',
    genres: ['test'],
    year: 2000,
  });
  service.update(1, { title: 'Updated Test' });
  const movie = service.getOne(1);
  expect(movie.title).toEqual('Updated Test');
});
```

- Test Movie 생성
- update 전 Movie title과 update 후 Movie title 비교


<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
