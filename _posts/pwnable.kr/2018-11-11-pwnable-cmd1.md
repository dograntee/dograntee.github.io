---
layout: post
title: "Pwnable.kr - 14. cmd1"
date: 2018-11-11 22:21:00
image: '/assets/img/pwn/cmd1.png'
description: pwnable.kr - cmd1 problem solving
category: 'pwnable'
tags:
- pwnable
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr
---

![problem](/assets/img/pwn/cmd1/startup.PNG "startup")
<center><font size="0.5em">(Fig 1. Write up)</font></center><br>

Mommy! what is PATH environment in Linux?

Write up is always helpful. Look at the follow c code.

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

This is a program that executes the command entered by the user. However, "flag", "sh", and "tmp" strings are filtered. This can be bypassed through using wild card.

The wild card is special charater that could be any characters. So we can pass "cat *" to cmd1. But nothing is happend.

Because, There is other trick about environment variable. Look at the "putenv". That function put environment variable into program. So "PATH" environment variable changes to "/thankyouverymuch". All Program use "PATH" to address frequently used path(default is "root"). When we execute "cat" command, in bash, "PATH" + command is done.

When we pass "cat *" as argument to cmd1, "/thankyouverymuch/cat *" is really done. So we need to pass absoulte address path.


![problem](/assets/img/pwn/cmd1/result.PNG "result")
<center><font size="0.5em">(Fig 2. Result)</font></center><br>

mommy now I get what PATH environment is for :)