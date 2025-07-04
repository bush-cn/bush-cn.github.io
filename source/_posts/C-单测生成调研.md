---
title: C++单测生成调研
date: 2025-01-12 13:06:39
tags:
  - C/C++单元测试生成
categories:
  - C/C++单元测试生成
---

# C++单测生成调研

调研C++单元测试生成在Undefined Behavior、内存、指针、Error Handling四个方面的特点、挑战及现状，据此考虑主要根据哪些特点来迁移已有模型。

## 1. Undefined Behavior

未定义行为（**Undefined Behavior**, UB）是编程语言规范中未对某些构造强制规定行为的部分。它为编译器优化提供了自由，但可能导致程序在不同平台上的行为不一致。<u>C/C++比Java具有更多UB</u>。

> A difficult trade-off in the design of a systems programming language is how much freedom to grant the compiler to generate efficient code for a target instruction set. On one hand, programmers prefer that a program behaves identically on all hardware platforms. On the other hand, programmers want to get high performance by allowing the compiler to exploit specific properties of the instruction set of their hardware platform. A technique that languages use to make this trade-off is labeling certain program constructs as **undefined behavior**, for which the language imposes no requirements on <u>compiler</u> writers.
>
> From: [Kaashoek_Undefined behavior.pdf (mit.edu)](https://dspace.mit.edu/bitstream/handle/1721.1/86949/Kaashoek_Undefined behavior.pdf)

Example:

- division by zero
- oversized shift
- signed integer overflow
- out-of-bounds pointer
- null pointer dereference
- type-punned pointer dereference（类型转换指针解引用）
- uninitialized read

----

未找到针对性的测试用例生成的论文。

## 2. 内存

[EffectiveSan: Type and Memory Error Detection using Dynamically Typed C/C++ (arxiv.org)](https://arxiv.org/pdf/1710.06125)

C/C++类型系统是静态且弱的，会出现**类型错误**（例如错误的类型转换）和**内存错误**（例如数组越界、使用释放后的内存等）。

提出了**动态类型** C/C++ 的概念。要点如下：

1. 每个分配的对象都会绑定**动态类型信息**（称为“有效类型”），用于描述其类型、边界和子对象布局。
2. 在运行时检查指针使用是否合法，包括指针的类型正确性和边界正确性。
3. EffectiveSan 支持复杂对象的子对象布局：涵盖嵌套结构体、类的继承、联合等。

---

[sefm2015-libre.pdf (d1wqtxts1xzle7.cloudfront.net)](https://d1wqtxts1xzle7.cloudfront.net/76763038/sefm2015-libre.pdf?1639840124=&response-content-disposition=inline%3B+filename%3DMemory_Management_Test_Case_Generation_o.pdf&Expires=1736853826&Signature=VM6--U4EKTgiW1nLN-1mQXQkfkmonBSz5V5MVphAvpKUSmYedrxd6oAj54WBdfZdr~XLyU9oMYH6zq6MHsGnN1LpFcS7aiVs9RMDuC00b8UmBZJtydQXjWVUpW2pnJKtCtL5Fwj0TptHUrzgVudnBaUGAdPuEg7y1GAMPcR1q~OZy0mcSXYNSW2-S21MESUWGVWSmkohHiUEjYmrYYzxxUgNmR2FuzZIm9ktaiin~ZMqWX1R3ZyrhyOYivnoLmV542tU0odxKT3VyI5ITzZ9IJ-RqT8M1bvAtXuf6x2YMjP3CY~RBgJpmfqlwWGKRhje266VLoYnrpj3AXA4Vb10gQ__&Key-Pair-Id=APKAJLOHF5GGSLRBV4ZA)

名为Map2Check的工具。提供了一种改进的**单元测试生成**方法，结合**ESBMC模型检查器**和动态分析技术，显著提高了**内存管理**缺陷检测的精度。

1. 识别安全属性，利用ESBMC提取潜在的程序缺陷。
2. 从安全属性中提取信息，如位置和潜在缺陷。
3. 将安全属性翻译为C代码中的**断言**。
4. 内存跟踪，记录程序执行期间内存地址的使用情况。
5. 在源代码中添加**断言**以验证安全属性。
6. 实现测试用例，通过CUnit或原生C断言执行测试。
7. 执行测试并分析结果。

## 3. 指针

[An automated test data generation method for void pointers and function pointers in C/C++ libraries and embedded projects - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0950584922000027)

介绍了一种名为VFP（Void and Function Pointers test data generation）的自动化测试数据生成方法，旨在提高C/C++库和嵌入式项目中void指针和函数指针的测试覆盖率。

- **背景**：在C/C++项目中，指针的使用非常普遍，尤其是void指针和函数指针。这些指针在实际项目中频繁出现，但目前对于这类指针的测试数据生成研究较少。

- **方法**：VFP方法基于Concolic测试方法（结合了静态和动态测试的方法），通过预处理源代码来找出所有可能的void指针类型和函数指针的引用。VFP方法的关键在于分析源代码，找出void指针的所有可能类型和函数指针的引用，并利用这些信息生成初始测试数据。
  1. **参数预处理**：分析被测单元内部和外部的源代码，找出所有可能的void指针类型和函数指针引用。
  2. **随机测试数据生成**：使用类型映射和引用映射，结合随机值生成，为其他数据类型生成初始测试数据。
  3. **测试数据执行和测试路径分析**：执行生成的测试数据，收集覆盖的测试路径，并尝试找出未覆盖的节点和最短未覆盖测试路径。
  4. **定向测试数据生成**：基于未覆盖的测试路径，生成路径约束，并转换为SMT-Lib表达式，由SMT求解器求解，生成新的测试数据。

## 4. Error Handling

C语言的错误处理方式是通过函数返回值指示错误状态（通常还有宏定义的errno），通过条件语句处理错误。C++在保留C风格的错误处理方式时，新增了异常处理。

[Automatically diagnosing and repairing error handling bugs in C | Proceedings of the 2017 11th Joint Meeting on Foundations of Software Engineering (acm.org)](https://dl.acm.org/doi/abs/10.1145/3106237.3106300)

研究结果表明，错误处理漏洞通常由四个原因引起：错误/缺失的错误检查（EC）、错误/缺失的错误传播（EP）、错误/缺失的错误输出（EO）和错误/缺失的资源释放（RR）。作者利用这些发现设计并实现了一个名为ErrDoc的工具，该工具使用受限符号执行来探索所有可能的错误路径，并使用静态分析技术验证错误值是否被检查、向上游传播或记录。

1. **探索错误路径**：ErrDoc通过符号执行来识别所有可能的错误路径，并标记CFG中的相应节点和边。
2. **检测和分类错误处理漏洞**：ErrDoc利用前一步收集的信息，输出有漏洞的程序位置和相应的漏洞类别。
3. **修复错误处理漏洞**：ErrDoc自动修复四种类型的错误处理漏洞，通过修改AST来引入修复

（自动修复=>生成对应处的用例？）
