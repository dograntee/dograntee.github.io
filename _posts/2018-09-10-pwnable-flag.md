---
layout: post
title: "Pwnable.kr - 4. flag"
date: 2018-09-10 16:38:00
image: '/assets/img/pwn/flag.png'
description: pwnable.kr - flag problem solving
category: 'pwnable'
tags:
- pwnable
- executable packing
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr - flag problem solving, it is a simple summary that i solve the flag problem to study pwnable 
---

#FLAG PROBLEM

##00xIntro

![problem](/assets/img/pwn/flag/writeup.PNG "dead code")
<center><font size="0.5em">(Fig 1. write up)</font></center><br>

The problem says __"Papa brought me a packed present! let's open it"__. It is a good hint for solving problem. Look at the flag assembly code.<br>

![problem](/assets/img/pwn/flag/broken.PNG "dead code")
<center><font size="0.5em">(Fig 2. dead code)</font></center><br>

But, there is some error. So IDA can't analyze the flag binary as well. So, for understanding problem, i execute flag binary. 

![problem](/assets/img/pwn/flag/execute.PNG "execute binary")
<center><font size="0.5em">(Fig 3. execute binary)</font></center><br>

The output is __"I will malloc() and strcpy the flag there. take it."__. So i think i can find that string, and next track the string reference. But in flag binary, i can't find that string but there is some string __"//upx.sf.net$\n"__. UPX is a packing way for executable. I unpack flag binary with UPX on Ubuntu18.

![problem](/assets/img/pwn/flag/string.PNG "Strings ouput")
<center><font size="0.5em">(Fig 4. Strings output)</font></center><br>

Then i can find __"I will malloc() and strcpy the flag there. take it."__. And i tracked the string reference and found the main function. There is assembly code using flag string. So i can find flag string 

![problem](/assets/img/pwn/flag/flag.PNG "FLAG")
<center><font size="0.5em">(Fig 5. Strings in flag)</font></center><br>