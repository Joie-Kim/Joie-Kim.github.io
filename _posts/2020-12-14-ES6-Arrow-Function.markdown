---
title: "ES6의 화살표 함수"
layout: post
date: 2020-12-14 17:30
tag:
    - 용어_정리
    - JS
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

화살표 함수(arrow function)는 ES6 문법에서 함수를 표현하는 새로운 방식이다.<br>
기존 function을 이용한 함수 선언 방식을 아예 대체 하는 것은 아니며, 사용 용도가 다르다.<br>
이 문법은 주로 함수를 파라미터로 전달할 때 유용하다.

```jsx
// 기존 function 이용
setTimeout(function () {
    console.log("Hi~ I'm Joie!");
}, 1000);

// 화살표 함수 이용
setTimeout(() => {
    console.log("Hi~ I'm Joie!");
}, 1000);
```

<br>

화살표 함수 문법과 기존 function과의 가장 큰 차이점은 서로 가리키고 있는 `this`의 값이 다르다는 것이다.<br>
아래 코드로 이해해 보자.

function으로 작성된 함수는 자신이 종속된 객체를 this로 가리킨다.<br>
여기서는 return 다음 대괄호({})로 묶인 객체가 this가 된다.

```jsx
function Dog() {
    this.name = "white dog";
    return {
        name: "black dog",
        bark: function () {
            console.log(this.name + ": bowwow!!");
        },
    };
}

const dog = new Dog();
dog.bark(); // balck dog: bowwow!!
```

만약 function을 이용해서 'white dog: bowwow!!'를 출력하고 싶다면 다른 객체를 선언하거나, 함수를 이용해야 한다. (아래는 `that` 객체에 this 값을 넣는 방식을 사용했다.)

```jsx
function Dog() {
    this.name = "white dog";
    let that = this;
    return {
        name: "black dog",
        bark: function () {
            console.log(that.name + ": bowwow!!");
        },
    };
}

const dog = new Dog();
dog.bark(); // white dog: bowwow!!
```

화살표 함수는 자신이 종속된 인스턴스를 가리킨다.<br>
여기서는 WhiteDog()로 생성된 whiteDog 인스턴스가 this가 된다.

```jsx
fucntion Dog() {
    this.name = 'white dog';
    return {
        name: 'black dog',
        bark: () => {
            console.log(this.name + ': bowwow!!');
        }
    }
}

const dog = new Dog();
dog.bark(); // white dog: bowwow!!
```

<br>

또한, 화살표 함수는 값을 연산하여 바로 반환해야 할 때 사용하면 가독성을 높일 수 있다.

```jsx
// 기존 function 이용
function twice(value) {
    return value * 2;
}

// 화살표 함수 이용
const triple = (value) => value * 3;
```

따로 {}를 열어 주지 않으면 연산한 값을 그대로 반환한다.

---

### 📚 참고

[리액트를 다루는 기술 (저자: 김민준, 출판사: 길벗)](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791160508796&orderClick=LEa&Kc=)
