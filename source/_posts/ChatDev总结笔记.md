---
title: ChatDev总结笔记
date: 2025-02-22 14:59:16
tags: 
  - ChatDev
categories:
  - 虚拟化软件开发团队
---

# 基于大模型智能体的虚拟化软件开发团队

## 项目介绍

软件开发过程涉及到需求分析、总体设计、详细设计、编码和测试等多种活动和多种角色的开发者。随着大模型技术的快速发展，很多软件开发活动都可以通过自动化和智能化方式来实现。本项目利用**大模型**和**多智能体**技术来实现智能化软件开发全过程，通过agent模拟不同角色的软件开发人员，通过agent之间的协同完成复杂的软件开发过程。

## 参考文献

https://openreview.net/pdf?id=yW0AZ5wPji

# ChatDev论文总结

## What is ChatDev?

ChatDev, a *chat*-powered software-*dev*elopment framework integrating multiple "software agents" for active involvement in three core phases of the software lifecycle: design, coding, and testing.

## Methodology

### 1. chat chain

ChatDev introduces *chat chain* to further break down each phase into smaller and manageable subtasks

- waterfall model -> chat chain
- chat chain -> sequential phases
- phase -> sequential subtasks
  - coding phase -> code writing + completion
  - testing phase -> code review (static testing) + system testing (dynamic testing)
- subtask -> extraction of solutions -> dialogue of Instructor & Assistant
- dialogue -> loop -> instructor instructing while assistant responding

---

- **Agentization**: inception prompting mechanism
- **Memory**: *short-term memory* / *long-term memory*

### 2. communicative dehallucination

ChatDev devise *communicative dehallucination* to alleviate unexpected hallucinations, which <u>encourages the assistant to actively seek more detailed suggestions from the instructor before delivering a formal response.</u>

## Evaluation

- Baselines: GPT-Engineer(fundamental), MetaGPT(advanced)
- Datasets: SRDD
- Metrics: Completeness + Executability + Consistency = Quality

The most important: multiple agents and roles

## **Limitations** & Solutions

1. Firstly, the capabilities of autonomous agents in software production might be overestimated: <u>it is crucial to clearly define detailed software requirements.</u>
2. Secondly, unlike traditional function level code generation, automating the evaluation of general-purpose software is highly complex: <u>consider additional factors such as functionalities, robustness, safety, and user-friendliness.</u>
3. Thirdly, compared to single-agent approaches, multiple agents require more tokens and time, increasing computational demands and environmental impact: <u>enhance agent capabilities with fewer interactions</u>

# 想法

- 针对论文提到的第二点不足，利用多模态大模型添加用户这一agent，提供对应用的界面、操作、使用等的反馈，作为增加的评判软件质量的标准并加以迭代
