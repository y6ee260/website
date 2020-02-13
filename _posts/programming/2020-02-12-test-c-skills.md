---
layout: post
title: Test Your C Skills
category: programming
comments: true
google_adsense: true
excerpt: C objective type questions to test your C skills. Each question has 4 options. Chose the correct one. If you find something new, run the programs in the system and find the concept behind it.
keywords: Test your C skills, test your C skills online, test your C skills by Yashwant Kanetkar alternative, C objective questions, C objective questions online test, C multiple choice questions, C multiple choice questions geeks for geeks alternative, C multiple choice questions indiabix alternative, C assessment questions.
image: /assets/img/test-c-skills.png
date: 2020-02-12 23:04:36 +5:30
---
## Introduction
All the following programs are run and verified in the ***Linux based operating system*** with ***GCC*** compiler. Each question has 4 options. Chose the correct one. If you find something new, change and run the programs in the system and find the concept behind it.

{% include image.html url="test-c-skills.png" description="Test Your C Skills" %}
## Question 1
**Guess the output**
```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(void)
{
    const char name[32], *name_ptr = name;

    memset(name, 0, sizeof(name));
    snprintf(name, sizeof(name), "nayab.xyz");
    puts(name);
    free(name_ptr);
    return 0;
}
```
**Options**
```
a) nayab.xyz  
b) Segmentation fault  
c) Aborted (core dumped)  
d) None of the above  
```
___

## Question 2
**Guess the output**

Assuming the address of `a` is `0xabcdef` and the address of `b` is `0xfedcba`

```C
#include <stdio.h>

int main(void)
{
    unsigned long a = 10;
    unsigned long *b = &a;
    unsigned long **c = &b;

    printf("a value : %lu, ",
        a);
    printf("*b value : %lu, *c value : %p\n", *b, *c);

    return 0;
}
```
**Options**
```
a) a value : 10, *b value : 10, *c value : 0xabcdef
b) a value : 10, *b value : 10, *c value : 0xfedcba
c) Compilation error
d) Segmentation fault
```
___
## Question 3
**Guess the ouput.**
```C
#include <stdio.h>

int main(void)
{
    printf("\z\n");
    return 0;
}
```
**Options**
```
a) Compilation error
b) z
c) Garbage
d) \z
```
___
## Question 4
**Guess the ouput.**

```C
#include <stdio.h>

#define SQUARE(x) x * x;

int main(void)
{
    printf("%d\n", SQUARE(2 + 1));
    return 0;
}
```
**Options**
```
a) 9
b) 5
c) 6
d) Program won't compile
```
___
## Question 5
**Guess the ouput.**
```C
#include <stdio.h>

int main(void)
{
    int data[5] = {10, 20, 30};
    int i;

    for (i = 0; i < sizeof(data)/sizeof(data[0]); i++) {
        printf("%d ", *data);
        data++;
    }

    return 0;
}
```
**Options**
```
a) 10 20 30
b) 10 20 30 0 0
c) Compile time error
d) Segmentation fault
```
___
## Question 6
**Guess the output**
```C
#include<stdio.h>

void function(int *p)
{
    int c=10;
    p=&c;
}

int main()
{
    int a=20;
    int *p=&a;

    function(p);
    printf("%d\n",*p);

    return 0;
}
```
**Options**
```
a) Garbage value
b) 10
c) 20
d) Segmentation fault
```
___
## Question 7
**Guess the output**
```C
#include <stdio.h>

int a;

int main()
{
   a+=5;

   if(a/2.0 == 2.5)
       printf("if");
   else if(a/2 == 2)
       printf("else if");
   else
       printf("else");

   return 0;
}
```
**Options**
```
a) if
b) else if
c) else
d) Prints nothing
```
___
## Question 8
**Guess the ouput.**
```C
#include<stdio.h>

int main(void)
{
 char *ptr = "nayab.xyz";
 void *vptr;

 vptr = &ptr;

 /* Print "nayab.xyz" using vptr */
 return 0;
}
```

**Print <i>nayab.xyz</i>  using vptr**
```
a) printf("%s" , *(char **)vptr);
b) printf("%s" , *(vptr);
c) Both
d) Compile time error (or) Segmentation fault
```
___
## Question 9
**Guess the output**
```C
#include <stdio.h>

int aa(int a)
{
   return --a;
}

int main()
{
   int a = 'a'-87;
   for ( ; a; )
       printf("%d ",aa(--a));
   return 0;
}
```
**Options**
```
a) 8 6 4 2 0
b) 10 8 6 4 2 0
c) 8 7 6 5 4 3 2 1 0 -1
d) None of the above
```
___
## Question 10
**Guess the output**
```C
#include<stdio.h>

int main(void)
{
    int arr[4] = {23, 11, 56, 29};
    int *p, *q;

    q = arr / 2;
    p = q * 2;
    printf("%d %d", *(p + 1), *(p + 3));

    return 0;
}
```
**Options**
```
a) 11 29
b) 23 56
c) Compile time error
d) Segmentation fault
```
___
## Question 11
**Guess the output**
```C
#include <stdio.h>
#include <stdbool.h>

bool func (void) {
     return -2;
}

int main()
{
    if (func() == -2) {
       printf("if");
    } else {
       printf("else");
    }
}
```
**Options**
```
a) if
b) else
c) Prints nothing
d) Compile time error
```
___
## Question 12
**Guess the output**
```C
#include <stdio.h>

int var = 20;

int main()
{
    int var = var;
    printf("%d ", var);

    return 0;
}
```
**Options**
```
a) Undefined behavior
b) 0
c) 20
d) Compile time error
```
___
## Question 13
**Guess the output**
```C
#include <stdio.h>

int main()
{
    printf("%c ", 4["nayab.xyz"]);
    return 0;
}
```
**Options**
```
a) Compile time error
b) b.xyz
c) b
d) nayab.xyz
```
___
## Question 14
**Guess the output**
```C
#include <stdio.h>

union uni1 {
    unsigned int a;
    unsigned int d;
    unsigned char b;
    unsigned char c;
} un;

int main()
{
    un.a = 498;
    un.d = un.d & 0xceba;

    printf("un.a : %d, un.c : %d\n",un.a, un.c);
}
```
**Options**
```
a) un.a : 255, un.c : 178
b) un.a : 498, un.c : 178
c) un.a : 178, un.c : 178
d) un.a : 498, un.c : 255
```