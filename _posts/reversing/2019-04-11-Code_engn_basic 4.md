---
layout: post
title: "Code engn"
date: 2019-04-11 13:42:00
image: '/assets/img/reversing/intro.PNG'
description: Codeengn basic challenge
category: 'reversing'
tags:
- reversing
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: A summary of my study
---
## 00x 시작

디버거를 탐지하는 기능의 함수를 찾아야한다.

## 01x 접근법

![problem](/assets/img/codeengn/basic2/intro.JPG "keyboard micro controller")
<center><font size="0.5em">(Fig 1. 접근법)</font></center><br>

디버거에서 해당 프로그램을 실행시키면 다음과 같은 함수 이후에 디버거 탐지 메세지가 출력된다. 이러한 방식으로 안으로 따라가면 탐지하는 함수를 찾을 수 있을 것 으로 보인다.

## 02x 해결방안

![problem](/assets/img/codeengn/basic2/answer.JPG "정답")
<center><font size="0.5em">(Fig 2. 정답)</font></center><br>

실제로 따라가면 굉장히 깊숙하게 구현되어 있어, 많은 핸들러를 타고가야 해당 함수를 발견할 수 있다. 따라서 ollydbg 기능 중 호출된 함수를 찾아주는 기능을 활용하면 vbaStrCmp 함수를 찾을 수 있다.




