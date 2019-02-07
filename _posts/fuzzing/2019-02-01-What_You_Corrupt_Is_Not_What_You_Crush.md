---
layout: post
title: "What You Corrupt Is Not What You Crash: Challenges in Fuzzing Embedded Devices"
date: 2019-02-01 16:38:00
image: '/assets/img/fuzzing/whatis/intro.png'
description: Embedded device fuzzing
category: 'fuzzing'
tags:
- fuzzing
- paper review
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: A summary of my study
---

## 00xIntro

 위 논문에서는 현재 임베디드 장치에 대한 퍼징 기술에서 메모리 누수를 발생시키더라도 이를 탐지하는 기술이 부족한 상황을 파악하고, 이를 해결하기 위해 여러 메모리 누수 탐지 기법들을 적용하여 기존에 탐지하지 못했던 임베디드 장치에서의 메모리 누수를 탐지하였다.

## 01x임베디드 장치에서 퍼징의 어려움

 **1. Fault Detection**
  
 


Type-1 : 일반적인 운영체제를 탑재한 임베디드 장치. (해당 실험에서는 COTS:라우터 시스템 사용)

Type-2 : 임베디드 장치용 운영체제를 탑재한 임베디드 장치. (해당 실험에서는 IP 카메라 사용)

Type-3 : 운영체제와 어플리케이션 간의 구분 없이 하나로 구성된 펌웨어 이미지를 가진 임베디드 장치. (해당 실험에서는 개발보드에 테스트 코드를 삽입하여 사용) 

#실험 방법

각 
