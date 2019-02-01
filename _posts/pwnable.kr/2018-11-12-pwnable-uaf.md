---
layout: post
title: "Pwnable.kr - 16. uaf"
date: 2018-11-12 23:32:00
image: '/assets/img/pwn/uaf.png'
description: pwnable.kr - uaf problem solving
category: 'pwnable'
tags:
- pwnable
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr
---


~~~cpp
#include <fcntl.h>
#include <iostream>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
using namespace std;

class Human{
private:
        virtual void give_shell(){
                system("/bin/sh");
        }
protected:
        int age;
        string name;
public:
        virtual void introduce(){
                cout << "My name is " << name << endl;
                cout << "I am " << age << " years old" << endl;
        }
};

class Man: public Human{
public:
        Man(string name, int age){
                this->name = name;
                this->age = age;
        }
        virtual void introduce(){
                Human::introduce();
                cout << "I am a nice guy!" << endl;
        }
};

class Woman: public Human{
public:
        Woman(string name, int age){
                this->name = name;
                this->age = age;
        }
        virtual void introduce(){
                Human::introduce();
                cout << "I am a cute girl!" << endl;
        }
};

int main(int argc, char* argv[]){
        Human* m = new Man("Jack", 25);
        Human* w = new Woman("Jill", 21);

        size_t len;
        char* data;
        unsigned int op;
        while(1){
                cout << "1. use\n2. after\n3. free\n";
                cin >> op;

                switch(op){
                        case 1:
                                m->introduce();
                                w->introduce();
                                break;
                        case 2:
                                len = atoi(argv[1]);
                                data = new char[len];
                                read(open(argv[2], O_RDONLY), data, len);
                                cout << "your data is allocated" << endl;
                                break;
                        case 3:
                                delete m;
                                delete w;
                                break;
                        default:
                                break;
                }
        }

        return 0;
}
~~~

Use After Free is one of the famous vulnerability. It occurs when using memory after free. In this problem, it is not a just simple uaf problem. The source code was written in c++. So we need to know how to allocate and delete object in c++.

First, I focused on the position of the object pointer variable "m" and "w" in the stack. 

![problem](/assets/img/pwn/uaf/p_man.PNG "p_man")
<center><font size="0.5em">(Fig 1. "m" pointer)</font></center><br>

Look at the main+68("mov edx, 0x19"), 0x19 is 25 in decimal. Man object has "25" variable. So i guess that main+65~88 is creating object part. In the result, $rbp-0x38 is the pointer of Man object.

![problem](/assets/img/pwn/uaf/in_man.PNG "in_man")
<center><font size="0.5em">(Fig 2. "m" in heap memory)</font></center><br>

I guess object structure has The address that points to the prototype of the object in first 4 or 8bytes. and from the next 8 bytes, i expects the value to be owned by the object. Look at the calling introduction() part. 

![problem](/assets/img/pwn/uaf/introduction.PNG "calling introduction() part")
<center><font size="0.5em">(Fig 3. calling introduction() part)</font></center><br>

In the process to call instruction, first get the value pointed to by $rbp-0x38, which value is pointing again to object prototype. Add 0x8 to the value and call the function at that address. 

![problem](/assets/img/pwn/uaf/proto.PNG "Proto type of Man")
<center><font size="0.5em">(Fig 4. Proto type of Man)</font></center><br>

Let's look at the prototype of the object for a while. Calling the object's prototype address plus 8 is equivalent to calling 0x004012d2 which is actually the address of introduction(). it means that when calling a function of an object, you can see that it calls the function through the address of the object prototype owned by itself. <br>
In that case, the address before introduction () can be guessed with give_shell (), and give_shell () can be executed instead of introduction () if the value of the object prototype address owned by itself minus 0x8. If you change the value through UAF it will be possible to exploit.

![problem](/assets/img/pwn/uaf/result.PNG "result")
<center><font size="0.5em">(Fig 5. Result)</font></center><br>


