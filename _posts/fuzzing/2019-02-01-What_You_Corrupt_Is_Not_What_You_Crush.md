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

  퍼징 기술, 프레임워크, 툴들은 모두 observable crash에 의존적이다. 이 말은 즉 silent crash(반응이 없는)의 경우 찾기 어렵다는 것이다. 현재 데스크탑 시스템의 경우 많은 보호 기법들을 가지고 있어 crash가 발생하면 이에 따른 반응이 나타나 crash를 탐지할 수 있다.(e.g. stack canary).

  그러나 임베디드 장치는 이러한 기술이 상대적으로 부족할 뿐만 아니라 observable crash가 발생하더라도 이를 탐지하는 것은 복잡하다. 데스크탑 시스템의 경우 반응으로 에러 메세지를 출력하는 경우가 많은 반면, 임베디드 장치의 경우 균일한 I/O 기능(이해 X)을 하지 못하는 상황으로 나타난다.
  
  이를 위해 liveness check하는 방법은 크게 두가지로 나뉜다. Activce check는 동작 중간 중간에 special request를 통해 주기적으로 상태를 점검하는 방법이고, 다른 하나는 Passive check로 장치의 state 정보를 탐지하는 것으로 보통 test input의 결과값을 살펴보거나 "visible" crash를 이용한다.

 **2. Performance and Scalability**
  
  데스크탑 시스템의 경우 동시에 여러 instance를 이용하여 같은 소프트웨어를 병렬적으로 퍼징할 수 있다(e.g. virtuablization, multi-processing). 그러나 임베디드 장치는 많은 수의 프로세스를 동시에 돌릴 수 없고, 이를 위해 많은 수의 장치를 구하는 것은 비용이 많이 들기 때문에 이러한 방식을 임베디드 장치에 적용하는 것은  어렵다(임베디드 장치에서 병렬적으로 퍼징을 적용하기 위해 에뮬레이션을 적용한 논문이 있고, 본 논문 또한 에뮬레이션 기술 활용하여 데스크탑 시스템에서 퍼징 진행).


 **3. Instrumentation**

  임베디드 장치의 퍼징은 대부분 black-box에서 진행되기 때문에 내부 정보를 획득하기 어렵다. 따라서 퍼징을 통해 접근할 수 있는 code coverage 문제와 소프트웨어가 자체적으로 가지고 있는 input 필터를 통과하거나 올바른 input으로 인식하는 valid한 input을 생성해야하는 문제를 해결하기 위한 정보를 획득하기 어렵고, 이를 위해 바이너리 계측 기술을 활용해야한다. 바이너리 계측 기술을 임베디드 장치에 적용시키는 것 도 현재 상황에서는 녹록치 않다.
 


Type-1 : 일반적인 운영체제를 탑재한 임베디드 장치. (해당 실험에서는 COTS:라우터 시스템 사용)

Type-2 : 임베디드 장치용 운영체제를 탑재한 임베디드 장치. (해당 실험에서는 IP 카메라 사용)

Type-3 : 운영체제와 어플리케이션 간의 구분 없이 하나로 구성된 펌웨어 이미지를 가진 임베디드 장치. (해당 실험에서는 개발보드에 테스트 코드를 삽입하여 사용) 

#실험 방법

각 
