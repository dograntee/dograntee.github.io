---
layout: post
title: "Pwnable.kr - 10. shellshock"
date: 2018-11-11 22:21:00
image: '/assets/img/pwn/cmd2.png'
description: pwnable.kr - cmd2 problem solving
category: 'pwnable'
tags:
- pwnable
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: pwnable.kr
---

![problem](/assets/img/pwn/cmd2/write_up.PNG "write_up")
<center><font size="0.5em">(Fig 1. Write up)</font></center><br>

 he call his father ~~~. And he got system shell. but there is some filter. Look at the source code.

~~~c
#include <string.h>

int filter(char* cmd){
        int r=0;
        r += strstr(cmd, "=")!=0;
        r += strstr(cmd, "PATH")!=0;
        r += strstr(cmd, "export")!=0;
        r += strstr(cmd, "/")!=0;
        r += strstr(cmd, "`")!=0;
        r += strstr(cmd, "flag")!=0;
        return r;
}

extern char** environ;
void delete_env(){
        char** p;
        for(p=environ; *p; p++) memset(*p, 0, strlen(*p));
}

int main(int argc, char* argv[], char** envp){
        delete_env();
        putenv("PATH=/no_command_execution_until_you_become_a_hacker");
        if(filter(argv[1])) return 0;
        printf("%s\n", argv[1]);
        system( argv[1] );
        return 0;
}
~~~

"/" is filtered. So we need other way to execute cat command. we choose read function. "read x" command get input from STDIN and store into "x" variable. And execute "x" again, we can execute any commnad bypassing filter.

![problem](/assets/img/pwn/cmd2/result.PNG "result")
<center><font size="0.5em">(Fig 2. Result)</font></center><br>