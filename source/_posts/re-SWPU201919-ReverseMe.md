---
title: re_SWPU201919_ReverseMe
date: 2020-10-12 23:46:59
tags: re
cover: /imgs/citynight1.jpg
coverWidth: 4270
coverHeight: 2530
---
一道加密过程花里胡哨的逆向题
<!--more-->

## 1

exe文件，无壳。直接拖入IDA中，根据关键字符串可定位至主要函数中:

```c
int __thiscall sub_EC2810(void *this)
{
  int v1; // eax
  int v2; // ecx
  int v3; // eax
  int v4; // esi
  signed int v5; // edi
  unsigned int v6; // kr00_4
  int *v7; // ecx
  int *v8; // ecx
  int *v9; // ecx
  __int128 *v10; // edx
  unsigned int v11; // edi
  int v12; // eax
  int v13; // eax
  bool v14; // cf
  unsigned __int8 v15; // al
  unsigned __int8 v16; // al
  unsigned __int8 v17; // al
  const char *v18; // edx
  int v19; // eax
  int v20; // eax
  int v22; // [esp-14h] [ebp-D8h]
  int v23; // [esp-10h] [ebp-D4h]
  int *input; // [esp+24h] [ebp-A0h]
  int len_of_input; // [esp+34h] [ebp-90h]
  unsigned int v26; // [esp+38h] [ebp-8Ch]
  __int128 v27; // [esp+3Ch] [ebp-88h]
  __int128 v28; // [esp+4Ch] [ebp-78h]
  int v29; // [esp+5Ch] [ebp-68h]
  __int128 v30; // [esp+60h] [ebp-64h]
  __int128 v31; // [esp+70h] [ebp-54h]
  int v32; // [esp+80h] [ebp-44h]
  __int64 v33; // [esp+84h] [ebp-40h]
  int v34; // [esp+8Ch] [ebp-38h]
  __int16 v35; // [esp+90h] [ebp-34h]
  int v36; // [esp+94h] [ebp-30h]
  int v37; // [esp+98h] [ebp-2Ch]
  int v38; // [esp+9Ch] [ebp-28h]
  int v39; // [esp+A0h] [ebp-24h]
  int v40; // [esp+A4h] [ebp-20h]
  int v41; // [esp+A8h] [ebp-1Ch]
  int v42; // [esp+ACh] [ebp-18h]
  int v43; // [esp+B0h] [ebp-14h]
  int v44; // [esp+C0h] [ebp-4h]

  len_of_input = 0;
  v26 = 15;
  LOBYTE(input) = 0;
  v44 = 0;
  LOBYTE(v44) = 1;
  v1 = print(this, "Please input your flag: ");
  sub_EC3050(v1);
  scan(&dword_EF0068, &input);
  v34 = 1413701433;
  v33 = qword_EEB9A0;
  v35 = 70;
  if ( len_of_input == 32 )
  {
    v40 = -1173078761;
    v41 = 494076752;
    v42 = -1811652486;
    v43 = 688582768;
    v5 = 0;
    v30 = 0i64;
    v32 = 0;
    v31 = 0i64;
    v6 = strlen((const char *)&v33);
    do
    {
      v7 = (int *)&input;
      if ( v26 >= 0x10 )
        v7 = input;
      *((_BYTE *)v7 + v5) ^= *((_BYTE *)&v33 + v5 % v6);// 循环异或
      ++v5;
    }
    while ( v5 < 32 );
    v8 = (int *)&input;
    v4 = (int)input;
    if ( v26 >= 0x10 )
      v8 = input;
    v29 = 0;
    v27 = 0i64;
    v28 = 0i64;
    *(_QWORD *)&v30 = *(_QWORD *)v8;
    *((_QWORD *)&v30 + 1) = *((_QWORD *)v8 + 1);
    *(_QWORD *)&v31 = *((_QWORD *)v8 + 2);
    *((_QWORD *)&v31 + 1) = *((_QWORD *)v8 + 3);
    complex_encrypt(v22, v23, 256, (unsigned int)&v30, (unsigned int)&v27);
    v36 = 0xF80F37B3;
    v37 = 0x5DAEBCBC;
    v9 = &v36;
    v38 = 0x864D5ABA;
    v10 = &v27;
    v39 = 0xD3629744;
    v11 = 28;
    v40 = 0x1624BA4F;
    v41 = 0x1A729F0B;
    v42 = 0x266D6865;
    v43 = 0x67C86BBA;
    while ( 1 )
    {
      v12 = *v9;
      if ( *v9 != *(_DWORD *)v10 )
        break;
      ++v9;
      v10 = (__int128 *)((char *)v10 + 4);
      v14 = v11 < 4;
      v11 -= 4;
      if ( v14 )
      {
        v13 = 0;
        goto LABEL_19;
      }
    }
    v14 = (unsigned __int8)v12 < *(_BYTE *)v10;
    if ( (_BYTE)v12 != *(_BYTE *)v10
      || (v15 = *((_BYTE *)v9 + 1), v14 = v15 < *((_BYTE *)v10 + 1), v15 != *((_BYTE *)v10 + 1))
      || (v16 = *((_BYTE *)v9 + 2), v14 = v16 < *((_BYTE *)v10 + 2), v16 != *((_BYTE *)v10 + 2))
      || (v17 = *((_BYTE *)v9 + 3), v14 = v17 < *((_BYTE *)v10 + 3), v17 != *((_BYTE *)v10 + 3)) )
    {
      v13 = -v14 | 1;
    }
    else
    {
      v13 = 0;
    }
LABEL_19:
    if ( v13 )
      v18 = "Try again!\r\n";
    else
      v18 = "Congratulations! I always knew you could do it.";
    v19 = print(v9, v18);
    sub_EC3050(v19);
    system("pause");
  }
  else
  {
    v3 = print(v2, "Try again!\r\n");
    sub_EC3050(v3);
    system("pause");
    v4 = (int)input;
  }
  if ( v26 >= 0x10 )
  {
    v20 = v4;
    if ( v26 + 1 >= 0x1000 )
    {
      v4 = *(_DWORD *)(v4 - 4);
      if ( (unsigned int)(v20 - v4 - 4) > 0x1F )
        sub_ECAFF7(v26 + 36);
    }
    sub_EC64DE(v4);
  }
  return 0;
}
```

