---
title: re_安洵杯2019_game
date: 2020-10-11 22:52:15
tags: re
cover: /imgs/illust_79816974_20200307_204222.png
coverWidth: 1812
coverHeight: 1082
---
一道看起来难以下手，但摸清套路后其实还挺明了的逆向题
<!--more-->

## 1

64位ELF文件，拖入IDA，无壳。通过字符串定位到主函数：

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  signed int v3; // eax
  int v4; // ST38_4
  signed int *v5; // rsi
  signed int v7; // [rsp+2Ch] [rbp-54h]
  char v8; // [rsp+40h] [rbp-40h]
  int v9; // [rsp+78h] [rbp-8h]
  int v10; // [rsp+7Ch] [rbp-4h]

  v9 = 0;
  printf("input your flag:", argv, envp);
  gets(&v8);
  v10 = general_inspection((int (*)[9])sudoku);
  v7 = 0x9471480F;
  while ( 1 )
  {
    while ( 1 )
    {
      while ( v7 == 0x848D30C0 )
      {
        v4 = blank_num((int (*)[9])sudoku);
        v5 = (signed int *)mem_alloc(v4);
        trace((__int64)sudoku, v5, v4);
        check((int (*)[9])sudoku);
        check1(&v8);
        check3(&v8);
        v9 = 0;
        v7 = 0xEDE5424E;
      }
      if ( v7 != 0x9471480F )
        break;
      v3 = 0x848D30C0;
      if ( v10 )
        v3 = 0x27966BFF;
      v7 = v3;
    }
    if ( v7 == 0xEDE5424E )
      break;
    if ( v7 == 0x27966BFF )
    {
      printf("error");
      check((int (*)[9])sudoku);
      v9 = 0;
      v7 = 0xEDE5424E;
    }
  }
  return v9;
}
```

说实话这个变量名真是帮大忙了，百度搜索sudoku发现是数独。看了下sudoku的定义部分确实就是一个数独，于是提取出来放到百度上解出，得到可填入部分：4693641762894685722843556137219876255986

直接把这一串输入题目，返回error

## 2

观察main函数，发现输入的字符串存储在v8，而使用了v8的只有check1()和check3()两个函数，可以初步确定这两个函数比较重要

进行动调。发现在经过check1()之前v8字符串没有变化，经过后似乎是进行了某种加密。check3()虽然逻辑比较乱但其实就是对check2()返回值进行简单判断：

```c
__int64 __fastcall check3(char *a1)
{
  __int64 result; // rax
  signed int v2; // eax
  signed int v3; // [rsp+28h] [rbp-18h]
  int v4; // [rsp+3Ch] [rbp-4h]

  v4 = check2(a1);
  v3 = 0xF607AE;
  while ( 1 )
  {
    while ( v3 == 0xF607AE )
    {
      v2 = 0x5819697A;
      if ( !v4 )
        v2 = 0x4BF1B86E;
      v3 = v2;
    }
    result = (unsigned int)(v3 - 824643665);
    if ( v3 == 824643665 )
      break;
    if ( v3 == 0x4BF1B86E )
    {
      v3 = 0x31271051;
      printf("error!\n");
    }
    else if ( v3 == 0x5819697A )   // if v4 == 1
    {
      v3 = 0x31271051;
      printf("you get it!\n");
    }
  }
  return result;
}
```

查看check2()函数，发现函数内出现了sudoku，而check1()中则没有出现。可以判定check2()是填入数独的函数，check1()是加密函数。

对check1()进行分析后发现其加密过程可分为三个阶段:

```c
__int64 __fastcall check1(char *input)
{
  __int64 result; // rax
  size_t v2; // rax
  signed int v3; // ecx
  char v4; // ST6F_1
  size_t v5; // rax
  signed int v6; // ecx
  char v7; // ST6E_1
  size_t v8; // rax
  signed int v9; // ecx
  signed int v10; // [rsp+68h] [rbp-18h]
  int v11; // [rsp+70h] [rbp-10h]
  int v12; // [rsp+74h] [rbp-Ch]

  v12 = strlen(input) >> 1;
  v11 = 0;
  v10 = 0x5A8A255C;
  while ( 1 )
  {
    while ( 1 )
    {
      while ( 1 )
      {
        while ( 1 )
        {
          while ( 1 )
          {
            while ( 1 )
            {
              while ( 1 )
              {
                while ( v10 == 0x83BBF730 )
                {
                  v8 = strlen(input);
                  v9 = 0xFBFDE91A;
                  if ( v12 < v8 )
                    v9 = 0x75B73061;
                  v10 = v9;
                }
                if ( v10 != 0x89775DDA )
                  break;
                v12 = 0;
                v10 = 0x83BBF730;
              }
              if ( v10 != 0xACF6779C )
                break;
              v5 = strlen(input);
              v6 = 0x89775DDA;
              if ( v12 < v5 )                   // 阶段二:将input中的相邻两个字符互换
                v6 = 0xC34B5938;
              v10 = v6;
            }
            if ( v10 != 0xC34B5938 )
              break;
            v7 = input[v12];
            input[v12] = input[v12 + 1];
            input[v12 + 1] = v7;
            v10 = 0xF740BE75;
          }
          if ( v10 != 0xCE7094F9 )
            break;
          ++v12;
          v10 = 0x83BBF730;
        }
        if ( v10 != 0xEEA33328 )
          break;
        ++v11;
        ++v12;
        v10 = 0x5A8A255C;
      }
      if ( v10 != 0xF740BE75 )
        break;
      v12 += 2;
      v10 = 0xACF6779C;
    }
    result = (unsigned int)(v10 + 0x40216E6);
    if ( v10 == 0xFBFDE91A )
      break;
    switch ( v10 )
    {
      case 0x47E3A40:                           // 阶段一:将input的前半部分和后半部分对换
        v4 = input[v12];
        input[v12] = input[v11];
        input[v11] = v4;
        v10 = 0xEEA33328;
        break;
      case 0x5A8A255C:
        v2 = strlen(input);
        v3 = 0x5CBA7BC7;
        if ( v11 < v2 >> 1 )
          v3 = 0x47E3A40;
        v10 = v3;
        break;
      case 0x5CBA7BC7:
        v12 = 0;
        v10 = 0xACF6779C;
        break;
      case 0x75B73061:
        input[v12] = (input[v12] & 0xF3 | ~input[v12] & 0xC) - 20;// 阶段三:input中每个字符2，3位取反然后-20
        v10 = 0xCE7094F9;
        break;
    }
  }
  return result;
}
```

推测是要让加密后的字符串等于4693641762894685722843556137219876255986这一串需要填入的数字

## 3

于是可以写出如下脚本:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char a[] = "4693641762894685722843556137219876255986";
    int len = strlen(a);
    int i;
    char temp;
    for (i = 0; i < len; i++) {
        temp = a[i] + 20;
        temp = temp & 0xf3 | ~temp & 0xc;
        a[i] = temp;
    }
    for (i = 0; i < len; i+=2) {
        temp = a[i];
        a[i] = a[i+1];
        a[i+1] = temp;
    }
    for (i = 0; i < len / 2; i++) {
        temp = a[i];
        a[i] = a[i+len/2];
        a[i+len/2] = temp;
    }
    printf("%s\n", a);

    return 0;
}
```

用c写的原因是该加密过程中需要取出2，3位进行取反，用Python感觉不太方便

执行后可得出一串字符串:KDEEIFGKIJ@AFGEJAEF@FDKADFGIJFA@FDE@JG@J，填入程序后返回you get it，说明成功

## 4

这道题里面的流程控制的形式有点像汇编那种到处跳来跳去的那种，我寻思是否可以有一种类似反汇编器一样的东西可以把这种流程转化为正常c语言的那种流程
