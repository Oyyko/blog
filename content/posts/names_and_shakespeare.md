---
title: name and shakespeare
date: 2021-01-25
tags: [Translation]
---

匿名函数与莎士比亚大定理
<!-- more -->

This article is translated from [Here](https://cs.stackexchange.com/questions/22497/why-is-it-important-for-functions-to-be-anonymous-in-lambda-calculus)

## 提问：为什么在lambda calculus里面，函数必须是匿名的。

我正在观看一个Youtube视频，在这个视频里面，讲者介绍了Y组合子的概念。
Y组合子概念产生的动机之一，正如讲者所述，是为了用lambda calculus来表示递归函数，以使得Church理论(任何能被实际上计算的东西都可以用lambda calculus来计算)保持成立。
我的问题在于：为什么我们不能通过名字来简单的调用一个函数。例如日常生活中我们经常会写下这种式子：
$$
n(x,y)=x+y
$$
但是在lambda calculus里面我们不允许把这个函数与名字n相关联，我们只能匿名的定义它为
$$
(x,y)\rightarrow x+y
$$
为什么在lambda calculus里面我们不能拥有被命名的函数? 如果存在具名函数，我们会破坏什么准则? 或者仅仅是我搞错了视频的意思?


## 回答
关于这个问题的主要结论来自一个十六世纪晚期的英国数学家，他叫莎士比亚。他最著名的关于这个问题的论文名为《罗密欧与茱丽叶》，在1597年发表。

他的主要结论在第二幕的第二场景中阐明。即如下的著名定理：
> 名称有什么关系呢?玫瑰不叫玫瑰,依然芳香如故!

这个定理可以被直观的理解为"名字对意义毫无帮助"
莎士比亚的论文的大部分内容是一个用来补充定理的例子，用以表明名字尽管名字没有任何意义，但它们却是无穷无尽的问题的根源。
正如莎士比亚所指出的那样，名称可以在不改变含义的情况下进行更改，这一操作后来被丘奇及其追随者称为α转换。结果，如何确定名字表示的意义变成了造成了许许多多的问题。例如我们要发展“**环境**”的概念，在环境中名字-意义联系是确定的，并且发展出一系列的规则来辨别当前的环境。这使得计算机科学家们困惑了很长一段时间，引起了诸如臭名昭著的Funarg问题之类的技术难题。“环境”在许多流行的编程语言中仍然是个大问题，几乎和莎士比亚在其论文中提出的例子一样**致命**。
这个问题也与形式语言理论中提出的问题接近，即必须将字母和形式系统定义为同构，以便强调字母符号是抽象实体，而与它们如何作为某些集合中的元素而“实现”无关。
莎士比亚的主要结果也表明了科学随后即将与魔术和宗教告别。因为在魔术与宗教的世界里面，人们认为一个东西有它的“真名”。
所有这一切的结论是：尽管名字可以方便人们的日常工作和生活，但是在理论研究中不被名字而困扰更为重要。
记住：**不是所有被叫做娘的都是你的母亲。**

### 评论
最近，玫瑰正在被foobar所取代。