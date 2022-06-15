---
layout: post
title: CSS 실습 2
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
categories:
  - web
  - web_basic
---

# CSS 실습 2

* toc
{:toc .large-only}

## 영화 리뷰 구현하기

### body - wrap 카드 구현

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<div class="wrap">
    <div class="row row-cols-1 row-cols-md-4 g-4">
        <div class="col">
        <div class="card">
            <img src="https://movie-phinf.pstatic.net/20210728_221/1627440327667GyoYj_JPEG/movie_image.jpg
" class="card-img-top" alt="...">
            <div class="card-body">
            <h5 class="card-title">제목</h5>
            <p class="card-text">내용</p>
            <p>⭐⭐⭐</p>>
            <p class="mycomments">코멘트</p>>
            </div>
        </div>
    </div>
    ...
</div>
```

- 코드스니펫에서 카드 코드 가져와서 사용
- `row-cols-1 row-cols-md-4` 3에서 4로 변경해서 카드 4개로 맞춰준다
- 이미지 삽입
- 레이팅 용 별 삽입
- 참조 : [이모티콘 모음](https://kr.piliapp.com/facebook-symbols/)

### head-style 폰트 설정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.mycomments{
    color: gray;
}

```

- 코멘트 색상 변경

### head-style 카드 레이아웃 조정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.wrap{
    width: 1200px;
    margin: 20px auto 0px auto;
}
```

### 최종 결과

![그림1](/assets/img/web/css_card.png)



끝!