---
title: >-
  论文阅读-Memory Management Test-Case Generation of C Programs using Bounded Model
  Checking
date: 2025-01-19 20:35:04
tags:
categories:
  - C/C++单元测试生成
---

论文链接：[Memory Management Test-Case Generation of C Programs Using Bounded Model Checking | SpringerLink](https://link.springer.com/chapter/10.1007/978-3-319-22969-0_18)

## 使用工具

- Efficient SMT-Based Bounded Model Checking（**ESBMC**，原文：[SMT-Based Bounded Model Checking for Embedded ANSI-C Software | IEEE Journals & Magazine | IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/5928354)）：ESBMC以C语言源程序为输入，以一系列“Claim”为输出，这些Claim指明了该程序的安全属性，即可能出现内存错误的地方，包括检查与算术不足和溢出、零除法、越界索引、指针安全、死锁和数据竞争相关的属性。在ESBMC中，验证过程是完全自动化的，不需要用户使用前条件或后条件注释程序。
- **Safety Properties**：论文定义了“安全属性”的概念。如果一个系统不能满足一个安全属性，那么就存在一个有限的执行来显示这个失败，使用断言将对应的错误复现，就可以生成针对性的testcase。
- **CUnit**（[CUnit - Introduction (sourceforge.net)](https://cunit.sourceforge.net/doc/introduction.html#usage)）：测试框架。CUnit支持更好的组织和代码重用，以更有效的方式创建单元测试。其支持完整的C语言书写，并提供了一组用于测试逻辑条件的断言。

## 方法过程

![image-20250121204757596](../assets/%E8%AE%BA%E6%96%87%E9%98%85%E8%AF%BB-Memory-Management-Test-Case-Generation-of-C-Programs-using-Bounded-Model-Checking/image-20250121204757596.png)

1. 用ESBMC识别安全属性，得到Claims。
2. 从安全属性中提取信息。即从四个方面分析Claim：identification、comments、line number、property。
3. 翻译安全属性。即将Claims翻译成C程序中的断言代码。
4. 内存跟踪分析。使用Pycparser（[eliben/pycparser: :snake: Complete C99 parser in pure Python (github.com)](https://github.com/eliben/pycparser)）生成C语言的语法树AST，再根据AST进行分析，目的是确保指针安全性。分为两个阶段：
   1. 识别和跟踪所分析的源代码中的变量，以及变量的操作和分配。对于非指针，直接进行变量映射；对于指针，执行对象映射，跟踪指针的运算、分配、回收。
   2. 对源代码的特定功能，用于根据程序执行监视内存地址和这些变量指向的地址。插入函数`mark_map_MF`，改函数以前阶段映射的数据为输入，维护一个包含内存地址、指向的地址、作用域范围、标识符、行数的列表，最后生成Map2Check提供的断言函数。
5. 根据ESBMC和Map2Check生成的断言生成testcase，包括原生C语言的断言和测试框架的断言函数。
6. 测试实施，包括两个测试模型。
7. 执行测试。

## 一些疑问

- 论文里变量的`dynamic`是什么意思？
- 两个测试模型没看懂🤯
