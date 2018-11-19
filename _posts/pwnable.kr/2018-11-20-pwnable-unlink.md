---
layout: post
title: "Pwnable.kr - 19. unlink"
date: 2018-11-12 23:32:00
image: '/assets/img/pwn/unlink.png'
description: pwnable.kr - unlink problem solving
category: 'pwnable'
tags:
- pwnable
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr
---

![problem](/assets/img/pwn/unlink/write.PNG "write up")
<center><font size="0.5em">(Fig 1. write up)</font></center><br>

The unlink is one of the famous vulnerability in heap area. Most of the case is happen at double free bug. Head area has chunks which form linked list strucutre. In "free" process, unlink function disconnect the chunk that want to be deleteed and concatenates them back and forth. At this point, if the heap overflow is occured, the attacker can write the desired data in the desired address in the concatenating task.



