# Synchronization {#pageTitle}

## Synchronization Tools

#### Atomic Operations

不阻塞竞争的线程

#### Memory Barriers and Volatile Variables

优化过程中，编译器重新排列汇编级指令，但有可能会出现错误结果

Memory Barriers是非阻塞工具，内存围栏前面的内存操作总是先于内存围栏后面的操作，OSMemoryBarrier

Volatile variables，每次使用变量时从内存加载而不是从寄存器

memory barriers和volatile variables会减少编译器带来的性能优化

#### Locks

| Lock | Description |
| :--- | :--- |
| Mutex | lock是信号量的一种~~（为什么执行锁的那一行代码没有线程同步问题，也就是锁的实现原理）~~ |
| Recursive lock | 是mutex lock的变体，每一个线程可以多次获得锁。只有当一个线程的锁都被释放时，其他线程才能获得锁，多用于递归 |
| Read-write lock | 有线程读数据时不能写，有线程写数据时不能读。只通过POSIX threads支持 |
| Distributed lock | 进程级别提供互斥访问，并不直接阻塞进程，在锁忙碌时告知其他进程 |
| Spin lock | 轮询锁的状态而不直接阻塞线程，系统不提供任何形式的Spin lock |
| Double-checked lock | 潜在不安全，不建议使用 |

> 大多数锁结合了Memory Barriers，保证在进入关键代码之前的读写操作完成

#### Conditions

条件是信号量的一种，当特定条件为真时，线程间可以相互通发信号。条件大多数时候被用来表示资源的可用性或者保证任务按特定顺序执行。和mutex lock的区别是，conditions同时可能允许多个线程访问

条件的一种用法是管理未处理事件池。当队列中有事件时，队列通过条件变量告知等待的线程

#### Perform Selector Routines

每一个perform请求被放进目标线程的run loop，执行时按照进入队列的顺序

## Synchronization Costs and Performance

同步确保了代码的正确性，但是付出了性能上的代价。同步工具带来了延迟，即使在非竞争情况下也是如此。锁和原子操作一般会引入内存边界和内核同步来保护代码。如果有对锁的竞争，线程会被阻塞从而带来更大的延迟

## Thread Safety and Signals

信号是一种底层BSD机制，被用来向一个进程发送信息或者以某种方式操纵它

多线程app中，信号有可能被系统发送给任何线程

不能假设在一个线程上安装了信号处理器就一定会在这个线程上接收到信号

## Tips for Thread-Safe Designs

#### Avoid Synchronization Altogether

#### Understand the Limits of Synchronization

#### Be Aware of Threats to Code Correctness

#### Watch Out for Deadlocks and Livelocks

#### Use Volatile Variables Correctly

## Using Atomic Operations

## Using Locks

## Using Conditions



