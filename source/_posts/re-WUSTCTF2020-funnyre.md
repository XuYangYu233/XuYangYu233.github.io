---
title: re_[WUSTCTF2020]funnyre
date: 2020-10-14 19:28:55
tags: re
cover: /imgs/home.jpg
coverWidth: 1200
coverHeight: 690
---
一道一点也不funny的逆向题
<!--more-->

## 0

elf文件，无壳。用ida64打开发现没有特殊字符串。。。？

![0IJFyQ.png](https://s1.ax1x.com/2020/10/14/0IJFyQ.png)

四处找找可以找到函数sub_4004C0()，里面有一些异或操作：

```c
__int64 sub_4004C0()
{
  char *v0; // rdx
  int v1; // ecx
  __int64 result; // rax
  signed __int64 v3; // rdx

  v0 = &s;
  do
  {
    v1 = *(_DWORD *)v0;
    v0 += 4;
    result = ~v1 & (v1 - 16843009) & 0x80808080;
  }
  while ( !(_DWORD)result );
  if ( !((unsigned __int16)~(_WORD)v1 & (unsigned __int16)(v1 - 257) & 0x8080) )
    result = (unsigned int)result >> 16;
  if ( !((unsigned __int16)~(_WORD)v1 & (unsigned __int16)(v1 - 257) & 0x8080) )
    v0 += 2;
  v3 = v0 - &byte_60304B[__CFADD__((_BYTE)result, (_BYTE)result)];
  if ( (signed int)v3 > 0 )
  {
    s ^= 0x11u;
    if ( (_DWORD)v3 != 1 )
    {
      byte_603049 ^= 0x12u;
      if ( (_DWORD)v3 != 2 )
      {
        byte_60304A ^= 0x13u;
        if ( (_DWORD)v3 != 3 )
        {
          byte_60304B[0] ^= 0x14u;
          if ( (_DWORD)v3 != 4 )
          {
            byte_60304C ^= 0x15u;
            if ( (_DWORD)v3 != 5 )
            {
              byte_60304D ^= 0x16u;
              if ( (_DWORD)v3 != 6 )
              {
                byte_60304E ^= 0x17u;
                if ( (_DWORD)v3 != 7 )
                {
                  byte_60304F ^= 0x18u;
                  if ( (_DWORD)v3 != 8 )
                  {
                    byte_603050 ^= 0x19u;
                    if ( (_DWORD)v3 != 9 )
                    {
                      byte_603051 ^= 0x1Au;
                      if ( (_DWORD)v3 != 10 )
                      {
                        byte_603052 ^= 0x1Bu;
                        if ( (_DWORD)v3 != 11 )
                        {
                          byte_603053 ^= 0x1Cu;
                          if ( (_DWORD)v3 != 12 )
                            byte_603054 ^= 0x1Du;
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  }
  return result;
}
```

然而写脚本解出来之后发现是字符串"you got flag"。顺着这个字符串的引用找，可以找到一堆有花指令的代码，花指令还是那种很经典的jx+jnx+e8。去掉花指令后f5，可以找到主要函数：

~~代码有1700多行，就不放了~~

总之就是一系列非常复杂的操作，大概告诉我们flag格式为flag{xxx}，中间有32个字符，经过很多操作之后与一个固定的串进行比较，成功则打印出"you got flag"

## 1

由于该函数中flag的位与位之间没有关联，所以可以使用逐位爆破的方法

将该函数复制到c语言文件中，稍加修改即可写出脚本：

```c
int func(char *v5) {
    //修改过后的该函数
}

int main() {
    int i, j, count;
    char v5[] = "flag{                                }";
    char temp[50];
    count = 1;
    for (i = 0; i < 0x20; i++) {
        for (j = 0; j < 0x7f - '0'; j++) {
            strcpy(temp, v5);
            int k = func(temp);
            if (k == count) {
                count++;
                printf("%c", v5[i + 5]);
                break;
            }
            v5[i + 5]++;
        }
    }
    printf("\nflag = %s\n", v5);

    return 0;
}
```

运行即可得出答案：flag{1dc20f6e3d497d15cef47d9a66d6f1af}

## 2

如果去掉花指令后f5失败，显示函数过大，那么可以去ida的配置文件里修改相关参数来避免这个失败(具体怎么改我也忘了，试试搜索引擎

说实话，没啥新意的一道题，亏他好意思说funnyre
