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

We need to search flag in that system. So ls command with -al option is useful for starting. -a option means show all file or directory list. basic ls command ignore entries starting with "." but using -a, ls command do not ignore entries starting with ".". -l option is long listing format option. So show the detail of file or directory. The form has eight items, which are permission, numver of links, owner, owner group, file size, last modification, file/directory name.


![problem](/assets/img/pwn/fd/ls-al.PNG)


If you look at the list, you can find __flag__. but 