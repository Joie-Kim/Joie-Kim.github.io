---
title: "[Express] body-parser"
layout: post
date: 2020-09-20 17:30
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
Express의 <u>body-parser 미들웨어 모듈</u>에 대해 알아보자.<br>
(공식 문서와 구글링을 하며 이해한 내용을 정리했다.)
</p>
---

## parser?

parsing(파싱)은 컴퓨터 언어 분석에 사용되며, 컴파일러 및 인터프리터 작성을 용이하게 하기 위해 입력 코드를 구성 요소 부분으로 **구문 분석**하는 것을 뜻하고, 그 역할을 하는 모듈 혹은 메소드를 parser(파서)라 한다.

또한, parser는 어떤 문서(HTML, XML 등..)를 읽어 활용할 수 있도록 <b>내부 표현 방식으로 변환</b>하는 것을 뜻하기도 한다. <br>
예를 들어, XML DOM은 XML 문서에 접근하고 조작할 수 있는 다양한 메소드를 포함하고 있는데, 이 메소드를 사용하기 위해서는 XML 문서를 XML DOM 객체로 변환해야 한다. XML parser가 XML 문서의 평문(plain text)데이터를 읽어 들여, 그것을 XML DOM 객체로 변환해 준다.

---

## body-parser?

클라이언트에서 post 또는 put 메소드로 body를 포함해 요청할 수 있다.<br>
[http 요청 메서드 관련 문서](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)를 확인 해보면, post와 put은 요청에 body(본문)가 존재한다고 되어 있다.

요청 받은 body를 서버에서 그대로 사용할 수 없기 때문에 서버에서 해석 가능한 형태로 변환해야 한다. 즉, <b>parsing</b>을 해야 한다.

http 모듈로만 body를 parsing 하려면 다음과 같이 이벤트를 등록 하고, 그 다음에 인코딩을 해줘야 한다.

```jsx
req.on('data', function(chunk) {
	body += chunk;
});
```

body-parser를 사용하면 자동으로 req에 body 속성이 추가되고 저장되고, 인코딩도 UTF-8을 기본 값으로 해준다.<br>
따라서 이벤트를 등록할 필요가 없어진다. (좀 더 편해진다.)

---

## how to use?

- **커멘드 라인에서 아래 명령어로 설치**

    ```
    $ npm install body-parser
    ```

- **모듈 사용**

    ```jsx
    var express = require('express');
    var bodyParser = require('body-parser');

    var app = express();

    // parse application/x-www-form-urlencoded
    app.use(bodyParser.urlencoded({ extended: false }));

    // parse application/json
    app.use(bodyParser.json());

    app.use(function (req, res) {
      res.setHeader('Content-Type', 'text/plain')
      res.write('you posted:\n')
      res.end(JSON.stringify(req.body, null, 2))
    });
    ```

    클라이언트에서 { name: 'joie', age: 26 }과 같은 JSON 형태의 body를 서버에 보내면 req.body 또는 req.body.name, req.body.age와 같이 해당 데이터를 바로 사용할 수 있다.

### urlencoded([option])?
```
extended option이 특별(?)해서 정리했다.
다른 함수는 간단하니까 생략한다.
```

#### 🤯 공식 문서를 해석 해보자.

Returns middleware that only parses `urlencoded` bodies and only looks at requests where the `Content-Type` header matches the `type` option. This parser accepts only UTF-8 encoding of the body and supports automatic inflation of `gzip` and `deflate` encodings.

> urlencoded body와 Content-Type 헤더와 매치되는 요청의 형태만을 parsing 하는 미들웨어이다.
이 parser는 **UTF-8 인코딩**만을 허용하고, gzip과 deflate 인코딩으로 자동 인플레이션을 지원한다. (gzip, deflate로 자동으로 확장된다는 뜻 같음..)

A new `body` object containing the parsed data is populated on the `request` object after the middleware (i.e. `req.body`). This object will contain key-value pairs, where the value can be a string or array (when `extended` is `false`), or any type (when `extended` is `true`).

> **parsing된 데이터를 포함하는 새로운 body 객체는** 이 미들웨어가 작동한 후에 요청 객체에 채워진다. (즉, **req.body에 채워진다.**)
이 객체는 key-value 쌍을 포함하는데, value는 문자열이나 배열이 될 수도 있고(extended: false), 어떤 유형이든 될 수도 있다(extended: true).

The extended option allows to choose between parsing the URL-encoded data with the querystring library (when false) or the qs library (when true).

> **extended 옵션**은 URL-encoded 데이터를 parsing 할 때, **querystring** 라이브러리를 사용할지(**false**), 또는 **qs** 라이브러리를 사용할지(**true**) 선택할 수 있게 한다.

Defaults to true, but using the default has been deprecated. Please research into the difference between qs and querystring and choose the appropriate setting.

> **기본 값은 true**이지만, 그 기본 값을 사용하는 것보다는 qs와 querystring의 차이점을 알아보고 적절한 설정을 선택해라.
<br> 👉 qs와 querystring의 차이점은 [여기](https://stackoverflow.com/questions/29960764/what-does-extended-mean-in-express-4-0/45690436#45690436)에 설명이 잘 되어 있다.


#### 🥴 위 내용을 요약하자면..

- urlencoded([option])는 parsing 한 데이터를 req.body에 넣어 주고, 인코딩을 해주는 미들웨어 함수이다.
- extended option을 설정할 수 있는데,
    - true : qs 라이브러리 (기본 값)
    - false : querystring 라이브러리

---

### 📚 참고

[http://expressjs.com/en/resources/middleware/body-parser.html](http://expressjs.com/en/resources/middleware/body-parser.html)
[https://velog.io/@yejinh/express-미들웨어-bodyParser-모듈](https://velog.io/@yejinh/express-%EB%AF%B8%EB%93%A4%EC%9B%A8%EC%96%B4-bodyParser-%EB%AA%A8%EB%93%88)
[https://en.wikipedia.org/wiki/Parsing](https://en.wikipedia.org/wiki/Parsing)
[http://tcpschool.com/xml/xml_dom_xmlParser](http://tcpschool.com/xml/xml_dom_xmlParser)
[https://developer.mozilla.org/ko/docs/Web/HTTP/Methods](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
[https://sjh836.tistory.com/154](https://sjh836.tistory.com/154)