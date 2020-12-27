---
title: "Github 레포지토리에서 폴더를 열 수 없었던 이유"
layout: post
date: 2020-12-27 16:00
tag:
    - Git
hidden: false
star: false
category: blog
author: Joie-Kim
description: 배운 것을 기록하는 습관! ✍️
---

최근에 ReactJS 책을 사서 공부를 하고 있다.<br>
프론트엔드에 대해서도 알고 있어야 앞으로 다른 팀원과 프로젝트를 할 때 수월할 거 같아서 시작했다.<br>

공부하는 걸 기록으로 남기면(~~깃헙에 잔디를 심으면🌱~~) 동기부여가 될 거 같아, 깃헙에 레포지토리를 하나 만들었다.<br>
아래처럼 ReactJS 레포지토리에 그 동안 실습하며 만들었던 리액트 프로젝트들을 한 곳에 모을 생각이었다.

![image](/assets/201227/sample.png)
<figcaption class="caption">RN을 공부하면서 실습한 것들을 올린 레포지토리</figcaption>

<br>

아래 명령어로 원격 레포지토리와 로컬 레포지토리를 연결하고, push를 날렸다.
```bash
git init
git remote add origin https://github.com/Joie-Kim/ReactJS
git add .
git commit -m " ... blabla ..."
git push origin master
```
<br>

---

## 이게 무슨 일이야? 폴더를 열 수 없잖아..! 🤦🏻‍♀️
이런 상황이었다.. 리액트 프로젝트 폴더를 열 수 없었다.
![image](/assets/201227/issue.png)
<figcaption class="caption">재현하기 위해 만든 Test 레포지토리</figcaption>

<br>

## Why can't I open it..? 🤷🏻‍♀️
구글링을 해 보니, 스택오버플로우에 다음과 같은 내용이 있었다.
```
I solved the problem by deleting .git folder
inside subfolders (Hidden files and folders).

You should have only one .git in the root folder.
```
즉, 루트 폴더에는 `.git` 폴더가 하나 있어야 하며, 하위 폴더 내의 `.git` 폴더를 삭제하여 문제를 해결했다는 얘기였다.<br>

`create react-app`을 하면 기본적으로 Git이 설정되기 때문에 `ReactJS` 하위에 있는 `hello-react`에도 .git이 존재해 생긴 문제였다.

<br>

## 문제를 해결해 보자!! 💁🏻‍♀️
finder에서는 .git을 찾을 수 없었다.
![image](/assets/201227/finder.png)

terminal에서 확인 해보니, 찾을 수 있었다.
![image](/assets/201227/terminal.png)

.git을 삭제하고, 없어진 걸 확인했다.
![image](/assets/201227/removed.png)

다시 github에 올리기 위해 우선 로컬 레포지토리 내부에 있던 hello-react를 다른 곳으로 이동했다. (원격 레포지토리에서 삭제하기 위해)
![image](/assets/201227/moved.png)

원격 레포지토리에 `push` 했고, 삭제된 것을 확인했다.
![image](/assets/201227/deleted.png)

hello-react를 원래 있던 로컬 레포지토리로 다시 가져오고, 원격 레포지토리에 `push` 하면 제대로 올라간 것을 확인할 수 있다.
![image](/assets/201227/added.png)

<br>

이번 일로 루트 폴더에는 `.git` 폴더가 하나 있어야 한다는 걸 알게 되었다.<br>
이슈도 해결 했으니, 앞으로 열심히 잔디를 심어보자 ~~!! 🌳
![image](/assets/201227/solved.png)
<figcaption class="caption">완성된 ReactJS 레포지토리!! 👏</figcaption>

<br>

---

### 📚 참고

[StckOverFlow: Why can I not open my folder in GitHub?](https://stackoverflow.com/questions/28384464/why-can-i-not-open-my-folder-in-github)
