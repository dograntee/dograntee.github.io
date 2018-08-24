---
layout: post
title: "Pwnable.kr - 1. fd"
date: 2018-08-20 12:00:00
image: '/assets/img/pwn/fd.png'
description: pwnable.kr - fd problem solving
category: 'pwnable'
tags:
- pwnable
- file descriptor
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr - fd problem solving, it is a simple summary that i solve the fd problem to study pwnable 
---


## File Descriptor problem

This is my first post. So it may be a poor article, but please understand.

![problem](/assets/img/pwn/fd/problem.PNG)


To the point, first we can see the ssh command, host name, port number and password. I use Window bash to connect. Once connected, you will see the following screen :)


![problem](/assets/img/pwn/fd/intro.PNG)

We need to search flag in that system. So ls command with -al option is useful for starting. -a option means show all file or directory list. basic ls command ignore entries starting with "." but using -a, ls command do not ignore entries starting with ".". -l option is long listing format option. The form has eight items, which are permission, numver of links, owner, owner group, file size, last modification, file/directory name.


![problem](/assets/img/pwn/fd/ls-al.PNG)


If you look at the list, you can find __flag__. but for reading flag, you are fd_pwn or root group. But not both. So you need to find way to read flag. Look at the fd, its permission has SetUID. SetUID can acquire root privileges during excution, as will be discussed further later. Then let's take a look at the source code "fd.c".

~~~c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
        if(argc<2){
                printf("pass argv[1] a number\n");
                return 0;
        }
        int fd = atoi( argv[1] ) - 0x1234;
        int len = 0;
        len = read(fd, buf, 32);
        if(!strcmp("LETMEWIN\n", buf)){
                printf("good job :)\n");
                system("/bin/cat flag");
                exit(0);
        }
        printf("learn about Linux file IO\n");
        return 0;

}
~~~



If __buf__ value is "LETMEWIN\n", system function works and show flag by execute '/bin/cat'. So we can use file descriptor. because buf value is read value from fd. So if fd is STDIN, we enter value for __buf__. We pass the argument to fd program. And file descriptor 0 is STDIN. If we pass 0x1234, fd value is zero. Then we can enter value that execute system function. 