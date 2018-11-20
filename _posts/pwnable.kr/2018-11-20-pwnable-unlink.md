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


~~~c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct tagOBJ{
        struct tagOBJ* fd;
        struct tagOBJ* bk;
        char buf[8];
}OBJ;

void shell(){
        system("/bin/sh");
}

void unlink(OBJ* P){
        OBJ* BK;
        OBJ* FD;
        BK=P->bk;
        FD=P->fd;
        FD->bk=BK;
        BK->fd=FD;
}
int main(int argc, char* argv[]){
        malloc(1024);
        OBJ* A = (OBJ*)malloc(sizeof(OBJ));
        OBJ* B = (OBJ*)malloc(sizeof(OBJ));
        OBJ* C = (OBJ*)malloc(sizeof(OBJ));

        // double linked list: A <-> B <-> C
        A->fd = B;
        B->bk = A;
        B->fd = C;
        C->bk = B;

        printf("here is stack address leak: %p\n", &A);
        printf("here is heap address leak: %p\n", A);
        printf("now that you have leaks, get shell!\n");
        // heap overflow!
        gets(A->buf);

        // exploit this unlink!
        unlink(B);
        return 0;
}
~~~

First, we found out the stack address of A, B, C to check the heap area.

![problem](/assets/img/pwn/unlink/whereis.PNG "A,B,C stack address")
<center><font size="0.5em">(Fig 2. A,B,C stack address)</font></center><br>

After malloc, ebp-0x14, ebp-0xc and ebp-0x10 get heap area address

![problem](/assets/img/pwn/unlink/address.PNG "A,B,C heap address")
<center><font size="0.5em">(Fig 3. A,B,C heap address)</font></center><br>
![problem](/assets/img/pwn/unlink/heap_area.PNG "Heap area")
<center><font size="0.5em">(Fig 4. Heap area)</font></center><br>

We know that if we overflow the A, we can change the B's fd and bk.  


