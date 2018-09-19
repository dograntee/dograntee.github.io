---
layout: post
title: "Pwnable.kr - 5. Passcode"
date: 2018-09-10 16:38:00
image: '/assets/img/pwn/passcode.png'
description: pwnable.kr - passcode problem solving
category: 'pwnable'
tags:
- pwnable
- GOT overwriting
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr - passcode problem solving, it is a simple summary that i solve the passcode problem to study pwnable 
---

~~~c
#include <stdio.h>
#include <stdlib.h>

void login(){
        int passcode1;
        int passcode2;

        printf("enter passcode1 : ");
        scanf("%d", passcode1);
        fflush(stdin);

        // ha! mommy told me that 32bit is vulnerable to bruteforcing :)
        printf("enter passcode2 : ");
        scanf("%d", passcode2);

        printf("checking...\n");
        if(passcode1==338150 && passcode2==13371337){
                printf("Login OK!\n");
                system("/bin/cat flag");
        }
        else{
                printf("Login Failed!\n");
                exit(0);
        }
}

void welcome(){
        char name[100];
        printf("enter you name : ");
        scanf("%100s", name);
        printf("Welcome %s!\n", name);
}

int main(){
        printf("Toddler's Secure Login System 1.0 beta.\n");

        welcome();
        login();

        // something after login...
        printf("Now I can safely trust you that you have credential :)\n");
        return 0;
}
~~~