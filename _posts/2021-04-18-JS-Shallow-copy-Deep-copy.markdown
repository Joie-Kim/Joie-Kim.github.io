---
title: "[JS] 얕은 복사 VS 깊은 복사"
layout: post
date: 2021-04-18 14:00
tag:
    - JS
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

이전에 얕은 복사(Shallow copy)와 깊은 복사(Deep copy)에 대해 글을 작성한 적이 있었다.

> [얕은 복사(shallow copy) VS 깊은 복사(deep copy)
> ](https://joie-kim.github.io/Shallow-copy-Deep-copy/)

그때는 불변성(immutable)에 대해 완벽하게 이해 했다기 보단, 가볍게 '이런 성질이 있구나!'하고 새로운 걸 알게된 느낌이었다. 이번에 공부를 하고 그때 정리한 글을 다시 보니, 부족한 점이 보였다.

그래서 다시 정리해보려 한다. 레츠고! 🚗

`이번 글은 코어 자바스크립트 책의 1장(데이터 타입)을 공부한 내용을 바탕으로 작성한 글이다.`

---

# 얕은 복사, 깊은 복사 ?

얕은 복사(Shallow copy)는 바로 아래 단계의 값만 복사하는 방법이고, 깊은 복사(Deep copy)는 내부의 모든 값들을 하나하나 찾아서 전부 복사하는 방법이다.

## 예시로 이해해 보자.

```jsx
let user = {
    name: "joie",
    urls: {
        portfolio: "http://joie-kim.github.io/about",
        blog: "http://joie-kim.github.io/blog",
    },
};
```

이렇게 객체 안에 또 다른 객체가 있는 중첩 객체를 선언했다. 메모리에 아래 그림처럼 데이터가 할당될 것이다.
![image](/assets/210418/1.jpeg)

```jsx
let user2 = user;
user2.name = "huiju";

console.log(user.name === user2.name); // true
```

user을 복사한 user2를 선언하고, user2의 이름을 'huiju'로 변경했다. user의 이름과 user2의 이름은 huiju로 같다. 그 이유는 `let user2 = user;`에서 <u>주소값만 복사되는 <b>얕은 복사</b></u>를 했고, 객체 내부 프로퍼티가 가지는 주소값만 변경되고 객체의 주소값은 변경되지 않았기 때문이다. ([[코어 자바스크립트] 01. 데이터 타입](https://joie-kim.github.io/Core-JS-01/)글을 참고하자.)

이름을 변경하기 위해서는 내부의 값을 복사하는 **깊은 복사**를 해야 한다. 아래처럼 함수를 만들어서 내부 프로퍼티를 하나씩 복사하는 과정을 거쳐보자.

```jsx
// 객체 내부 프로퍼티를 하나씩 복사한 객체를 반환하는 함수
const copyObj = (target) => {
    let result;

    for (let prop in target) {
        result[prop] = target[prop];
    }

    return result;
};

// user 객체를 복사한 user2 선언
let user2 = copyObj(user);

// user2의 이름 변경
user2.name = "huiju";

console.log(user === user2); // false
console.log(user.name === user2.name); // false
console.log(user.url === user2.url); // true
```

아래 그림의 <span style='color:#fab005'>노란색 박스 부분</span>을 보면 객체 내부 프로퍼티를 하나씩 복사한 새로운 객체를 user2 변수에 할당한 것을 볼 수 있다. 그리고 <span style='color:#4c6ef5'>보라색 박스 부분</span>을 보면 user2의 이름을 'huiju'로 변경하기 위해 데이터 영역에 'huiju'를 새로 저장하고(만약 기존에 있었으면 그 데이터를 재활용), 그 주소값을 user2의 name 변수에 지정했다.
![image](/assets/210418/2.jpeg)

하지만 여전히 user와 user2의 url 객체는 동일한 주소값을 바라보고 있다. 이 부분 역시 **얕은 복사**가 진행됐기 때문이다. 함수를 조금 수정해 객체 내부 프로퍼티가 객체일 경우, 깊은 복사를 하고 값을 변경해 보자.

```jsx
// 객체 내부 프로퍼티를 하나씩 복사한 객체를 반환하는 함수
const copyObj = (target) => {
    let result;

    // 객체 내부 프로퍼티가 객체(참조형 데이터)일 경우, 해당 함수를 재귀 호출
    // 아닐(기본형 데이터) 경우, 복사
    if (typeof target === "object" && target !== null) {
        for (let prop in target) {
            result[prop] = copyObj(target[prop]);
        }
    } else {
        result = target;
    }

    return result;
};

// user 객체를 복사한 user2 선언
let user2 = copyObj(user);

// user2의 데이터 변경
user2.url.portfolio = " ";

console.log(user === user2); // false
console.log(user.url === user2.url); // false
```

> copyObj()의 조건문에서 `typeof target === "object"` 뒤에 `target !== null`을 붙인 이유는 **null**의 타입이 **object**이기 때문이다.

아래 그림의 <span style='color:#228be6'>파란색 박스 부분</span>이 복사 함수를 처음 실행한 복사한 결과고, <span style='color:#fab005'>노란색 박스 부분</span>이 두 번째 실행한 결과이다. 내부에 있는 모든 객체(참조형 데이터)를 복사해 새로운 객체로 저장했다.<br>
또한, <span style='color:#fa5252'>빨간색 박스 부분</span>을 보면 user2의 url 중 portfolio를 ' '(공백)으로 변경하기 위해 새롭게 저장하고, 그 주소값을 지정한 것을 확인할 수 있다. 깊은 복사를 했기 때문에 user의 데이터는 그대로이고, user2의 데이터만 변경 되었다.
![image](/assets/210418/3.jpeg)

## Tip) 깊은 복사를 간단하게 처리하기

위에서 구현한 깊은 복사를 간단하게 처리할 수 있는 방법이 있다. 바로 객체를 `JSON.stringify()`를 사용해 JSON 문법으로 표현된 문자열로 전환했다가 `JSON.parse()`를 사용해 다시 JSON 객체로 바꾸는 것이다.

다만, 메서드(함수)나 숨겨진 프로퍼티인 \_\_proto\_\_나 getter/setter 등과 같이 JSON으로 변경할 수 없는 프로퍼티들은 모두 무시한다. httpRequest로 받은 데이터를 저장한 객체를 복사할 때 등 <u>순수한 정보만 다룰 때 활용</u>하기 좋은 방법이다.

```jsx
const copyObjViaJSON = (target) => {
    return JSON.parse(JSON.stringify(target));
};

// user 객체를 복사한 user2 선언
let user2 = copyObjViaJSON(user);

console.log(user === user2); // false
```

<br>

---

### 📚 참고

[코어 자바스크립트 (저자: 정재남)](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791158391720&orderClick=LEa&Kc=)
