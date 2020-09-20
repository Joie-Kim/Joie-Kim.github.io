---
title: "[Express] express.static"
layout: post
date: 2020-09-19 20:00
tag:
- Node.js
- Server
hidden : false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---
<p>
<b>Express 모듈 시리즈 🐤</b><br>
express.static 미들웨어 함수를 언제 어떻게 사용해야 하는지 알아보자.<br>
(공식 문서 내용을 보며 이해한 내용을 기록 했다.)
</p>
---

## express.static?

Express 모듈의 기본 제공 미들웨어 함수이며,<br>
이미지, CSS 파일 및 JavaScript 파일과 같은 정적 자산(asste)을 제공하기 위해 사용한다.

<u><b>미들웨어 함수란?</b></u>

 - 요청에 대한 응답을 완수하기 전까지 중간 처리를 할 수 있도록 하기 위한 함수
- express.static 함수처럼 기본으로 제공되는 것도 있고, 직접 함수를 정의해서 사용할 수도 있다. ~~(방법은 나중에 정리해보는 걸로..)~~
- 미들웨어 함수의 생명 주기 : request - response<br>
    > 요청이 발생했을 때, (미들웨어 함수가 있다면)함수 실행<br>→ 응답 후 미들웨어 함수 죽음
- 미들웨어 함수의 우선 순위 : 먼저 로드 되는 함수가 먼저 실행 (따라서 코드 순서 중요)

---

## how to use?
### (1) 기본적인 사용

정적 자산이 포함된 디렉토리의 이름을 전달하면 파일을 직접 제공할 수 있다.<br>
예를 들어, 다음과 같은 코드로 public 이라는 이름의 디렉토리에 포함된 이미지, CSS 파일 및 JavaScript 파일 등을 직접 로드할 수 있다.<br>
단, Express는 지정한 디렉토리에 대해 상대적으로 파일을 검색하기 때문에 <u><b>디렉토리의 이름은 URL의 일부가 아니다.</b></u>

```jsx
app.use(express.static('public'));
```

```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```

### (2) 디렉토리 여러 개 지정하기

여러 개의 디렉토리를 지정하려면 다음과 같이 express.static 함수를 여러 번 호출하면 된다.<br>
Express는 <u><b>디렉토리를 지정한 순서대로 파일을 검색</b></u>한다. (public → files)

```jsx
app.use(express.static('public'));
app.use(express.tattic('files'));
```

### (3) 가상 경로 지정하기

express.static 함수를 통해 제공되는 파일에 대한 가상 경로 접두부를 사용하려면 다음과 같이 하면 된다.

```jsx
app.use('/static', express.static('public'));
```

```
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```

---

### tip!
express.static 함수에 제공되는 경로는 node 프로세스가 실행되는 디렉토리에 대해 상대적이다.<br>
따라서 Express 앱을 다른 디렉토리에서 실행하는 경우에는 다음과 같이 원하는 디렉토리의 절대 경로를 사용하는 것이 더 좋다.

```jsx
app.use('/static', express.static(__dirname + '/public'));
```

---

### 📚 참고

[https://expressjs.com/ko/starter/static-files.html](https://expressjs.com/ko/starter/static-files.html)
[https://jinbroing.tistory.com/126](https://jinbroing.tistory.com/126)