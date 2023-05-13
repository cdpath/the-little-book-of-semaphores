Most undergraduate Operating Systems textbooks have a module on Synchronization, which usually presents a set of primitives (mutexes, semaphores, monitors, and sometimes condition variables), and classical problems like readerswriters and producers-consumers. 

大多数本科的操作系统教科书都有一个关于同步的模块，通常会介绍一些基本的原语（互斥锁，信号量，监视器，有时还有条件变量），以及经典的问题，如读者-写者问题和生产者-消费者问题。

When I took the Operating Systems class at Berkeley, and taught it at Colby College, I got the impression that most students were able to understand the solutions to these problems, but few would have been able to produce them, or solve similar problems. 

我在伯克利大学学习操作系统课程，以及在科尔比学院教授这门课程时，我感觉到大多数学生能够理解这些问题的解决方案，但能够自己产生这些解决方案或解决类似问题的学生却不多。

One reason students don’t understand this material deeply is that it takes more time, and more practice, than most classes can spare. Synchronization is just one of the modules competing for space in an Operating Systems class, and I’m not sure I can argue that it is the most important. But I do think it is one of the most challenging, interesting, and (done right) fun. 

学生们没有深入理解这个材料的一个原因是，这需要更多的时间和更多的练习，而大多数课程都无法提供这样的时间。同步只是操作系统课程中争夺空间的众多模块之一，我不能肯定说这是最重要的部分。但我确实认为这是最具挑战性，最有趣，而且（如果做得好）最有趣的部分之一。

I wrote the first edition this book with the goal of identifying synchronization idioms and patterns that could be understood in isolation and then assembled to solve complex problems. This was a challenge, because synchronization code doesn’t compose well; as the number of components increases, the number of interactions grows unmanageably. 

我撰写这本书的第一版是为了确定可以独立理解的同步习语和模式，然后将它们组合起来解决复杂的问题。这是一项挑战，因为同步代码并不容易组合；随着组件数量的增加，交互的数量也会变得难以管理。

Nevertheless, I found patterns in the solutions I saw, and discovered at least some systematic approaches to assembling solutions that are demonstrably correct. 

尽管如此，我在我看到的解决方案中发现了一些模式，并至少发现了一些系统的方法来组合解决方案，这些解决方案被证明是正确的。

I had a chance to test this approach when I taught Operating Systems at Wellesley College. I used the first edition of The Little Book of Semaphores along with one of the standard textbooks, and I taught Synchronization as a concurrent thread for the duration of the course. Each week I gave the students a few pages from the book, ending with a puzzle, and sometimes a hint. I told them not to look at the hint unless they were stumped. 

当我在韦尔斯利学院教授操作系统时，我有机会测试这种方法。我使用了《信号量小册子》的第一版，配合一本标准的教科书，我将同步作为课程持续时间的一个并行主题进行教授。每周我都会给学生们几页书中的内容，以一个谜题结束，有时还会给出提示。我告诉他们，除非他们陷入困境，否则不要看提示。


I also gave them some tools for testing their solutions: a small magnetic whiteboard where they could write code, and a stack of magnets to represent the threads executing the code. 

我还给他们提供了一些测试他们解决方案的工具：一个小型磁性白板，他们可以在上面写代码，以及一堆磁铁，代表执行代码的线程。

The results were dramatic. Given more time to absorb the material, students demonstrated a depth of understanding I had not seen before. More importantly, most of them were able to solve most of the puzzles. In some cases they reinvented classical solutions; in other cases they found creative new approaches. 

结果是显著的。给予更多的时间来吸收材料，学生们展现出我以前从未见过的理解深度。更重要的是，他们中的大多数人能够解决大多数的谜题。在某些情况下，他们重新发明了经典的解决方案；在其他情况下，他们找到了创新的新方法。

When I moved to Olin College, I took the next step and created a half-class, called Synchronization, which covered The Little Book of Semaphores and also the implementation of synchronization primitives in x86 Assembly Language, POSIX, and Python. 

