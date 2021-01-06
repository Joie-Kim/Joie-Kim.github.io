---
title: "모듈 VS 라이브러리 VS 프레임워크"
layout: post
date: 2020-09-08 12:30
tag:
- Etc
hidden : false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

<p>
프로그램은 작고 단순한 것에서 크고 복잡한 것으로 진화한다.<br>
그 과정에서 <b>코드의 재활용과 유지보수를 쉽게 하기 위해</b> 다양한 기법들이 사용된다. <br>
그 중 하나가 <b>코드를 여러 개의 파일로 분리하는 것</b>이며, 장점은 다음과 같다.<br>
<br>
(1) 자주 사용되는 코드를 별도의 파일로 만들어 필요할 때마다 사용할 수 있다.<br>
(2) 별도의 파일로 된 코드를 개선하면 이를 사용하고 있는 모든 애플리케이션의 동작이 개선된다.<br>
(3) 로직을 변경할 때, 코드에서 수정이 필요한 부분을 빠르게 찾을 수 있다.<br>
(4) 필요한 로직을 로드해 사용하기 때문에 메모리 낭비를 줄일 수 있다.<br>
(5) 한 번 다운로드 하면 웹 브라우저에 의해 저장되기 때문에 동일한 로직을 로드 할 때 시간과 네트워크 트래픽을 절약할 수 있다. (브라우저에만 해당)
</p>

<br>

이렇게 분리하는 기법을 공부하다 보면 모듈, 라이브러리, 프레임워크라는 용어들이 등장하는데, 제대로 알고 사용하기 위해 찾아봤다. 🐣🐥

---

## Module

- 별도의 파일로 분리된 독립된 기능을 뜻한다.<br>
(순수한 JavaScript에서는 모듈이라는 개념이 분명하게 존재하지 않는다고 한다.)<br>
JavaScript가 구동되는 호스트 환경에 따라서 서로 다른 모듈화 방법이 제공된다.

> 호스트 환경?<br>: JavaScript가 구동되는 환경을 의미한다. 예를 들어, Node.js는 JavaScript의 문법을 따르지만 이 언어가 구동되는 환경은 브라우저가 아니라 시스템을 제어하는 서버 측 환경이다.

- 모듈을 만들면 다른 파일에서 모듈을 불러와 사용할 수 있다.<br>
Node.js에서는 CommonJs 표준 스펙을 따르며, exports 전역 객체를 사용한다.

{% highlight js %}
// module.js (1)
// export 사용

exports.add = function(a, b){
    return a + b;
};
exports.multiply = function(a. b){
    return a * b;
};
{% endhighlight %}

{% highlight js %}
// module.js (2)
// module.exports 사용해서 객체 직접 할당

var calc = {};

calc.add = function(a, b){
    return a + b;
};
calc.multiply = function(a, b){
    return a * b;
};

module.exports = calc;
{% endhighlight %}

방법 (1) 또는 (2)를 사용해서 module.js를 만들고, main.js에서 다음과 같이 불러와 사용하면 된다.

{% highlight js %}
// main.js
// 모듈을 사용할 때, 절대 경로 또는 상대 경로를 정확하게 입력해야 함

//var module1 = require('../test/module.js'); //가능
var module1 = require('./module.js');
console.log(module1.add(3,4));
console.log(module1.multiply(3,4));
{% endhighlight %}


## Library

- 라이브러리는 모듈과 비슷한(거의 같은) 개념이다.
- 생활 코딩에서 설명하는 차이점은 다음과 같다.

> 모듈이 프로그램을 구성하는 작은 부품의 느낌이라면, 라이브러리는 자주 사용 되는 로직을 잘 정리한 집합 느낌이다.

- [위키백과](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC_(%EC%BB%B4%ED%93%A8%ED%8C%85))에서는 라이브러리를 다음과 같이 설명하고 있다.

> 컴퓨터 프로그램이 사용하는 비휘발성 자원의 모임으로 구성 데이터, 문서, 도움말 자료, 메시지 틀, 미리 작성된 코드, 서브루틴(함수), 클래스, 값, 자료형 사양을 포함할 수 있다.

## Framework

- 복잡한 문제를 해결하거나 서술하는 데에 사용 되는 기본 개념 구조, 즉 뼈대이다.
- 애플리케이션 개발에 바탕이 되는 템플릿과 같은 역할을 하는 클래스들과 인터페이스의 집합으로 애플리케이션을 구축할 때 공통적인 부분을 제공한다.

---

### 이렇게 이해 했다.

로봇을 만드는 **공장(Framework)**이 있다. <br>
이 공장 안에는 로봇의 머리, 몸통, 팔, 다리를 만들 수 있는 **파트(Library)**가 있다. <br>
로봇의 머리를 만드는 파트에는 눈, 코, 입을 생산하는 **기계(Module)** 3개가 있다. <u>(Library > Module)</u> <br>
로봇의 몸통을 만드는 파트에는 기계가 1개 있다. <u>(Library = Module)</u> <br>

    잘못된 부분이 있다면 피드백 plz..🙏🏻

---

### 📚 참고

[https://brownbears.tistory.com/437](https://brownbears.tistory.com/437) <br>
[https://ko.wikipedia.org/wiki/라이브러리_(컴퓨팅)](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC_(%EC%BB%B4%ED%93%A8%ED%8C%85))<br>
[https://opentutorials.org/course/743/4750](https://opentutorials.org/course/743/4750)<br>
[https://www.it-swarm.dev/ko/python/파이썬에서-모듈과-라이브러리의-차이점은-무엇입니까/1041805782/](https://www.it-swarm.dev/ko/python/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%97%90%EC%84%9C-%EB%AA%A8%EB%93%88%EA%B3%BC-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9E%85%EB%8B%88%EA%B9%8C/1041805782/)<br>
[https://ko.wikipedia.org/wiki/소프트웨어_프레임워크](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)<br>
[https://m.blog.naver.com/manddonara/119738492](https://m.blog.naver.com/manddonara/119738492)