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

c接口，使用更方便

默认是non-detached thread

#### Using NSObject to Spawn a Thread

`performSelectorInBackground:withObject:`

#### Using POSIX Threads in a Cocoa Application

在framework中用POSIX实现多线程时，Cocoa无法得知什么时候开始使用多线程。解决办法是生成一个NSThread然后立即退出。\[NSThread isMultiThreaded\]

Cocoa的锁和条件对象是对POSIX相关对象的封装

## Configuring Thread Attributes

#### Configuring the Stack Size of a Thread

| Technology | Option |
| :--- | :--- |
| Cocoa | \[NSThread setStackSize:\]; |
| POSIX | pthread\_attr\_setstacksize =&gt; pthread\_attr\_t =&gt; pthread\_create |
| Multiprocessing Services |  |

#### Configuring Thread-Local Storage

Cocoa: \[NSThread threadDictionary\]

POSIX: pthread\_setspecific、pthread\_getspecific

#### Setting the Detached State of a Thread

Most high-level thread technologies create detached threads by default

POSIX creates threads as joinable by default

You can think of joinable threads as akin to child threads

#### Setting the Thread Priority

Cocoa: \[NSThread setThreadPriority:\]

POSIX: pthread\_setschedparam

最好使用默认的线程优先级

## Writing Your Thread Entry Routine

#### Creating an Autorelease Pool

每个线程至少有一个autorelease pool

ARC：autorelease pool被忽略；MRC：autorelease pool在入口函数最开始生成，最后释放

适当的增加autorelease pool有利于减少内存

#### Setting Up an Exception Handler

[_Exception Programming Topics_](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Exceptions/Exceptions.html#//apple_ref/doc/uid/10000012i)

#### Setting Up a Run Loop

两类线程：有run loop和没有run loop的

## Terminating a Thread

 Cocoa、POSIX和Multiprocessing Services提供了直接杀死线程的接口，但是不建议使用，这样做很可能导致内存泄漏

