---
title: "[Express] router"
layout: post
date: 2020-10-01 14:30
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
<b>Express 시리즈 🐤</b><br>
Express는 Node.js를 위한 빠르고 간편한 <u>웹 프레임워크</u>다.<br>
Node.js의 핵심 모듈인 http와 Connect 컴포넌트를 기반으로 하며, 이런 컴포넌트를 미들웨어(Middleware)라고 한다.<br>
<br>
Express에서 제공하는 <u>router</u>에 대해 알아보자.
</p>

---

## Routing?

### 정의

라우팅은 URI(또는 경로) 및 특정한 HTTP 요청 메소드(GET, POST 등)인 특정 엔드포인트에 대한 클라이언트 요청에 애플리케이션이 응답하는 방법을 결정하는 것을 말한다.<br>
각 라우트는 하나 이상의 핸들러 함수를 가질 수 있으며, 이러한 함수는 라우트가 일치할 때 실행된다.

```jsx
var express = require('express');
var app = express();

app.METHOD(PATH, HANDLER);
```

- app : express의 인스턴스
- METHOD : HTTP 요청 메소드 (METHOD 대신 'use'를 사용할 수 있음)
- PATH : 서버에서의 경로
- HANDLER : 라우트가 일치할 때 실행되는 함수

### 예시

1. 홈 페이지에서 Hello World!로 응답

    ```jsx
    app.get('/', function(req, res) {
    	res.send('Hello World!');
    });
    ```

2. 애플리케이션의 홈 페이지인 루트 라우트(/)에서 POST 요청에 응답

    ```jsx
    app.post('/', function(req, res) {
    	res.send('Got a POST request');
    });
    ```

3. /user 라우트에 대한 PUT 요청에 응답

    ```jsx
    app.put('/user', function(req, res) {
    	res.send('Got a PUT request at /user');
    });
    ```

4. /user 라우트에 대한 DELETE 요청에 응답

    ```jsx
    app.delete('/user', function(req, res) {
    	res.send('Got a DELETE request at /user');
    });
    ```

### 라우트 메소드

라우트 메소드는 HTTP 메소드 중 하나로부터 파생 되며, express 클래스의 인스턴스에 연결된다.<br>
Express는 HTTP 메소드에 해당하는 다음과 같은 라우팅 메소드를 지원한다.

`get, post, put, head, delete, options, trace, copy, lock, mkcol, move, purge, propfind, proppatch, unlock, report, mkactivity, checkout, merge, m-search, notify, subscribe, unsubscribe, patch, search, connect`

→ 올바르지 않은 JavaScript 변수 이름으로 변환되는 메소드를 라우팅 하려면 대괄호 표기법을 사용하면 된다. 예를 들어 `app['m-search']('/', function ...` 과 같이!

### 응답 메소드

- res.download() : 파일이 다운로드 되도록
- res.end() : 응답 프로세스를 종료
- res.json() : JSON 응답을 전송
- res.jsonp() : JSONP 자원을 통해 JSON 응답을 전송
- res.redirect() : 요청의 경로를 재지정
- res.render() : 보기 템플릿을 렌더링
- res.send() : 다양한 유형의 응답을 전송
- res.sendFile : 파일을 옥텟 스트림의 형태로 전송
- res.sendStatus() : 응답 상태 코드를 설정한 후, 해당 코드를 문자열로 표현한 내용을 응답 본문으로서 전송

### 라우트 핸들러

미들웨어와 비슷하게 작동하는 여러 콜백 함수를 제공하여 요청을 처리할 수 있다.

1. 하나의 콜백 함수는 하나의 라우트를 처리할 수 있다.

    ```jsx
    app.get('/example/a', function(req, res) {
    	res.send('Hello from A!');
    });
    ```

2. 2개 이상의 콜백 함수는 하나의 라우트를 처리할 수 있다. (단, next 오브젝트를 반드시 지정해야 함)

    ```jsx
    app.get('example/b', function(req, res, next) {
    	console.log('the response will be sent by the next function ...');
    	next();
    }, function(req, res) {
    	res.send('Hello from B!');
    });
    ```

    → 2번째 콜백 함수에서 res.send() 실행

3. 하나의 콜백 함수 배열은 하나의 라우트를 처리할 수 있다.

    ```jsx
    var f1 = function(req, res, next) {
    	console.log('f1');
    	next();
    }

    var f2 = function(req, res, next) {
    	console.log('f2');
    	next();
    }

    var f3 = funciton(req, res) {
    	res.send('f3!!');
    }

    app.get('/example/c', [f1, f2, f3]); // callback array
    ```

4. 독립적인 함수와 함수 배열의 조합은 하나의 라우트를 처리할 수 있다.

    ```jsx
    var f1 = function(req, res, next) {
    	console.log('f1');
    	next();
    }

    var f2 = function(req, res, next) {
    	console.log('f2');
    	next();
    }

    app.get('/example/d', [f1, f2], function(req, res, next) {
    	console.log('the response will be sent by the next function...');
    	next();
    }, function(req, res) {
    	res.send('Hello!!');
    });
    ```

---

## 라우터 레벨 미들웨어

라우터 레벨 미들웨어는 express.Router() 인스턴스에 바인드 된다는 점을 제외하면 애플리케이션 레벨 미들웨어와 동일한 방식으로 작동한다. (위에서 본 '라우터 핸들러'와 동일하게)

> **애플리케이션 레벨 미들웨어?**<br>: 
`app = express()`로 인스턴스에 바인드 한 뒤, app.use() 또는 app.METHOD() 함수를 이용

```jsx
var express = require('express');
var router = express.Router();

router.METHOD(PATH, HANDLER);
```

### 똑같으면 왜 사용하지?

`var app = express()`의 경우, 앱 객체를 반환한다. 이것을 메인 앱이라고 생각하면 된다.<br>
`var router = express.Router()`의 경우, 약간 다른 작은 앱(응용 프로그램)을 반환한다. 이렇게 나누는 이유는 서비스의 규모가 커지면서 프로그램의 경로가 복잡 해질 수 있어 코드를 별도의 파일로 분리해 관리를 쉽게 하기 위해서다.

**app.js**

```jsx
var express = require('express');
var dogs = require('./routes/dogs'),
		cats = require('./routes/cats'),
		birds = require('./routes/birds');

var app = express();

app.use('/dogs', dogs);
app.use('/cats', cats);
app.use('birds', birds);

app.listen(3000);
```

**dogs.js**

```jsx
var express = require('express');

var router = express.Router();

router.get('/', function(req, res) {
	res.send('GET handler for /dogs route.');
});

router.post('/', function(req, res) {
	res.send('POST handler for /dogs route.');
});

module.exports = router;
```

/dogs 경로에 대한 코드는 메인 앱을 복잡하게 만들지 않도록 따로 파일로 분리한다.<br>
/cats, /birds도 따로 분리해 비슷하게 구조화한다. 세 개의 작은 앱으로 분리하면 각각의 로직에 대해 개별적으로 작업할 수 있다. (즉, 다른 두 앱에 어떤 영향을 미치지 않는다.)

---

### 📚 참고

[https://expressjs.com/ko/starter/basic-routing.html](https://expressjs.com/ko/starter/basic-routing.html)<br>
[https://expressjs.com/ko/guide/routing.html](https://expressjs.com/ko/guide/routing.html)<br>
[https://expressjs.com/ko/guide/using-middleware.html](https://expressjs.com/ko/guide/using-middleware.html)<br>
[https://stackoverflow.com/questions/28305120/differences-between-express-router-and-app-get](https://stackoverflow.com/questions/28305120/differences-between-express-router-and-app-get)