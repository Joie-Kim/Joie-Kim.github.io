---
title: "Node.js와 친해지기"
layout: post
date: 2020-09-06 20:00
image: /assets/200906/nodejs-logo.png
headerImage: true
tag:
- Node.js
- Server
- JavaScript
hidden : false
star: true
category: blog
author: Joie-Kim
description: Markdown summary with different options
---
<p>
서버 공부를 Node.js로 시작하기로 했다.<br>
학부때 들었던 웹 서버 수업에서 Node.js를 사용했어서 복습하면서 빠르게 서버에 대한 개념을 익힐 수 있을 거 같아 선택했다. <br>
<br>
공부하면서 중요한 개념들을 정리했다.<br>
</p>
---

# 🤷🏻‍♀️ Node.js란?

- Node.js는 구글 크롬의 자바 크롬의 JavaScript 엔진(V8 Engine)으로 빌드 된 JavaScript 런타임이다.

> 엔진과 런타임이 뭔지 궁금하다면 아래 링크로 가보자.<br>
👉 [JavaScript 엔진 VS JavaScript 런타임](https://joie-kim.github.io/JavaScript-engine-runtime/)

- JavaScript는 일반적으로 브라우저에 내장되어 있다. (종속 되어 있음)<br>
  JavaScript를 브라우저 밖에서 다양한 용도로 사용하기 위해 Node.js가 만들어졌고, 이를 활용하면 File System를 사용하거나 서버를 만들 수 있다.<br>즉, Node.js는 JavaScript를 활용하는 방법 중 하나다.

> Node.js는 Express 같은 라이브러리를 이용해서 서버를 만들 수 있으며, Node.js 자체가 웹 서버인 것은 아니다. 웹 서버를 만들 수 있는 하나의 방법일 뿐!

---

## 🛠 구조 (architecture)

Node.js 구조는 아래와 같다.(~~강의 자료에 나와있는 내용을 정리해서 그렸다.~~)<br>
V8 엔진 위에서 동작한다는 걸 알 수 있다.

![AppPage](/assets/200906/architecture.jpeg)

---

## 🔫 특징
### 1. I/O(입출력) 방식

**(1) 비동기 방식 → Non-Blocking IO**<br>: 하나의 요청 처리가 끝날 때까지 기다리지 않고, 다른 요청을 동시에 처리한다.

![AppPage](/assets/200906/non-blocking.jpeg)

비동기식(Non-Blocking) 방식은 파일 내용을 읽은 후 콜백 함수로 반환한다.<br>파일 읽기가 시작되면 즉시 다음 실행 순서로 넘어간다.
  
{% highlight js %}
file.read('a.txt', function(contents){
    doShow(contents);
});

var result = doAdd(10,10); 
{% endhighlight %}

<br>

cf) 동기 방식 → Blocking IO<br>: 파일 내용을 다 읽고 난 후에 다른 일을 처리한다.

![AppPage](/assets/200906/blocking.jpeg)

동기(Blocking) 방식은 파일의 내용을 읽어 함수의 결과값으로 반환한다.<br>결과값이 반환될 때까지 대기한다.

{% highlight js %}
var contents = file.read('a.txt');
// ----------
//    대기
// ----------
doShow(contents);
var result = doAdd(10,10);
{% endhighlight %}

<br>

**(2) 이벤트 기반 방식 → Event Driven IO**<br>: 콜백 함수는 이벤트를 받았을 때 실행 된다.

![AppPage](/assets/200906/event-driven.jpeg)

### 2. 빠른 속도

구글 크롬(Google Chrome)의 V8 JavaScript 엔진을 사용하여 코드 실행이 빠르다.

### 3. 단일 스레드 (Single Thread)

문맥 교환으로 인한 오버 헤드가 없다.

### 4. 풍부한 라이브러리

모듈과 패키지를 사용하면서 서버 프로그램을 구성하는데, 이미 다른 사람들이 만들어 놓은 모듈이 굉장히 많다.

---

## 🖇 함께 알아두기!
**npm**<br>: Node Package Manager 또는 Node Package Modules

> node.js에서 사용하는 모듈을 패키지로 만들어 npm을 통해 관리하고 배포한다. 다른 사람이 만들어 놓은 모듈을 npm을 통해 설치하여 사용할 수 있다.

**express**<br>: node.js를 사용해 웹 앱을 만들기 위한 틀을 제공하는 프레임워크
