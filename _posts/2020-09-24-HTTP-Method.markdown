---
title: "HTTP Method"
layout: post
date: 2020-09-24 18:00
tag:
- 용어_정리
- HTTP
hidden : false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---
## HTTP 메소드?

클라이언트가 **요청(request)을 한 목적**과 요청의 결과로 기대하는 바를 나타낸다.<br>(그래서 HTTP 요청 메소드라고도 한다.)<br>
즉, 클라이언트는 어떤 요청을 할 것인지 표현하기 위해, 서버는 어떤 요청에 대한 응답인 지를 나타내기 위해 사용하면 된다.

---

## 메소드 속성
### (1) Safe (안전한)

HTTP는 안전한 메소드(Safe Methods)라 불리는 메소드의 집합을 정의한다. 정의된 의미 체계가 본질적으로 '읽기 전용'이면 요청 메소드는 안전한 것으로 간주된다. 즉, <u>클라이언트가 안전한 메소드로 요청을 한다면, 그 결과로 서버에 어떠한 작용(상태 변화와 같은)도 하지 않는다.</u> 그 예로 GET, HEAD 메소드 등이 있다.

안전한 메소드의 목적은 <u>서버에 영향을 줄 수 있는 안전하지 않은 메소드가 사용될 때 사용자에게 알려줄 수 있는</u> HTTP 애플리케이션을 만드는 데에 있다.

### (2) Idempotent (멱등)

```
Idempotent?
: (수학이나 전산학에서) 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질.
```

즉, 멱등(성) 메소드(Idempotent Methods)는 <u>서버에 동일한 요청을 여러 번 전송 하더라도 동일한 응답이 돌아와야 함</u>을 의미한다. 이런 메소드의 예로 PUT, DELETE 등이 있다.

만약 클라이언트가 요청을 보내고 응답이 수신되기 전에 기본 연결이 닫히면, 클라이언트는 새로 연결 후 요청을 재시도 한다. 이때, 멱등(성) 메소드로 요청을 하는 경우라면 동일한 결과를 응답 받을 수 있다.

### (3) Cacheable (캐시 가능한)

캐시 가능한 메소드(Cacheable Methods)로 정의된 요청은 그에 대한 <u>응답이 향후 재사용을 위해 저장될 수 있음</u>을 나타낸다. 이 메소드의 예로 GET, HEAD가 있다.

---

## 종류

![image](/assets/200924/HTTP-Method.png)

- **GET**<br>
목적 리소스의 현재 표시를 전송

- **HEAD**<br>
GET 메소드의 요청과 동일한 응답을 요구하지만, 오직 Status Line과 Header 영역 만을 전송 (즉, 응답 본문은 포함하지 않음)

- **POST**<br>
요청 payload에 대한 리소스 별 처리 수행
> payload : 전송되는 데이터를 의미

- **PUT**<br>
목적 리소스의 모든 현재 표시를 요청 payload로 변경

- **DELETE**<br>
목적 리소스의 모든 현재 표시를 삭제

- **CONNECT**<br>
목적 리소스로 식별되는 서버로의 터널을 맺음

- **OPTIONS**<br>
목적 리소스의 통신 옵션을 설명(설정)

- **TRACE**<br>
목적 리소스의 경로를 따라 메시지 look-back 테스트 수행

---

### 📚 참고

[https://tools.ietf.org/html/rfc7231#section-4.2.1](https://tools.ietf.org/html/rfc7231#section-4.2.1)
[https://developer.mozilla.org/ko/docs/Web/HTTP/Methods](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
[https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/#disqus_thread](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/#disqus_thread)