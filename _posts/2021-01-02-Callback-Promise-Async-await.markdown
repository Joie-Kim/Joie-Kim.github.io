---
title: "Callback VS Promise VS async/await"
layout: post
date: 2021-01-02 22:00
tag:
- JS
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

JavaScript를 사용 하면서 꼭꼭꼭! 알아야 하고, 잘 사용해야 하는 게 있다.<br>
바로 **비동기 작업**에 대한 이해와 그를 바탕으로 **콜백 함수 잘 사용하기**이다. (aka. 콜백 지옥에 빠지지 않기.....🤦🏻‍♀️)<br>

사실 예전에 프로젝트를 하면서 한 번 공부 했었는데, 잘 이해 되지 않았었다..<br>
이번에 ReactJS 책에 해당 내용이 나와있어 이해한 부분을 정리 해두려 한다..!<br>
책에 나와있지 않은 내용은 친구의 도움으로 이해 할 수 있었다. ([이 친구의 블로그](https://velog.io/@seung3837/JavaScript-Callback-%EC%A7%80%EC%98%A5-%EB%B2%97%EC%96%B4%EB%82%98%EA%B8%B0)에도 해당 내용이 잘 정리가 되어 있다.)

---

# 비동기 작업의 이해

웹 애플리케이션을 만들다 보면 처리할 때 시간이 걸리는 작업이 있다. 예를 들어, 웹에서 서버 쪽 데이터가 필요해 서버의 API를 호출 해야 할 때, 네트워크 송수신 과정에서 시간이 걸리기 때문에 응답을 받을 때까지 기다렸다가 전달받은 데이터를 처리 해야 한다.<br>
이때 만약 작업을 동기적으로 처리한다면 데이터를 요청하고 기다리는 동안 작업이 중지되어 다른 작업들을 할 수 없다.<br>
하지만 이를 비동기적으로 처리하면 웹이 멈추지 않고, 동시에 여러 가지 요청을 처리하거나 응답을 기다리는 동안 다른 함수를 호출할 수 있다.<br>

## 이럴 때 비동기적으로 처리 하자
- 서버 API를 호출할 때
- DB 호출할 때
- setTimeout 함수를 사용할 때
> setTimeout 함수는 특정 작업을 예약할 때 사용한다. (즉, 일정 시간 이후에 동작 하도록 한다.)

# 콜백 함수
아래와 같이 파라미터 값이 주어지면 1초 뒤에 10을 더해서 반환하는 함수가 있다고 가정해 보자.
```jsx
function increase(number, callback) {
    setTimeout(() => {
        const result = number + 10;
        if (callback) {
            callback(result);
        }
    }, 1000);
}

increase(0, result => {
    console.log(result);
});
```

10, 20, 30, 40과 같은 형태로 여러 번 순차적으로 처리하고 싶다면 콜백 함수를 중첩하여 구현할 수 있다.
```jsx
function increase(number, callback) {
    setTimeout(() => {
        const result = number + 10;
        if (callback) {
            callback(result);
        }
    }, 1000);
}

console.log('start..');
increase(0, result => {
    console.log(result);  // 10
    increase(result, result => {
        console.log(result);   // 20
        increase(result, result => {
            console.log(result);  // 30
            increase(result, result => {
                console.log(result);  // 40
                console.log('end..');
            });
        });
    });
});
```

이렇게 콜백 안에 또 콜백을 넣어서 구현을 할 수 있지만, 너무 여러 번 중첩되어 코드의 가독성이 나빠졌다. 이런 형태를 흔히 **콜백 지옥**이라 한다. (지양하는 편이 좋다.)

# Promise
Promise는 콜백 지옥을 해결하기 위한 방안으로 ES6에 도입된 기능이다.<br>
Promise 내부에 코드를 작성해 코드가 정상적으로 작동한다면 `resolve`, 비정상적으로 작동한다면 `reject`를 지정할 수 있다.<br>
또한, 해당 Promise를 할당 받은 변수에서 `.then()`, `.catch()`, `.finally()` 등으로 결과 값을 처리할 수 있다.

## Promise의 상태
Promise는 다음 중 하나의 상태를 가진다.
- **pending**: 초기 상태
- **fullfilled**: 연산이 성공적으로 완료된 상태
- **rejected**: 연산이 실패한 상태
> Promise는 대기 중이지 않으며, fullfilled 또는 rejected 됐을 때 처리(settled)됐다고 말한다.

## Promise의 Prototype
- **Promise.prototype.then()** : Promise에서 resolve된 value를 처리한다.
- **Promise.prototype.catch()** : Promise에서 reject된 error를 처리한다.
- **Promise.prototype.finally()** : Promise에서 resolve인지 reject인지 상관없이 동작한다

위에서 콜백으로 구현한 코드를 Promise를 사용해 바꾸면 다음과 같다.
```jsx
function increase(number) {
    const promise = new Promise((resolve, reject) => {
        // resolve: 성공, reject: 실패
        setTimeout(() => {
            const result = number + 10;
            if (result > 50) {
                const e = new Error('Number is too big');
                return reject(e);
            }
            resolve(result);
        }, 1000);
    });
    return promise;
}

increase(0)
    .then(number => {
        // Promise에서 resolve된 값은 .then을 통해 받아 올 수 있음
        console.log(number);        // 10
        return increase(number);    // Promise를 리턴하면
    })
    .then(number => {
        // 또 .then으로 처리 가능
        console.log(number);        // 20
        return increase(number);
    })
    .then(number => {
        console.log(number);        // 30
        return increase(number);
    })
    .then(number => {
        console.log(number);        // 40
        return increase(number);
    })
    .then(number => {
        console.log(number);
        return increase(number);
    })
    .catch(e => {
        // 도중에 에러가 발생한다면 .catch를 통해 알 수 있음
        console.log(e);
    });
```

# async/await
async/await는 Promise를 더욱 쉽게 사용할 수 있도록 해 주는 ES2017(ES8) 문법이다. 이 문법을 사용하려면 함수의 앞부분에 async 키워드를 추가하고, 해당 함수 내부에서 Promise의 앞부분에 await 키워드를 사용한다. 이렇게 하면 Promise가 끝날 때까지 기다리고, 결과 값을 특정 변수에 담을 수 있다.
```jsx
function increase(number) {
    const promise = new Promise((resolve, reject) => {
        // ... 위 코드와 동일 ...
    })
}

async function runTasks() {
    try {
        let result = await increase(0);
        console.log(result);                // 10
        result = await increase(result);
        console.log(result);                // 20
        result = await increase(result);
        console.log(result);                // 30
        result = await increase(result);
        console.log(result);                // 40
        result = await increase(result);
        console.log(result);
    } catch (e) {
        console.log(e);
    }
}
```

---

### 📚 참고

[리액트를 다루는 기술 (저자: 김민준)](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791160508796&orderClick=LAG&Kc=)
