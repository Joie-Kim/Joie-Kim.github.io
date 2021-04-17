---
title: "GET, POST, PUT, PATCH 비교"
layout: post
date: 2020-09-30 22:00
tag:
    - HTTP
    - Web
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

이전에 알아본 HTTP 메소드 중에서 헷갈릴 수 있는 것들을 비교해 정리했다.<br>
각 메소드 마다 어떤 특징이 있는지 숙지하고, 적절하게 사용하도록 하자.

---

## 👀 주요 특징을 한 눈에!

> Request, body의 개념을 알고 싶다면..<br>
> 👉 [HTTP](https://joie-kim.github.io/HTTP)<br>
> Safe, Idempotemt가 뭔지 궁금하다면..<br>
> 👉 [HTTP Method](https://joie-kim.github.io/HTTP-Method)<br>

![image](/assets/200930/HTTP-Method-02.jpeg)

---

## 🍎 GET vs POST

### GET

-   **서버로부터 정보를 조회하기 위해 설계된 메소드**
-   **필요한 데이터를 쿼리 스트링(query string)을 통해 전송**

    > **query string?** <br>: URL 끝에 ?와 함께 이름과 값으로 쌍을 이루는 요청 파라미터

    만약 요청 파라미터가 여러개이면 &로 연결한다.<br>
    예를 들어, <u>요청 파라미터명이 key1, key2</u>이고, 각각의 파라미터는 <u>value1, value2라는 값으로 서버에 요청</u>을 보낸다면 아래와 같이 하면 된다.

    ```
    www.example-url.com/resources?key1=value1&key2=value2
    ```

-   **안전함 O, 멱등성 O**

    서버의 변화 없이 정보 조회만 해서 항상 같은 응답을 받는다.

### POST

-   **리소스를 생성/변경하기 위해 설계된 메소드**

    따라서 GET과 달리 전송해야 될 데이터를 HTTP 메시지의 Body에 담아서 전송한다. (길이 제한 없이 데이터 전송 가능!)

-   **요청 헤더의 Content-Type에 요청 데이터의 타입을 표시 해야 함**

    데이터 타입을 표시하지 않으면 서버는 내용이나 URL에 포함된 리소스의 확장자명 등으로 데이터 타입을 유추한다.<br>
    만약 알 수 없으면 application/octect-stream으로 요청을 처리한다.

-   **안전함 X, 멱등성 X**

    리소스를 생성하는 과정에서 서버의 변화가 있고, 매번 다른 응답을 받는다.

---

## 🍊 POST vs PUT

### POST

-   **보통 INSERT의 개념으로 사용**

    삽입, 수정, 삭제 다 POST로 가능하지만, 적절하게 함수를 나눠 사용하는 것이 좋다.

-   **클라이언트가 리소스의 위치를 지정하지 않음 (/profile)**

    따라서, 요청을 여러 번 수행하면 매번 새로운 profile이 생성된다.<br>(profile/3, profile/4 등 매번 새로운 자원이 생성)

-   **안전함 X, 멱등성 X**

```
POST /profile HTTP/1.1
{"name": "joie", "age": 26}
HTTP/1.1 201 Created
```

### PUT

-   **보통 UPDATE의 개념으로 사용**
-   **클라이언트가 리소스의 위치를 명확하게 지정함 (/profile/3 또는 /profile/4)**

    따라서, 아무리 많이 수행 하더라도 동일한 리소스를 수정하기 때문에 여러 번 요청 하더라도 멱등하다.

-   **안전함 X, 멱등성 O**

    리소스를 변경하는 과정에서 서버의 변화가 있으나, 매번 같은 응답을 받는다.

```
PUT /profile/3 HTTP/1.1
{"name": "joie", "age": 26}
```

---

## 🍓 PUT vs PATCH

### PUT

-   **해당 자원의 전체 교체를 의미**

    따라서 동일한 리소스에 대해 여러 번 PUT을 수행하면 동일한 결과를 응답한다.

-   **안전함 X, 멱등성 O**

### PATCH

-   **해당 자원의 일부 변경을 의미**

    따라서 동일한 리소스에 대해 여러 번 PATCH를 수행하면 그 일부분을 여러 번 변경한 다른 결과를 응답한다.

-   **안전함 X, 멱등성 X**

    리소스를 변경하는 과정에서 서버의 변화가 있고, 매번 다른 응답을 받는다.

---

## 😋 이렇게 사용하자!

-   서버로부터 정보 조회 할 때는 <u><b>GET</b></u>
-   리소스를 생성(INSERT) 할 때는 <u><b>POST</b></u>
-   리소스를 전체 변경(Update all) 할 때는 <u><b>PUT</b></u>
-   리소스를 일부 변경(Update some) 할 때는 <u><b>PATCH</b></u>

---

### 📚 참고

[https://javaplant.tistory.com/18](https://javaplant.tistory.com/18)<br>
[https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/#disqus_thread](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/#disqus_thread)
