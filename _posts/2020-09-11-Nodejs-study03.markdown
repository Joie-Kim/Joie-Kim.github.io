---
title: "해시(Hash)와 Crypto 모듈"
layout: post
date: 2020-09-11 20:00
tag:
- Node.js
- Server
hidden : false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---
<p>
Node.js에서 사용하는 내장 모듈 중 하나인 <b>Crypto 모듈</b>에 대해 공부 하다가<br>
<b>해시(Hash)</b>에 대해 좀 더 찾아보고 정리했다.<br>
</p>
---

## 🧐 해시(Hash)란?
해시 함수(Hash Function) 또는 해시 알고리즘을 이용해 고정된 길이의 문자열로 바꾸는 것을 의미한다.

```
- 해시 함수 (Hash Function) : 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑
- 키 (Key) : 매핑 전 원래 데이터의 값
- 해시 값 (Hash Value) : 매핑 후 데이터의 값
- 해싱 (Hashing) : 매핑 하는 과정
```

![image](/assets/200911/hash.jpeg)

위 그림과 같이 비밀번호를 암호화할 수 있는데, 해시는 단방향 암호화 기법으로 복호화 할 수 없다. (다만 몇 가지 암호화 알고리즘은 뚫렸다고.. <u>MD5나 SHA1 방식은 절대 네버 사용 금지</u>)

```
복호화 : 암호화된 문자열을 다시 원래 문자열로 돌려놓는 것
```

즉, 사용자가 웹 사이트에 가입하면 서버는 사용자의 비밀번호를 해시 값으로 저장하고, 서버에는 사용자 비밀번호의 원본이 남지 않는다.

해시 알고리즘은 특정 입력에 대해 항상 같은 값을 반환하기 때문에 사용자가 로그인하면 비밀번호를 해시화 해 비교할 수 있다.
> **참고!**<br>
원본 문자열이 조금이라도 다르면 해시 형태가 굉장히 많이 달라진다. 이를 <u>눈사태 효과 또는 쇄도 효과(avalanche effect)</u>라고 한다.

---

## 🔐 Crypto 모듈로 해시를 사용해 보자.
### (1) 기본적인 사용

```jsx
var crypto = require('crypto');

var shasum = crypto.createHash('sha512');
shasum.update('PASSWORD');
var output = shasum.digest('hex');

// 한 번에 써도 됨!
//var output = crypto.createHash('sha512').update('PASSWORD').shasum.digest('hex');

console.log('PASSWORD : ', output);
```

- **createHash( )** : 인자로 사용할 알고리즘을 넣어준다.
    > sha256, sha512와 같은 것이 있는데, 둘 중 sha512가 더 길고 안전하다.
- **update( )** : 인자로 암호화할 비밀번호를 넣어준다.
- **digest( )** : 인자로 어떤 인코딩 방식으로 암호화된 문자열을 표시할지 넣어준다.
    > base64, hex, latin1 등의 방식이 있다.

입력은 길이 제한 없이 무한정으로 만들 수 있지만, 해시 값은 항상 고정된 길이의 값으로 나타내기 때문에 다른 입력이 같은 해시 값으로 나오는 경우가 있을 수 있다. (해시 충돌이 일어날 수 있다.)

### (2) 솔트(salt) 사용하기

최근에는 해커들이 "A의 해시 값이 B다."를 알고 있어 보안에 좀 더 신경 써야만 한다.
> 그렇게 정리 해둔 데이터베이스를 <u>레인보우 테이블</u>이라고 한다.

유출 피해를 최소화 하기 위해 비밀번호에 솔트(salt)라는 특정 값을 넣거나, 해시 함수를 여러 번 수행 하거나, 수행 횟수를 다르게 하는 등의 방법을 사용하는 것이 좋다.

```jsx
crypto.randomBytes(64, (err, buf) => {
  crypto.pbkdf2('PASSWORD', buf.toString('hex'), 100000, 64, 'sha512', (err, key) => {
    console.log(key.toString('hex'));
  });
});
```

- **randomBytes( )** : 64 bytes 길이의 솔트를 생성한다.
- **pbkdf2( 비밀번호, 솔트, 반복 횟수, 비밀번호 길이, 해시 알고리즘 )**
- **buf.toString( )** : hex 문자열의 솔트로 변경한다.

위 방법은 솔트를 사용하면서 해시 함수를 반복 수행하도록 했다.<br>
주의할 점은 randomBytes( )가 매번 다른 솔트 값을 뱉어내기 때문에 솔트를 비밀번호(key)와 함께 저장해야 한다.

```jsx
crypto.pbkdf2('입력비밀번호', '기존salt', 100000, 64, 'sha512', (err, key) => {
  console.log(key.toString('hex') === '기존 비밀번호');
});
```

이렇게 솔트 값, 반복 횟수, 비밀번호 길이, 해시 알고리즘, 인코딩 방식 모두 같아야 같은 값으로 인식한다.

<br>
해킹 수법이 진화하는 것처럼 암호화 기법도 진화하고 있다고 한다.<br>
다음에 다른 암호화 기법들도 찾아봐야겠다.

---

### 📚 참고

<a href="http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788968482946&orderClick=LEa&Kc=">모던 웹을 위한 Node.js 프로그래밍 (저자:윤인성)</a><br>
[https://ko.wikipedia.org/wiki/해시_함수](https://ko.wikipedia.org/wiki/%ED%95%B4%EC%8B%9C_%ED%95%A8%EC%88%98)<br>
[https://medium.com/@yeon22/crypto-해시-hash-란-6962be197523](https://medium.com/@yeon22/crypto-%ED%95%B4%EC%8B%9C-hash-%EB%9E%80-6962be197523)<br>
[https://www.zerocho.com/category/NodeJS/post/593a487c2ed1da0018cff95d](https://www.zerocho.com/category/NodeJS/post/593a487c2ed1da0018cff95d)