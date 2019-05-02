---
layout: post
title: "Code engn"
date: 2019-04-11 14:42:00
image: '/assets/img/reversing/intro.PNG'
description: Codeengn basic challenge
category: 'reversing'
tags:
- reversing
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: A summary of my study
---
## 00x 시작

프로그램의 등록키를 찾아야한다.

## 01x 접근법

비교를 위해 문자열이 박혀 있을 확률이 높다. 따라서 바이너리 내부의 스트링 중 등록키로 추정되는 것을 찾는다.
## 02x 해결방안

![problem](/assets/img/codeengn/basic5/캡쳐.JPG "정답")
<center><font size="0.5em">(Fig 1. 정답)</font></center><br>

ollydbg 기능을 이용하여 쉽게 찾을 수 있었다.