当我转到奥林学院时，我迈出了下一步，创建了一个半课程，名为“同步”，涵盖了《信号量小册子》以及在x86汇编语言、POSIX和Python中实现同步原语。

The students who took the class helped me find errors in the first edition and several of them contributed solutions that were better than mine. At the end of the semester, I asked each of them to write a new, original problem (preferably with a solution). I have added their contributions to the second edition. 

参加这门课程的学生帮助我找到了第一版中的错误，其中一些学生提供的解决方案比我自己的还要好。学期结束时，我要求他们每人写一个新的、原创的问题（最好附带解答）。我将他们的贡献添加到了第二版。

Also since the first edition appeared, Kenneth Reek presented the article “Design Patterns for Semaphores” at the ACM Special Interest Group for Computer Science Education. He presents a problem, which I have cast as the Sushi Bar Problem, and two solutions that demonstrate patterns he calls “Pass the baton” and “I’ll do it for you.” Once I came to appreciate these patterns, I was able to apply them to some of the problems from the first edition and produce solutions that I think are better. 

自从第一版出版以来，Kenneth Reek在ACM计算机科学教育特别兴趣小组上发表了一篇名为“信号量设计模式”的文章。他提出了一个问题，我将其称为寿司吧问题，以及两个展示他称之为“传递接力棒”和“我会为你做”的模式的解决方案。一旦我开始欣赏这些模式，我就能将它们应用到第一版中的一些问题上，并产出我认为更好的解决方案。

One other change in the second edition is the syntax. After I wrote the first edition, I learned Python, which is not only a great programming language; it also makes a great pseudocode language. So I switched from the C-like syntax in the first edition to syntax that is pretty close to executable Python1. In fact, I have written a simulator that can execute many of the solutions in this book. 

第二版的另一个变化是语法。在我写完第一版之后，我学习了Python，这不仅是一种很棒的编程语言；它也是一种很好的伪代码语言。所以我从第一版的类C语法转换为接近可执行Python的语法。实际上，我编写了一个模拟器，可以执行本书中的许多解决方案。

Readers who are not familiar with Python will (I hope) find it mostly obvious. In cases where I use a Python-specific feature, I explain the syntax and what it means. I hope that these changes make the book more readable. 

对于不熟悉Python的读者，我希望他们会觉得这本书大部分内容都很明显。在我使用Python特定功能的情况下，我会解释语法及其含义。我希望这些改变会使书更易阅读。

The pagination of this book might seem peculiar, but there is a method to my whitespace. After each puzzle, I leave enough space that the hint appears on the next sheet of paper and the solution on the next sheet after that. When I use this book in my class, I hand it out a few pages at a time, and students collect them in a binder. My pagination system makes it possible to hand out a problem without giving away the hint or the solution. Sometimes I fold and staple the hint and hand it out along with the problem so that students can decide whether and when to look at the hint. If you print the book single-sided, you can discard the blank pages and the system still works. 

这本书的分页可能看起来有些奇特，但我的空白处有其方法。每个难题之后，我留出足够的空间，以便提示出现在下一张纸上，解决方案出现在之后的纸上。当我在课堂上使用这本书时，我一次分发几页，学生们将它们收集在文件夹中。我的分页系统可以在不泄露提示或解决方案的情况下分发问题。有时我会折叠和订好提示，和问题一起分发，这样学生可以决定是否以及何时查看提示。如果你单面打印这本书，你可以丢弃空白页，系统仍然可以工作。

This is a Free Book, which means that anyone is welcome to read, copy, modify and redistribute it, subject to the restrictions of the license. I hope that people will find this book useful, but I also hope they will help continue to develop it by sending in corrections, suggestions, and additional material. Thanks! 

这是一本免费的书，意味着任何人都可以阅读、复制、修改和重新分发它，但需遵守许可证的限制。我希望人们会发现这本书有用，同时也希望他们通过发送更正、建议和额外材料来继续开发它。谢谢！


Allen B. Downey

Needham, MA

June 1, 2005 

