---
layout: post
title: NestJS Test
description: >
  노마드코더 NestJS로 API 만들기 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - nestjs
---

# NestJS Test

- toc
{:toc .large-only}

## Testing 기본

*package.json*

```json
"scripts": {
  ...
  "test": "jest",
  "test:watch": "jest --watch",
  "test:cov": "jest --coverage",
  "test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --runInBand",
  "test:e2e": "jest --config ./test/jest-e2e.json"
}
```

- `package.json` 파일을 보면 test와 관련된 5가지 항목들 존재
- `jest`는 자바스크립트를 테스팅하는 npm 패키지
- NestJS에서 자동 설정

`spec.ts` 파일들이 테스트를 포함하는 파일이며 `jest`가 해당 파일들을 찾을 수 있도록 설정이 되어 있다.
예를들어 movie controller를 테스트하고 싶다면 `movies.controllers.spec.ts` 파일이 필요하다.

## Test 명령어 확인

**Test Coverage 확인**

```cmd
npm run test:cov
```

- 모든 `spec.ts` 파일들을 찾아 테스트하고 몇 줄이 테스트 되었는지 알려줌

**Test watch mode**

```cmd
npm run test:watch
```

- 모든 `spec.ts` 파일들을 찾아 테스트하고 어떤 일들이 일어나는지 관찰

**디버그**

```cmd
npm run test:debug
```

- 디버깅 모드로 들어감

**E2E 테스팅**

```cmd
npm run test:e2e
```

- unit Testing과 반대로 모든 시스템을 테스팅



<span style="font-size:70%">[참조] 노마드코더 NestJS로 API 만들기

끝!
