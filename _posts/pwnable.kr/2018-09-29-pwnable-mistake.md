---
layout: post
title: "Pwnable.kr - 9. mistake"
date: 2018-09-25 16:38:00
image: '/assets/img/pwn/mistake.png'
description: pwnable.kr - mistake problem solving
category: 'pwnable'
tags:
- pwnable
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr
---


# MISTAKE PROBLEM

## 0x00 INTRO
![problem](/assets/img/pwn/mistake/writeup.PNG "write_up")
<center><font size="0.5em">(Fig 1. write_up)</font></center><br>

 In write up, i guess we don't need a any fancy hacking skill. Let's think easily and read source code first.


## 0x01 ANALYZE
~~~c
#include <stdio.h>
#include <fcntl.h>

#define PW_LEN 10
#define XORKEY 1

void xor(char* s, int len){
        int i;
        for(i=0; i<len; i++){
                s[i] ^= XORKEY;
        }
}

int main(int argc, char* argv[]){

        int fd;
        if(fd=open("/home/mistake/password",O_RDONLY,0400) < 0){
                printf("can't open password %d\n", fd);
                return 0;
        }

        printf("do not bruteforce...\n");
        sleep(time(0)%20);

        char pw_buf[PW_LEN+1];
        int len;
        if(!(len=read(fd,pw_buf,PW_LEN) > 0)){
                printf("read error\n");
                close(fd);
                return 0;
        }

        char pw_buf2[PW_LEN+1];
        printf("input password : ");
        scanf("%10s", pw_buf2);

        // xor your input
        xor(pw_buf2, 10);

        if(!strncmp(pw_buf, pw_buf2, PW_LEN)){
                printf("Password OK\n");
                system("/bin/cat flag\n");
        }
        else{
                printf("Wrong Password\n");
        }

        close(fd);
        return 0;
}
~~~

In source code, If statement has priority error. And there is a sleep function of unknown intent.<br>

First, in all languages have operator priority as well as C languages, __"<"__ operator has higher operating priority than __"="__.

~~~c
if(fd=open("/home/mistake/password",O_RDONLY,0400) < 0)
~~~

In that line, __"="__ operator will be calculated after __"<"__. So that statement operate as follows

~~~c
if(fd= (open("/home/mistake/password",O_RDONLY,0400) < 0)
~~~

When open task is failed, open fuction return zero, not a negative value. So __"open(~~~) < "__ will be always fail(boolean) and fail boolean is zero on integer type. It means if statment always fail and fd is given zero, which means standard input. It make we can insert the value to __pw_buf__ during sleep task.<br>


## 0x02 SOLUTION

To execute system function, __pw_buf__ and __pw_buf2__ should be same. As i said, we can enter value we want into __pw_buf__. Also enter value we want into __pw_buf2__ through scanf function. And then the last we consider is xor function. That function just do 1 __xor__ operation for every character in the __pw_buf2__. xor operation makes 1 to 0 and 0 to 1. Finally __the solution__ is to enter the value twice, each value will be complemented. 

![problem](/assets/img/pwn/mistake/result.PNG "answer")
<center><font size="0.5em">(Fig 2. Solution)</font></center><br>