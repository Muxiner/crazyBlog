---
title: CSAPP：BUFBOMB缓存区溢出攻击实验
date: 2021-1-9 20:08:50
categories: 实验记录
tags:
- 计算机基础
- Csapp
- 实验记录
cover: /Images/21.jpg
---

### 实验目的
加深对IA-32函数调用规则和栈帧结构的理解
### 实验要求
+ 对目标程序实施缓冲区溢出攻击（buffer overflow attacks）
+	通过造成缓冲区溢出来破坏目标程序的栈帧结构
+	继而执行一些原来程序中没有的行为

##### 实验文件
  1. `bufbomb`: 
     实验中实施缓冲区溢出攻击的目标程序
  2. `makecookie`: 
     该程序基于命令行参数（用户名/学号/输入的字符）产生一个唯一的由8个16进制数字组成的字节序列，称为cookie，用作实验中需要置入栈中的数据之一并区分不同学生的实验

      + 指令 :
        `./makecookie 学号` 
        例如：![](/csapp-bufbomb/cookie.png)
  3. `hex2raw`: 
     字符串格式转换程序
##### 目标程序介绍
1. `userid`:
    + 以给定的用户ID `userid`（例如学号或字符）运行程序。
    + 每次在运行程序时均应指定该参数
    + 因为`bufbomb`程序将基于`userid`决定内部使用的`cookie`值（同makecookie程序的输出）
    + 在`bufbomb`程序内部，一些关键的栈地址取决于`userid`所对应的`cookie`值。
2. `-h`：打印可用命令行参数列表
3. `-n`：以`Nitro`模式运行，专用于`Level 4`实验阶段

##### 测试命令
1. `./hex2raw < ×××××.txt | ./bufbomb -u [userid]`
    + `×××××.txt`为攻击字符串所在文本文件
    + `userid`为学号

2. 第四阶段测试命令
`./hex2raw  -n < ×××××.txt | ./bufbomb -n -u [userid]`

3. 如图 ![](/csapp-bufbomb/test.png)

### 实验过程
#### smoke（level0）(最简单)
