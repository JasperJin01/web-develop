---
title: 华中科技大学考研复试 汇编语言笔记
date: 2024-03-18
category: Jekyll
layout: post
---



# 基础知识（寻址方式、指令）

1. 乘法指令（MUL,IMUL）==MUL和IMUL语法和要求都是一样的吧？==
    * MUL OPS
    * MUL OP
    * MUL
2. 除法指令
3. 











# 寻址方式与基础

1. 常见寄存器：

    通用寄存器：EAX、EBX、ECX、EDX——双字/4字节；AX、BX等——单字/2字节；AX拆分为AH和AL（H high 高字节，L low 低字节）

    段寄存器：CS、DS、ES等。代码段寄存器CS不能赋值，其他寄存器可以赋字字寄存器，不能赋**立即数/段地址/段名**。例：MOV DS, DATA ❌DATA是段名；MOV DS, SEG BUG ❌段地址

    其他寄存器：SI 字、DI 字、ESI 双字

2. <img src="/Users/jmjin/Library/Application Support/typora-user-images/image-20240115104144749.png" alt="image-20240115104144749" style="zoom:50%;" />

3. **A   DW   B** ; A，B为变量，则A的初始值为B的**偏移地址**（应该把变量A、B理解为地址）

4. 双操作数指令，对两操作数的要求：**不能同时是地址**，同时因为变量算是地址，不能同时是变量。**不允许出现不带方括号的寄存器符号**。

5. 移位指令 SHL AX, CL

6. 如果想把变量A的值赋给变量B，`mov AX [A], mov [B] AX`。注意把寄存器的值赋给变量时的[B]。据说MOV B AX的写法是语法错误的，不太懂。







**<font color = red>SEG&OFFSET</font>**





1. 如果 `BUF` 是一个变量或标签，它会被解析为一个内存地址。 









CS寄存器用来存储当前执行代码段的基地址

- **DS**（Data Segment）: 数据段寄存器，用于指向程序中使用的数据的段的基地址。
- **ES**（Extra Segment）: 附加段寄存器，通常用于指向额外的数据段的基地址，供字符串和其他数据操作使用。
- **SS**（Stack Segment）: 堆栈段寄存器，用于指向程序堆栈的段的基地址。

