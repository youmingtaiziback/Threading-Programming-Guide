# Thread Management {#pageTitle}

## Thread Costs

每一个线程的创建需要：内核内存空间提供管理和协调线程的核心数据结构，应用内存空间线程栈和数据

一个线程的大致时空开销

| 线程类型 | 时空开销 |
| :--- | :--- |
| Kernel data structures | 1KB |
| Stack space | 主线程 1M，子线程512KB |
| 创建 | 90微秒 |

Operation Objects使用了线程池，创建线程要快些

## Creating a Thread

#### Using NSThread

detachNewThreadSelector:toTarget:withObject:、创建NSThread并调用start方法

当detached thread结束时，系统自动回收他的资源

performSelector:onThread:withObject:waitUntilDone: 需要线程有runloop，执行后立即撤销

#### Using POSIX Threads

#### Using NSObject to Spawn a Thread

#### Using POSIX Threads in a Cocoa Application

## Configuring Thread Attributes

## Writing Your Thread Entry Routine

## Terminating a Thread



