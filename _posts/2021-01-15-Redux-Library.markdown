---
title: "Redux 라이브러리 이해하기"
layout: post
date: 2021-01-15 18:00
tag:
    - ReactJS
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

**[리액트를 다루는 기술 책](<(http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791160508796&orderClick=LAG&Kc=)>)**의 16장을 공부하며 정리한 내용이며, ReactJS가 아닌 VanilaJS를 사용하여 리덕스를 실습한 것을 바탕으로 작성 되었다.

> 리덕스는 리액트에서 사용하기 위해 만들어졌지만 다른 UI 라이브러리/프레임워크와 함께 사용할 수도 있다.

> vanila JS<br>: 라이브러리나 프레임워크 없이 사용하는 순수 자바스크립트 그 자체를 의미한다.

<b>실습 코드를 확인하고 싶다면 👉 [깃헙 레파지토리](https://github.com/Joie-Kim/ReactJS/tree/master/vanila-redux)</b>

<br>

---

# 🤟 Redux?

리덕스는 리액트 상태 관리 라이브러리이다.<br>
리덕스를 사용하면 컴포넌트의 <u>상태 업데이트 관련 로직을 다른 파일로 분리</u>시켜서 더욱 효율적으로 관리할 수 있다.
또한, <u>컴포넌트끼리 똑같은 상태를 공유해야 할 때</u>도 여러 컴포넌트를 거치지 않고 손쉽게 상태 값을 전달하거나 업데이트할 수 있다.

Context API처럼 전역 상태를 관리할 때 효과적인데,
리액트 v16.3가 릴리즈되기 이전에는 Context API 사용이 불편했기 때문에 주로 리덕스로 상태 관리를 해왔다.<br>

> 최근 Context API가 개선되어 리덕스 사용이 줄어들 것이라는 글을 어디선가 봤다. 하지만 알고 사용하지 않는 것과 몰라서 사용하지 못 하는 건 다르니 사용법을 익혀두자.

<br>

# 🤞 키워드 개념

## 1. 액션

상태에 어떠한 변화가 필요하면 액션(action)이 발생한다. 이는 <u>하나의 객체로 표현</u>된다.

액션 객체는 `type` 필드를 반드시 가지고 있어야 한다. 이 값을 <u>액션의 이름</u>이라고 생각하면 된다. 그리고 그 외의 값들은 나중에 상태 업데이트를 할 때 참고해야 할 값이며, 작성자 마음대로 넣을 수 있다.

다음은 액션의 에시이다.

```jsx
// 숫자를 더하는 액션
{
  type: 'INCREASE',
  difference: 1
}

// 숫자를 빼는 액션
{
  type: 'DECREASE',
}

// 데이터를 추가하는 액션
{
  type: 'ADD_DATA',
  data: {
    id: 1,
    text: '액션 예시입니다.'
  }
}
```

## 2. 액션 생성 함수

액션 생성 함수(action creator)는 <u>액션 객체를 만들어 주는 함수</u>이다.

어떤 변화를 일으켜야 할 때마다 액션 객체를 만들어야 하는데 매번 액션 객체를 직접 작성하기 번거로울 수 있고, 만드는 과정에서 실수로 정보를 놓칠 수도 있다. 이러한 일을 방지하기 위해 이를 함수로 만들어 관리한다.

```jsx
// 화살표 함수
const increase = (difference) => ({ type: "INCREASE", difference });
const decrease = () => ({ type: "DECREASE" });

// 기존 함수형
function addData(data) {
    return {
        type: "ADD_DATA",

        data,
    };
}
d;
```

## 3. 리듀서

리듀서(reducer)는 <u>변화를 일으키는 함수</u>이다. 액션을 만들어서 발생시키면 리듀서가 현재 상태와 전달받은 액션 객체를 파라미터로 받아 온다. 그리고 두 값을 참고하여 <u>새로운 상태를 만들어서 반환</u>해 준다.

리듀서 코드는 다음과 같은 형태로 이루어져 있다.

```jsx
// 초기 상태 설정
const initialState = {
  counter: 0
};

// 리듀서 함수 정의
function reducer(state = initialState, action) {
  switch (action.type) {
	  case INCREMENT:
		  return {
			  ...state // 불변성 유지
			  counter: state.counter + action.difference
		  };
	  case DECREASE:
		  return {
			  ...state
			  counter: state.counter - 1
		  };
	  default:
		  return state;
  }
}
```

## 4. 스토어

프로젝트에 <u>리덕스를 적용하기 위해</u> 스토어(store)를 만든다. 한 개의 프로젝트는 <u>단 하나의 스토어만</u> 가질 수 있다. 스토어 안에는 현재 애플리케이션 상태와 리듀서가 들어가 있으며, 그 외에도 몇 가지 중요한 내장 함수를 지닌다.

```jsx
import { createStore } from "redux";

(...)

// 스토어 생성
const store = createStore(reducer);
```

## 5. 구독

구독(subscribe)도 스토어의 내장 함수 중 하나이다. `subscribe` 함수 안에 리스너 함수를 파라미터로 넣어서 호출해 주면, 액션이 디스패치되어 <u>상태가 업데이트될 때마다 리스너 함수가 호출</u>된다.

아래 예시에서는 `render` 함수를 호출하도록 했는데, 리액트의 `render` 함수와 다르게 이미 만들어진 UI의 속성을 상태에 따라 변경한다.

또한, 리액트에서는 아래처럼 `subscribe` 함수를 직접 사용하지 않는다. `react-redux` 라이브러리가 이 작업을 대신 해 주기 때문이다.

```jsx
// html 객체
const counter = document.querySelector("h1");

(...)

// render 함수 생성
const render = () => {
  const state = store.getState();

  counter.innerText = state.counter;
};

render();

// 구독
// 스토어의 상태가 바뀔 때마다 render 함수가 호출되도록 한다.
store.subscribe(render);
```

## 6. 디스패치

디스패치(dispatch)는 스토어의 내장 함수 중 하나이다. 디스패치는 <u>'액션을 발생시키는 것'</u>이라고 이해하면 된다. 이 함수는 `dispatch(actio)`과 같은 형태로 액션 객체를 파라미터로 넣어서 호출한다.

이 함수가 호출되면 스토어는 리듀서 함수를 실행시켜서 새로운 상태를 만들어 준다.

```jsx
// html 객체
const btnIncrease = document.querySelector("#increase");
const btnDecrease = document.querySelector("#decrease");

(...)

btnIncrease.onclick = () => {
  store.dispatch(increase(1));
  console.log("+1");
};
btnDecrease.onclick = () => {
  store.dispatch(decrease());
  console.log("-1");
};
```

<br>

# 🤙 사용 규칙

## 1. 단일 스토어

<u>하나의 애플리케이션 안에는 하나의 스토어</u>가 있어야 한다. (1 App 1 Store)<Br>
특정 업데이트가 너무 빈번하게 일어나거나 애플리케이션의 특정 부분을 완전히 분리시킬 때 여러 개의 스토어를 만들 수 있지만, 상태 관리가 복잡해질 수 있어 권장하지 않는다.

## 2. 읽기 전용 상태

리덕스 상태는 읽기 전용이기 때문에 상태를 업데이트할 때 <u>기존의 객체는 건드리지 않고</u> 새로운 객체를 생성해 주어야 한다. (즉, 불변성을 지켜줘야 한다.)

리덕스에서 불변성을 유지해야 하는 이유는 좋은 성능을 유지하기 위해 내부적으로 데이터가 변경되는 것을 감지하기 위해 얕은 비교(shallow equality)를 하기 때문이다.

## 3. 리듀서는 순수한 함수

변화를 일으키는 리듀서 함수는 순수한 함수로 다음 조건을 만족해야 한다.

-   리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받는다.
-   파라미터 외의 값에는 의존하면 안 된다.
-   이전 상태는 절대 건드리지 않고, 변화를 준 새로운 상태 객체를 만들어서 반환한다.
-   <u>똑같은 파라미터</u>로 호출된 리듀서 함수는 언제나 <u>똑같은 결과 값</u>을 반환해야 한다.

만약 리듀서 함수 내부에 랜던 값을 만들거나, Date 함수로 현재 시간을 가져오는 등의 작업을 하면 안 된다. 파라미터가 같아도 다른 결과를 만들어 낼 수 있기 때문이다.

<br>

---

### 📚 참고

[리액트를 다루는 기술 (저자: 김민준)](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791160508796&orderClick=LAG&Kc=)
