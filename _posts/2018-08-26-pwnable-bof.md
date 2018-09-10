---
layout: post
title: "Pwnable.kr - 3. bof"
date: 2018-08-23 12:00:00
image: '/assets/img/pwn/bof.png'
description: pwnable.kr - bof problem solving
category: 'pwnable'
tags:
- pwnable
- stack overflow
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr - bof problem solving, it is a simple summary that i solve the bof problem to study pwnable 
---


# BOF problem

## 00xIntro

bof stands for "buffer overflow". So this problem may be using buffer overflow vulnerability. 

What is buffer overflow? buffer overflow can be divided into two big section. One is BOF in *stack section, and the other is *heap section overflow. This problem is about stack BOF, describing the reason later
(* Stack and Heap is a field of running process structure in memory)
![problem](/assets/img/pwn/bof/process_structure.PNG "process structure")

## 01xAnalyze

First, look at the source code bof.c

~~~c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
void func(int key){
	char overflowme[32];
	printf("overflow me : ");
	gets(overflowme);	// smash me!
	if(key == 0xcafebabe){
		system("/bin/sh");
	}
	else{
		printf("Nah..\n");
	}
}
int main(int argc, char* argv[]){
	func(0xdeadbeef);
	return 0;
}
~~~
 
In the source code, there is char array named overflowme. In guessing, the namimg of array is related to solution of this problem. And next, line 7, __gets__ function receive user input and store at char array __"overflowme"__. but __gets__ function have weak point can be receive too much amount of data rather than allocated data size. So we can input data bigger that 32 byte and more, it can be overflowed.

What we can do by overflowing __"overflowme"__. For understanding this wokring, we need to know that local variable is located at stack field and how composed stack frame is.

In the stack section, there is some subsection. See the picture below.

![problem](/assets/img/pwn/bof/stack_frame.PNG "stack frame")

Detailed explanation is folded, we can notice that local variables are located at stack section and stored under __Save old frame pointer field(RBP)__ and __Return address to the caller__, __Function arguments__. So if we enter over 32 byte, input data is stored over local variable storage section and modify __"Function Arguments filed"__. 

## 02xSolution

We know that we enter proper input, we can modify function argument. So we modify __"key"__  

















### FFxReference
 - The Linux Programming Interface by Michael Kerrisk
 - [URL](https://loonytek.com/2015/04/28/call-stack-internals-part-1/) by siddharthteotia