可以看出69到76行这一部分是一个循环异或，将输入的字符串与"SWPU_2019_CTF"进行循环异或，然后将异或后的字符串放入v30，再传入complex_encrypt()，加密结果通过v27带出，与v36进行比较。

## 2

那么再来看一下complex_encrypt()函数:

```c
void __cdecl complex_encrypt(int a1, int a2, int a3, unsigned int a4_v30, unsigned int a5_v27)
{
  int v5; // ecx
  unsigned int v6; // ebx
  int v7; // esi
  unsigned int v8; // edi
  unsigned int v9; // esi
  unsigned int v10; // edx
  unsigned int v11; // esi
  _DWORD *v12; // ecx
  unsigned int v13; // eax
  int v14; // ebx
  unsigned int v15; // esi
  __m128i v16; // xmm0
  __m128i v17; // xmm1
  __m128i v18; // xmm0
  __m128i v19; // xmm1
  __m128i v20; // xmm0
  __m128i v21; // xmm1
  __m128i v22; // xmm0
  __m128i v23; // xmm1
  unsigned int v24; // ebx
  int v25; // eax
  unsigned int v26; // esi
  int v27; // edi
  int v28; // ecx
  signed int v29; // [esp+20h] [ebp-20h]
  int v30; // [esp+24h] [ebp-1Ch]
  _DWORD *v31; // [esp+28h] [ebp-18h]
  int v32; // [esp+2Ch] [ebp-14h]
  int v33; // [esp+30h] [ebp-10h]
  int v34; // [esp+34h] [ebp-Ch]
  int v35; // [esp+38h] [ebp-8h]

  v6 = a5_v27;
  v7 = v5;
  v8 = (unsigned int)(a3 + 31) >> 5;
  v30 = 4 * v8;
  v31 = (_DWORD *)sub_ECB048(4 * v8);
  v32 = -1839987866;
  v33 = 120;
  v34 = -1839987866;
  v35 = 120;
  sub_EC2270(v7, &v32);
  sub_EC20E0();
  sub_EC2150();
  sub_EC1F80();
  v29 = 0;
  if ( v8 )
  {
    do
    {
      dword_EF0D94 = 2 * dword_EF0DA4 ^ (unsigned __int16)(dword_EF0DBC ^ 2 * dword_EF0DA4);
      dword_EF0D78 = (dword_EF0DB8 << 16) | ((unsigned int)dword_EF0DAC >> 15);
      v9 = (dword_EF0D70 << 16) | ((unsigned int)dword_EF0D90 >> 15);
      dword_EF0D8C = (dword_EF0D68 << 16) | ((unsigned int)dword_EF0D84 >> 15);
      dword_EF0D7C = (dword_EF0D70 << 16) | ((unsigned int)dword_EF0D90 >> 15);
      v31[v29] = v9 ^ sub_EC2150();
      sub_EC1F80();
      ++v29;
    }
    while ( v29 < (signed int)v8 );
    v6 = a5_v27;
  }
  v10 = 0;
  if ( v8 )
  {
    v11 = a4_v30;
    if ( v8 < 0x10 || v6 <= a4_v30 + v30 - 4 && v6 + v30 - 4 >= a4_v30 )
    {
      v12 = v31;
    }
    else
    {
      v12 = v31;
      if ( v6 > (unsigned int)&v31[v8 - 1] || v6 + v30 - 4 < (unsigned int)v31 )
      {
        v13 = a4_v30 + 16;
        v12 = v31;
        v14 = v6 + 32;
        v15 = (unsigned int)v31 - a4_v30;
        do
        {
          v16 = *(__m128i *)(v13 - 16);
          v13 += 64;
          v14 += 64;
          v17 = _mm_xor_si128(*(__m128i *)&v31[v10], v16);
          v18 = *(__m128i *)(v13 - 64);
          *(__m128i *)(v14 - 96) = v17;
          v19 = _mm_xor_si128(*(__m128i *)(v15 + v13 - 64), v18);
          v20 = *(__m128i *)(v13 - 48);
          *(__m128i *)(a5_v27 - a4_v30 + v13 - 64) = v19;
          v15 = (unsigned int)v31 - a4_v30;
          v21 = _mm_xor_si128(*(__m128i *)((char *)v31 + v14 - a5_v27 - 64), v20);
          v22 = *(__m128i *)(v13 - 32);
          *(__m128i *)(v14 - 64) = v21;
          v23 = *(__m128i *)&v31[v10 + 12];
          v10 += 16;
          *(__m128i *)(v14 - 48) = _mm_xor_si128(v23, v22);
        }
        while ( v10 < (v8 & 0xFFFFFFF0) );
        v6 = a5_v27;
        v11 = a4_v30;
      }
    }
    if ( v10 < v8 )
    {
      v24 = v6 - a4_v30;
      v25 = v11 + 4 * v10;
      v26 = (unsigned int)v12 - a4_v30;
      v27 = v8 - v10;
      do
      {
        v28 = *(_DWORD *)(v26 + v25);           // v12 + 4 * v10
        v25 += 4;
        *(_DWORD *)(v24 + v25 - 4) = *(_DWORD *)(v25 - 4) ^ v28;
        --v27;
      }
      while ( v27 );
    }
  }
  sub_ECAE05(v31);
}
```

