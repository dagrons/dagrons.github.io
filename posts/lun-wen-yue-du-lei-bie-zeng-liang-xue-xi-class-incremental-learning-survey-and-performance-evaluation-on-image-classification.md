<!--
.. title: 论文阅读-类别增量学习-Class-incremental learning: survey and performance evaluation on image classification
.. slug: lun-wen-yue-du-lei-bie-zeng-liang-xue-xi-class-incremental-learning-survey-and-performance-evaluation-on-image-classification
.. date: 2021-08-21 13:28:23 UTC+08:00
.. tags: incremental learning
.. category: 
.. link: 
.. description: 
.. type: text
-->

[TOC]

> 基于增量学习的恶意代码家族分类
> 为什么用增量学习 + 恶意代码家族分类?
> 恶意代码家族分类是需要增量学习的典型任务场景, 恶意代码家族不可能预先给定, 不同的恶意代码家族会随着发展不断产生变体, 而在所有数据集上重新训练模型也是难以实现的, 是典型的class-incremental learning场景
> 其次, BODMAS数据集中包含500多个恶意代码家族, 为研究增量学习提供了可能性
> 再其次, 增量学习目前发展较慢, 仍存在大量问题, 通过增量学习实现恶意代码家族分类是有意义的, 而针对恶意代码家族分类去研究增量学习也是有意义的

## 简介

In most incremental learning scenarios, tasks are presented to a learner in a sequence of delineated *training sessions* during which data from a single task is available for learning.
After each training session, the learner should be capable of performing all perviously seen tasks on unseen data.
> 在增量学习场景下, 每一轮训练针对一个任务进行训练, 在训练结束后, 要求模型能对前面训练过的所有任务进行完成

The main challenge in incremental learning is to learn from data from the current task in a way that prevents forgetting of previously learned tasks.
> 增量学习的主要问题就是避免当前训练对以前任务的遗忘

这一点和迁移学习类似但也有所不同, 迁移学习不要求对以前训练任务的知识实现保留

They aim to exploit knowledge from previous classes to improve learning of new ones(*forward transfer*), as well as exploiting new data to improve performance on previous tasks(*backward transfer*)
> 用旧知识帮助学习新知识, 用新知识巩固旧知识, sounds make sense

## 一些常见概念

### task-IL vs class-IL

task-IL: task-incremental learning, 在inference的时候, 会给定样本的task ID

class-IL: class-incremental learning, 在inference的时候, 不给定样本的task ID

这里还是有些不清楚


### SIL vs CIL vs FIL

1. SIL

问题：由于新数据的各种原因，样本的特征值可能会改变，每个类别的比例也会改变。这些都会影响分类的准确率。

任务：因此，需要确保在现有知识的情况下，通过新样本的增量学习来提取新知识，融合新旧知识以提高分类的准确性。

2. CIL
任务：识别新类，并将其加入现有类别的集合中，提升分类的准确性和智能。

3. FIL
一些新的属性特征能够将分类提升到一个很大的程度，并提升分类准确率。

任务：在现有特征空间的基础上，加入新的属性特征，构建新的特征空间，提升分类准确率。


## 问题

catastrophic forgetting(灾难性遗忘), 为了克服灾难性遗忘, 我们一方面希望模型能从新数据中有效学习新知识, 但另一方面又必须防止新输入的数据对已有知识的显著干扰(稳定性), 即**稳定性-可塑性困境**(stability-plasticity dilemma)

增量学习的主要研究目的就是在计算和存储资源有限的条件下, 在稳定性-可塑性困境中寻找最佳平衡点


## 方法

1. 正则(regularization)

2. 回放(rehearsal)

3. 参数隔离(parameter isolation)


