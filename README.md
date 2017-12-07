# Introduction

NSOperation和GCD更高效，创建和管理线程仍支持

> 开发新App时，推荐使用其他并发技术，[_Concurrency Programming Guide_](https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)

## Organization of This Document

[About Threaded Programming](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/AboutThreads/AboutThreads.html#//apple_ref/doc/uid/10000057i-CH6-SW2) 介绍线程的概念和在app设计中的角色

[Thread Management](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW2) 介绍线程技术和如何使用

[Run Loops](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1) 介绍在子线程中如何管理事件处理Loop

[Synchronization](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW1) 介绍同步问题及相关工具

[Thread Safety Summary](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-SW1) 线程安全的总结和一些关键framework的介绍

## See Also

其他并发技术：[_Concurrency Programming Guide_](https://youmingtaiziback.gitbooks.io/concurrency-programming-guide/content/)

本文只介绍了少量的POSIX threads API，更多信息和用法参考_Programming with POSIX Threads _by David R. Butenhof



