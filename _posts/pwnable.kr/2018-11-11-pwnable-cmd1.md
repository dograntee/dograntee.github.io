---
layout: post
title: "Pwnable.kr - 10. shellshock"
date: 2018-09-30 21:11:00
image: '/assets/img/pwn/cmd1.png'
description: pwnable.kr - shellshock problem solving
category: 'pwnable'
tags:
- pwnable
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr
---

![problem](/assets/img/pwn/cmd1/startup.PNG "startup")
<center><font size="0.5em">(Fig 1. Start Up)</font></center><br>

~~~c
#include <stdio.h>
#include <string.h>

int filter(char* cmd){
        int r=0;
        r += strstr(cmd, "flag")!=0;
        r += strstr(cmd, "sh")!=0;
        r += strstr(cmd, "tmp")!=0;
        return r;
}
int main(int argc, char* argv[], char** envp){
        putenv("PATH=/thankyouverymuch");
        if(filter(argv[1])) return 0;
        system( argv[1] );
        return 0;
}
~~~