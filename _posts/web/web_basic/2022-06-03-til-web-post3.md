---
layout: post
title: CSS 실습 3
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
categories:
  - web
  - web_basic
---

# CSS 실습 3

* toc
{:toc .large-only}

## 영화 리뷰 구현하기

### body - mypost 포스팅 구현

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<div class="mypost">
    <div class="form-floating mb-3">
        <input type=url class="form-control" id="floatingInput" placeholder="name@example.com">
        <label for="floatingInput">영화URL</label>
    </div>
    <div class="input-group mb-3">
        <label class="input-group-text" for="inputGroupSelect01">별점</label>
        <select class="form-select" id="inputGroupSelect01">
            <option selected>-- 선택하기 --</option>
            <option value="1">⭐</option>
            <option value="2">⭐⭐</option>
            <option value="3">⭐⭐⭐</option>
            <option value="4">⭐⭐⭐⭐</option>
            <option value="5">⭐⭐⭐⭐⭐</option>
        </select>
    </div>
    <div class="form-floating">
            <textarea class="form-control" placeholder="Leave a comment here" id="floatingTextarea2"
                      style="height: 100px"></textarea>
        <label for="floatingTextarea2">코멘트</label>
    </div>
    <div class="button-wrap">
        <button type="button" class="btn btn-dark">기록하기</button>
        <button type="button" class="btn btn-outline-dark">닫기</button>
    </div>
</div>
```

- 코드스니펫에서 코드 가져와서 사용
- url 입력 부분
- 별점 매기기 부분
- 코멘트 남기기
- 버튼 추가

### head-style 버튼 레이아웃 설정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.button-wrap {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
}

.button-wrap > button {
    margin: 0 5px 0 5px;
}

```

- 버튼 수평으로 정렬
- Margin 주기

### head-style 모바일 설정

```css
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

.wrap {
    max-width: 1200px;
    width: 95%;
    margin: 20px auto 0px auto;
}

.mypost {
    max-width: 500px;
    width: 95%;

    margin: 20px auto 0px auto;

    box-shadow: 0px 0px 3px 0px gray;
    padding: 20px;

    color: gray;
}
```

- max-width로 최대 넓이 지정
- 최대 넓이 이전에는 95%로 지정

### 최종 결과

![그림1](/assets/img/web/css_final.png)


끝!