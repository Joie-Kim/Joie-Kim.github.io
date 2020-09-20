---
title: "JavaScript 엔진  VS  JavaScript 런타임"
layout: post
date: 2020-09-05 17:00
image: /assets/200905/v8-logo.png
headerImage: true
tag:
- 용어_정리
hidden : false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

JavaScript 엔진과 JavaScript 런타임.. 비슷하게 생겨서(?) 헷갈렸다..🤯<br>
어떤 차이점이 있는지 알고 싶어서 찾아봤다.

---

# JavaScript Engine
- 파싱과 JIT 컴파일(프로그램을 실제 실행하는 시점에 기게어로 번역하는 컴파일 기법)을 하는 머신을 제공한다.
- JavaScript로 쓰여진 스크립트를 기계가 실행할 수 있도록 한다.

<br>

# JavaScript Runtime
- Runtime(런타임) : 컴퓨터 프로그램이 실행되고 있는 동안의 동작
- JavaScript Runtime Environment(런타임 환경)은 프로그램이 실행되는 동안 사용 가능한 내장 라이브러리를 제공한다.<br>
만약 브라우저 안에서 BOM 또는 DOM을 사용하고자 한다면, 이 과정에 브라우저의 JavaScript 런타임 환경이 포함될 것이다.

> -**BOM** (Browser Object Model, 브라우저 객체 모델)<br>
-**DOM** (Document Object Model, 문서 객체 모델)

---

## 정리하자면..
전통적인 컴파일드 언어와 비교했을 때,
- 컴파일러 : 엔진(Engine)
- 링커(로드) : 런타임(runtime)

> -**Linker(링커)** : 컴파일러가 만들어낸 하나 이상의 목적 파일을 가져와 이를 단일 실행 프로그램으로 병합하는 프로그램.<br>
-**목적 파일** : 컴파일러나 어셈블러가 소스 코드 파일을 컴파일 또는 어셈블 해서 생성하는 파일. 기계어나 혹은 이에 준하는 RTL과 같은 이진 코드로 이루어져 있다.

---

### 📚 참고

[https://geonlee.tistory.com/91](https://geonlee.tistory.com/91)<br>
[http://tcpschool.com/javascript/js_bom_window](http://tcpschool.com/javascript/js_bom_window)<br>
[https://ko.wikipedia.org/wiki/링커_(컴퓨팅)](https://ko.wikipedia.org/wiki/%EB%A7%81%EC%BB%A4_(%EC%BB%B4%ED%93%A8%ED%8C%85))<br>
[https://ko.wikipedia.org/wiki/목적_파일](https://ko.wikipedia.org/wiki/%EB%AA%A9%EC%A0%81_%ED%8C%8C%EC%9D%BC)