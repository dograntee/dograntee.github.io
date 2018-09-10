---
layout: post
title: "Pwnable.kr - 3. bof"
date: 2018-08-23 12:00:00
image: '/assets/img/pwn/bof.png'
description: pwnable.kr - bof problem solving
category: 'pwnable'
tags:
- pwnable
- file descriptor
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr - bof problem solving, it is a simple summary that i solve the bof problem to study pwnable 
---


## BOF problem

bof stands for "buffer overflow". So this problem may be using buffer overflow vulnerability. 

What is buffer overflow? buffer overflow can be divided into two big section. One is BOF in stack section, and the other is heap section overflow.
(Stack and Heap is a field of running process structure in memory)
![problem](/assets/img/pwn/bof/process_structure.PNG "process structure")


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