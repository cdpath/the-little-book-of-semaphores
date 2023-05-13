---
title: Introduction
---

In common use, "synchronization" means making two things happen at the same time. In computer systems, synchronization is a little more general; it refers to relationships among events---any number of events, and any kind of relationship (before, during, after).

在常规用法中，“同步”意味着让两件事同时发生。在计算机系统中，同步的概念稍微广泛一些；它涉及到事件之间的关系——任何数量的事件，以及任何类型的关系（在...之前，期间，之后）。

Computer programmers are often concerned with **synchronization constraints**, which are requirements pertaining to the order of events. Examples include:

计算机程序员经常关注**同步约束**，这些约束涉及到事件的顺序。例如：

Serialization:

:   Event A must happen before Event B.

Mutual exclusion:

:   Events A and B must not happen at the same time.

序列化：

: 事件A必须在事件B之前发生。

互斥：

: 事件A和事件B不能同时发生。


In real life we often check and enforce synchronization constraints using a clock. How do we know if A happened before B? If we know what time both events occurred, we can just compare the times.

在现实生活中，我们通常使用时钟来检查和执行同步约束。我们如何知道A是在B之前发生的呢？如果我们知道两个事件发生的时间，我们可以直接比较时间。

In computer systems, we often need to satisfy synchronization constraints without the benefit of a clock, either because there is no universal clock, or because we don't know with fine enough resolution when events occur.

在计算机系统中，我们经常需要在没有时钟的情况下满足同步约束，原因可能是没有通用的时钟，或者我们不知道事件发生的精确时间。

That's what this book is about: software techniques for enforcing synchronization constraints.

这就是这本书的主题：执行同步约束的软件技术。
