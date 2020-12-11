---
title: "ES6의 const와 let"
layout: post
date: 2020-12-11 23:00
tag:
- 용어_정리
- JS
hidden : false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

const는 ES6 문법에서 새로 도입되었으며 한 번 저장하고 나면 변경이 불가능한 상수를 선언할 때 사용하고,
let은 동적인 값을 담을 수 있는 변수를 선언할 때 사용한다.

ES6 이전에는 값을 담는 데에 var을 사용했는데, var는 scope이 함수 단위이다.
> scope : 해당 값을 사용할 수 있는 코드 영역

<br>

```jsx
function var_example() {
    var a = "hello";

    if(true) {
        var a = "bye";
        console.log(a);     // bye
    }

    console.log(a);         // bye
}
var_example();
```
if문 밖에서 var 값을 'hello'로 선언하고, if문 안에서 'bye'로 선언했다.<br>
if문 안에서 a의 값을 변경한 것이 함수 전체에 반영되어, if문 밖에서 a 값을 조회해도 'bye'가 출력된다.<br>
이런 점에 불편함을 느껴 나오게 된 것이 let과 const이다.

<br>

```jsx
function let_example() {
    let a = "hello";

    if(true) {
        let a = "bye";
        console.log(a);     // bye
    }

    console.log(a);         // hello
}
let_example();
```

<br>

```jsx
let a = 1;
let a = 2;  // Uncaught SyntaxError: Identifier 'a' has already been declared.
```
let과 const는 scope가 함수 단위가 아닌 블록 단위이므로, if문 밖에서 선언한 a 값과 if문 안에서 선언한 a 값이 각각 처리 된다.<br>
let과 const를 사용할 때, 블록 내부에서는 중복 선언이 불가능하다는 점에 주의해야 한다.

<br>

```jsx
const b = 1;
b = 2;  // Uncaught TypeError: Assignment to constant variable.
```
const는 let과 다르게 한 번 선언하면 재설정할 수 없다.

<br>

어떤 상황에 각 키워드를 사용해야 할까? 일단 ES6 문법에서는 var을 사용할 일은 없다. let은 한 번 선언한 후 값이 유동적으로 변할 수 있을 때만(ex: for문) 사용하고, const는 한 번 설정한 후 변할 일이 없는 값에 사용한다.

---

### 📚 참고
[리액트를 다루는 기술 (저자: 김민준, 출판사: 길벗)](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791160508796&orderClick=LEa&Kc=)