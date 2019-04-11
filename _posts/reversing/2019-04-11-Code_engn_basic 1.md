---
layout: post
title: "Code engn"
date: 2019-03-04 16:42:00
image: '/assets/img/reversing/intro.PNG'
description: Codeengn basic challenge
category: 'reversing'
tags:
- reversing
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: A summary of my study
---
## 00x 시작

비주얼 베이직으로 짜여진 프로그램에서 스트링 비교함수를 찾아야한다.

## 01x 접근법

![problem](/assets/img/codeengn/basic1/intro.JPG "keyboard micro controller")
<center><font size="0.5em">(Fig 1. 문제)</font></center><br>

해당 프로그램을 실행하면 다음과 같이 특정 키를 비교한다. 그렇다면 쭉 따라가서 실행되는 strcmp 관련 함수를 찾아보자.

## 02x 해결방안

![problem](/assets/img/codeengn/basic1/intro.JPG "keyboard micro controller")
<center><font size="0.5em">(Fig 2. 정답)</font></center><br>

실제로 따라가면 굉장히 깊숙하게 구현되어 있어, 많은 핸들러를 타고가야 해당 함수를 발견할 수 있다. 따라서 ollydbg 기능 중 호출된 함수를 찾아주는 기능을 활용하면 vbaStrCmp 함수를 찾을 수 있다.




