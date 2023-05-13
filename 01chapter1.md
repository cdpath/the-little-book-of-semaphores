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

## Execution model 执行模型

In order to understand software synchronization, you have to have a model of how computer programs run. In the simplest model, computers execute one instruction after another in sequence. In this model, synchronization is trivial; we can tell the order of events by looking at the program. If Statement A comes before Statement B, it will be executed first.

为了理解软件同步，你必须对计算机程序如何运行有一个思维模型。在最简单的模型中，计算机按顺序执行一个接一个的指令。在这个模型中，同步是很简单的；我们可以通过查看程序来确定事件的顺序。如果语句A在语句B之前，它将首先被执行。

There are two ways things get more complicated. One possibility is that the computer is parallel, meaning that it has multiple processors running at the same time. In that case it is not easy to know if a statement on one processor is executed before a statement on another.

有两种情况会变得更复杂。一种可能是计算机是并行的，意味着它有多个处理器同时运行。在这种情况下，我们很难知道一个处理器上的语句是否在另一个处理器上的语句之前执行。

Another possibility is that a single processor is running multiple threads of execution. A thread is a sequence of instructions that execute sequentially. If there are multiple threads, then the processor can work on one for a while, then switch to another, and so on.

另一种可能是单个处理器运行多个执行线程。线程是按顺序执行的指令序列。如果有多个线程，那么处理器可以先处理一个线程，然后切换到另一个线程，依此类推。

In general the programmer has no control over when each thread runs; the operating system (specifically, the scheduler) makes those decisions. As a result, again, the programmer can't tell when statements in different threads will be executed.

通常情况下，程序员无法控制每个线程何时运行；操作系统（具体来说，调度器）会做出这些决定。因此，程序员无法确定不同线程中的语句何时被执行。

For purposes of synchronization, there is no difference between the parallel model and the multithread model. The issue is the same---within one processor (or one thread) we know the order of execution, but between processors (or threads) it is impossible to tell.

就同步而言，并行模型和多线程模型之间没有区别。问题是一样的——在一个处理器（或一个线程）内部，我们知道执行顺序，但在处理器（或线程）之间，我们无法判断。

A real world example might make this clearer. Imagine that you and your friend Bob live in different cities, and one day, around dinner time, you start to wonder who ate lunch first that day, you or Bob. How would you find out?

一个现实世界的例子可能会让这一点更清楚。设想你和你的朋友鲍勃住在不同的城市，有一天在晚餐的时候，你开始想知道当天谁先吃的午餐，是你还是鲍勃。你怎么才能知道呢？

Obviously you could call him and ask what time he ate lunch. But what if you started lunch at 11:59 by your clock and Bob started lunch at 12:01 by his clock? Can you be sure who started first? Unless you are both very careful to keep accurate clocks, you can't.

当然，你可以给他打电话，问他吃午餐的时间。但是，如果你按照自己的钟表在11:59开始吃午餐，而鲍勃按照他的钟表在12:01开始吃午餐呢？你能确定谁先开始吗？除非你们的时钟都是准确的，否则你就无法确定。

Computer systems face the same problem because, even though their clocks are usually accurate, there is always a limit to their precision. In addition, most of the time the computer does not keep track of what time things happen. There are just too many things happening, too fast, to record the exact time of everything.

计算机系统面临相同的问题，因为尽管它们的时钟通常是准确的，但是精确度总是有限的。此外，大多数时候计算机并不跟踪事情发生的时间。事情发生得太多、太快，无法记录每件事的确切时间。

Puzzle: Assuming that Bob is willing to follow simple instructions, is there any way you can *guarantee* that tomorrow you will eat lunch before Bob?

问题：假设鲍勃愿意按照简单的指令行事，有没有办法确保明天你在鲍勃之前吃午餐？

