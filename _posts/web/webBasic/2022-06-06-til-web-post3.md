---
layout: post
title: Flask
description: >
  스파르타 코딩 클럽 웹개발 종합반 수강 중
sitemap: true
hide_last_modified: true
categories:
  - web
  - webBasic
---

# Flask

* toc
{:toc .large-only}

## Flask란?

> 서버 구동을 위한 프레임워크로 서버 구동을 위한 코드들의 모음이라고 생각하면 된다. 

## Flask 시작 

__시작 코드__

```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
   return 'This is Home!'

@app.route('/mypage')
def mypage():
   return 'This is My page!'

if __name__ == '__main__':
   app.run('0.0.0.0',port=5000,debug=True)
```

- `localhost:5000`으로 접속 가능

__Flask 폴더 구조__

- __static 폴더:__ 주로 이미지, css 파일 포함
- __templates 폴더:__ 주로 html 파일 포함
- __app.py 파일:__ 실행 파일 

__html 파일 불러오기__

```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def home():
   return render_template('index.html')

if __name__ == '__main__':
   app.run('0.0.0.0',port=5000,debug=True)
```

__jQuery 임포트__

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
```
\<head>안에 위치

__get 요청 확인 Ajax__
```html
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<script>
    function hey() {
        $.ajax({
            type: "GET",
            url: "/test?title_give=봄날은간다",
            data: {},
            success: function (response) {
                console.log(response)
            }
        })
    }
</script>
```

__get 요청 API__
```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

from flask import Flask, render_template, request, jsonify

@app.route('/test', methods=['GET'])
def test_get():
   title_receive = request.args.get('title_give')
   print(title_receive)
   return jsonify({'result':'success', 'msg': '이 요청은 GET!'})
```
- `request`, `jsonfy` 추가로 넣어주어야 한다.

__post 요청 확인 Ajax__

```html
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

<script>
    function hey() {
        $.ajax({
            type: "POST",
            url: "/test",
            data: {title_give: '봄날은간다'},
            success: function (response) {
                console.log(response)
            }
        })
    }
</script>
```

__post 요청 API__
```py
스파르타 코딩 클럽 웹개발 종합반 강의 자료 참조

@app.route('/test', methods=['POST'])
def test_post():
   title_receive = request.form['title_give']
   print(title_receive)
   return jsonify({'result':'success', 'msg': '요청을 잘 받았어요'})
```

끝!