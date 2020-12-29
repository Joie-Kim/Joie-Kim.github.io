---
title: "얕은 복사(shallow copy) VS 깊은 복사(deep copy)"
layout: post
date: 2020-12-29 16:00
tag:
    - 용어 정리
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

ReactJS를 공부하던 중 **불변성**을 지키기 위해 **전개 연산자(...문법)**을 사용하는 것을 알게 되었으나, 전개 연산자가 **얕은 복사**를 한다는 점이 이해되지 않았다. (얕은 복사가 뭐지? 🤨)<br>

그래서 객체를 복사하는 방식에 대해 공부했다. (~~모르는 건 알아야 직성이 풀린다.~~) <br>

```
😇 설명을 잘 해둔 블로그 글을 발견해서 해당 글을 참고 해 작성했다.

해당 블로그에서 예시 설명이 파이썬으로 되어 있었고, 본인도 알고리즘 공부를 파이썬으로 하고 있기에
이번 블로그 글의 예시 코드는 모두 파이썬으로 작성했다.
```

---

## 단순 객체 복사

```python
a = [1, 2, 3, 4]
b = a
print(b)    # [1, 2, 3, 4]

b[0] = 100
print(a)    # [100, 2, 3, 4]
print(b)    # [100, 2, 3, 4]
```

리스트를 만들어 a에 할당하면 a는 리스트 객체의 주소를 바라보는 변수가 된다.<br>
그리고 나서 a를 b에 할당 해주면 b와 a는 아래 그림처럼 같은 객체의 주소를 바라보게 된다.<br>
그렇기 때문에 당연히 a 또는 b를 수정하면 a와 b의 값이 모두 수정된다.
![image](/assets/201229/simple_copy.jpeg)

다만, 리스트와 같이 변경가능(mutable) 객체일 때만 허용되며, 숫자나 문자열과 같이 불변의(immutable) 객체일 경우에는 아래 경우처럼 수정되지 않는다.<br>
불변의 객체란 값이 바뀌지 않는 객체이기 때문에 참조 변수를 수정하면 같은 주소의 값(value)이 바뀌는 것이 아니라, 그 변수에 새로운 객체가 할당되는 것을 뜻한다.
```python
a = 'apple'
b = a

b = 'banana'

print(a)    # apple
print(b)    # banana
```

<br>

## 얕은 복사

```python
import copy

a = ['apple', [1, 2, 3, 4]]

# mutable
b = copy.copy(a)    # shallow copy
print(b)            # ['apple', [1, 2, 3, 4]]
b[1].append(5)
print(a)            # ['apple', [1, 2, 3, 4, 5]]
print(b)            # ['apple', [1, 2, 3, 4, 5]]

# immutable
c = copy.copy(a)    # shallow copy
c[0] = 'cat'
print(a)            # ['apple', [1, 2, 3, 4, 5]]
print(c)            # ['cat', [1, 2, 3, 4, 5]]
```

단순 복사와의 차이점은 복합 객체(여기서는 리스트)를 별도로 생성하지만, 그 안에 들어가는 내용은 원래와 같은 객체라는 것이다.<br>

a와 b는 서로 다른 리스트이지만(다른 주소를 바라보고 있지만), 라스트 내부의 객체는 동일하기 때문에 mutable한 객체인 `[1, 2, 3, 4]`가 함께 수정되었다.<br>
a와 c도 서로 다른 리스트이고 리스트 내부의 객체가 동일하지만, immutable한 객체를 수정했기 때문에 위와 같은 결과를 얻었다.

![image](/assets/201229/shallow_copy.jpeg)
<figcaption class="caption">이해를 돕기 위한 그림일 뿐! 실제로 이렇다는 건 아님 🙅🏻‍♀️</figcaption>

<br>

## 깊은 복사

```python
import copy

a = ['apple', [1, 2, 3, 4]]
b = copy.deepcopy(a)        # deep copy
print(b)                    # ['apple', [1, 2, 3, 4]]

b[0] = 'banana'
b[1].append(5)

print(a)                    # ['apple', [1, 2, 3, 4]]
print(b)                    # ['banana', [1, 2, 3, 4, 5]]
```

mutable한 내부 객체(여기서는 리스트)도 변하지 않도록 하기 위해서는 얕은 복사가 아닌 깊은 복사를 해야 한다.<br>

깊은 복사는 복합 객체를 새롭게 생성하면서 내부의 객체들도 모두 새롭게 생성하게 된다.<br>
그래서 깊은 복사를 하게 되면, 처음에 만들었던 객체와 복사된 객체가 전혀 달라져 어느 한쪽을 수정한다고 해서 다른 한쪽이 영향을 받는 경우는 없다.

<br>

---

### 📚 참고

[얕은 복사(shallow copy) vs 깊은 복사(deep copy)](https://blueshw.github.io/2016/01/20/shallow-copy-deep-copy/)
