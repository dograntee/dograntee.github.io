---
layout: post
title: "Pwnable.kr - 2. collision"
date: 2018-08-22 10:34:00
image: '/assets/img/pwn/collision.png'
description: pwnable.kr - collision problem solving
category: 'pwnable'
tags:
- pwnable
- Litte math
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr - collision problem solving, it is a simple summary that i solve the collsion problem to study pwnable 
---

## Simple math problem

Quickly, look at the source code.

~~~c
#include <stdio.h>
#include <string.h>
unsigned long hashcode = 0x21DD09EC;
unsigned long check_password(const char* p){
        int* ip = (int*)p;
        int i;
        int res=0;
        for(i=0; i<5; i++){
                res += ip[i];
        }
        return res;
}

int main(int argc, char* argv[]){
        if(argc<2){
                printf("usage : %s [passcode]\n", argv[0]);
                return 0;
        }
        if(strlen(argv[1]) != 20){
                printf("passcode length should be 20 bytes\n");
                return 0;
        }

        if(hashcode == check_password( argv[1] )){
                system("/bin/cat flag");
                return 0;
        }
        else
                printf("wrong passcode.\n");
        return 0;
}
~~~

If check_password function return value 0x21DD09EC, system call execute and show flag.

check_password function add 5 times to __res__ value, increasing the address value by 4 bytes(becaues ip point has int data type).

So we list five variables in 4 bytes and sum of variables should be 0x21DD09EC.

If we divide 0x21DD09EC by five is 0x6C5CEC8. The rest is 4, so adding 0x6C5CEC8 4 times and 0x6C5CECC(0x6C5CEC8 + 4) once and we get 0x21DD09EC.

And We pass the list to the col. We can solve the problem.


![problem](/assets/img/pwn/col/answer.PNG)