第一眼看上去非常复杂，而且还用到了xmm寄存器。但仔细分析后就可以发现106行以前的部分都是在固定地生成一张表，真正的加密在106以后的一小部分，其实是就是把输入的字符串与表进行了一次异或。通过动调可以获得这个表。

## 3

于是可以写出脚本:

```Python
table = [0xca3e0c86, 0x19aed798, 0xa66b77e2, 0xb077a16a, 0x05379169, 0x307bf97a, 0x104b5a43, 0x28d47d86]
res = [0xF80F37B3, 0x5DAEBCBC, 0x864D5ABA, 0xD3629744, 0x1624BA4F, 0x1A729F0B, 0x266D6865, 0x67C86BBA]
c = []
for i in range(8):
    c.append(res[i] ^ table[i])
b = ""
for i in c:
    temp = hex(i)
    b += temp[8:10]
    b += temp[6:8]
    b += temp[4:6]
    b += temp[2:4]
blist = []
for i in range(len(b)):
    if i % 2 == 0:
        temp = b[i:i+2]
        blist.append(int(temp, 16))
a = "SWPU_2019_CTF"
for i in range(len(blist)):
    print(chr(blist[i] ^ ord(a[i%len(a)])), end="")
```

运行后得到flag{Y0uaretheB3st!#@_VirtualCC}

## 4

灵活运用动调。

另外，感谢做题时robot-r sf在一旁提出了一些指导，差点就强行分析了一波complex_encrypt()。。。
