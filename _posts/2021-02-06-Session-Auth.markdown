---
title: "Session 기반 인증 방식"
layout: post
date: 2021-02-06 19:00
tag:
    - Web
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

### 인증 방식을 알아보자 1편

리액트를 사용해 웹 환경에서 CRUD를 구현해 보기 위해 간단한 블로그 앱을 만들어 보고 있다. 게시글 CRUD 뿐만 아니라 회원가입과 로그인 기능까지 구현할 예정이다.

회원 관리를 넣으려 하니 이런 생각이 들었다. 블로그에서 새 글 작성이나 글 수정은 로그인한 사용자만 할 수 있는데, 그 기능들을 사용할 때마다 어떻게 확인을 해야 할까?

매번 기능을 사용할 때마다, 그러니까 클라이언트에서 서버에 요청을 보낼때마다 사용자 인증을 해야 한다면 어떨까? 당연히 매우 불편할 것이다. 따라서 로그인한(즉, 인증된) 사용자임을 식별하는 무언가가 필요하다.

먼저 Session 기반 인증 방식에 대해 알아보자.

---

<br>

# 👀 용어 정리

## Session

세션이란 일정 시간 동안 같은 사용자(정확하게는 브라우저)로부터 들어오는 일련의 요청들을 하나의 상태로 보고, 그 상태를 일정하게 유지시키는 기술이다. 여기서 일정 시간이라는 건 사용자가 웹 브라우저를 통해 휍 서버에 접속한 시점부터 웹 브라우저를 종료해 연결을 끝내는 시점까지를 말한다. 즉, 세션은 사용자가 웹 서버에 접속해 있는 동안 상태를 유지할 수 있도록 하는 것이다.

세션은 웹 서버가 세션 아이디 파일을 만들어 운영되는 서버에 저장을 한다.

## Cookie

쿠키란 특정 웹 사이트를 방문했을 때 만들어지는 정보를 담는 파일을 뜻하며, 이 또한 상태 정보를 유지하는 기술이다.

쿠키는 세션과 다르게 사용자의 정보를 사용자 컴퓨터의 메모리에 저장한다. (크롬에서 웹사이트 비밀번호를 기억하는 것을 떠올려보자.)

---

<br>

# ⚙️ 동작 방식

![image](/assets/210206/Session_auth.jpeg)

1. 사용자가 로그인을 요청한다. ID, Password 정보가 유효하다면(DB에 저장된 값이라면), 세션을 생성하고 서버의 메모리에 저장한다. 세션 식별키인 SessionId를 기준으로 정보를 저장하며, 조회할 때도 사용된다.

2. 서버에서 SessionId를 쿠키에 담아 사용자(웹 브라우저)에게 전달한다.

3. 사용자는 Request 할 때 SessionId가 담긴 쿠키를 포함한다. (나 로그인한 사용자야!라고 서버에게 알리기 위해)

4. 서버는 사용자가 보낸 SessionId로 서버에 저장된 세션을 조회한다. 세션 정보가 유효하다면 Response한다.

> **✨ 함께 알면 좋은 개념** <br>
> 세션 기반 인증방식을 사용하게 되면 사용자로부터 요청(request)을 받을 때, 사용자의 상태를 서버의 메모리에 저장하고 유지한다. 그리고 그 정보를 서비스 제공에 이용한다. (예를 들어, 로그인 한 사용자만 글을 쓸 수 있다거나..)
> 이렇게 동작하는 서버를 Stateful Server라고 한다.

---

<br>

# 👍 좋은 점

1. 서버에서 사용자의 상태를 유지하고 있어, 사용자의 로그인 여부 확인이 용이하다. 경우에 따라 강제 로그아웃 등의 제재를 가할 수도 있다.
2. 사용자가 임의로 정보를 변경하더라도 서버에서 사용자의 상태 정보를 가지고 있기 때문에 상대적으로 안전하다.

<br>

# 🙏 아쉬운 점

1. 서버에 과부화가 생긴다.

    서버에 모든 사용자의 상태 정보를 저장 및 유지 해야 하므로, 사용자의 수가 증가할 수록 서버에 과부화가 생긴다.

2. Stateful server이기 때문에 로드 밸런싱을 사용한 서버 확장이 어렵다.

    만약 A라는 유저가 1번 서버에게 로그인 요청을 해 1번 서버에 A 유저에 대한 세션이 생겼다고 하자. 이후 A 유저가 로드 밸런싱으로 인해 3번 서버에 요청을 보내게 된다면 A 유저에 대한 세션이 3번 서버에는 없기 때문에 또 다시 인증(로그인)을 해야 하는 상황이 생긴다.<br>
    DB에 저장하거나, 세션 정보를 모든 서버에 동기화 하는 방법을 사용 할 수 있겠지만, 이럴 경우 과부화가 생긴다.

3. CORS 방식 사용시 어려움이 있다.

    웹 브라우저에서 세션 관리에 사용하는 쿠키는 단일 도메인 및 서브 도메인에서만 작동하도록 설계 되어 있다. 따라서 CORS 방식(여러 도메인에서 Request를 보내는 브라우저)을 사용할 때 쿠키 및 세션 관리가 어렵다.

<br>

---

### 📚 참고

[Session 기반 인증 방식](https://surprisecomputer.tistory.com/36)
