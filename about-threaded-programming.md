# About Threaded Programming

OS X处理系统事件时利用多核，app也可以通过线程利用多核

## What Are Threads?

进程间时间轮转，线程间同时运行。系统管理线程的运行，安排他们在可用的核上运行并根据需要停止线程

线程是内核级和应用级数据结构的组合。内核负责分发事物给线程并安排线程的执行；应用级存储调用栈和管理线程的属性和状态

多线程能提高app响应速度并且同一时间处理更多任务，但也有一定的开销和线程安全问题

## Threading Terminology

* thread：代码的一条执行路径
* process：运行的可执行文件，可包括多个线程
* task：需要完成的抽象任务

## Alternatives to Threads

下表列出了线程的替代技术和如果高效的利用单一线程

| Technology | Description |
| :--- | :--- |
| Operation objects | 单独使用或者配合queue使用 |
| GCD | 封装的task放到queueu中执行 |
| Idle-time notifications | 适用于低优先级的任务，向[NSNotificationQueue](https://developer.apple.com/documentation/foundation/nsnotificationqueue)发送一个带有[NSPostWhenIdle](https://developer.apple.com/documentation/foundation/notificationqueue.postingstyle/1418001-whenidle)选项的通知，系统会延迟传播通知直到run loop变的空闲时。[_Notification Programming Topics_](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html#//apple_ref/doc/uid/10000043i) |
| Asynchronous functions | 系统创建线程处理任务 |
| Timers | 非主线程使用时需要runloop。[Timer Sources](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW21) |
| Separate processes |  |

## Threading Support

### Threading Packages

threads底层实现是Mach threads，POSIX API使用起来更方便

**Table 1-2 **Thread technologies

| Technology | Description |
| :--- | :--- |
| Cocoa threads | [NSThread](https://developer.apple.com/documentation/foundation/thread)、[NSObject](https://developer.apple.com/documentation/objectivec/nsobject)支持在已有的线程上执行代码。[Using NSThread](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW11)、[Using NSObject to Spawn a Thread](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW13) |
| POSIX threads | C接口、使用方便、灵活可配。[Using POSIX Threads](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW12) |
| Multiprocessing Services | 只在OS X上支持并且不建议使用 |

线程启动后，在运行、就绪、阻塞三个状态之间切换直到终止状态

线程创建的内存和时间成本相对较高，建议入口函数尽量多做些事情或者通过run loop使任务可以重新开始

#### Run Loops

run loop监听事件源，事件源来了，系统唤醒线程，把事件分发给run loop。没有事件需要处理时，run loop把线程置为休眠状态

没有事件需要处理时，runloop将线程置为休眠状态，从而减少了轮询，节省了CPU时间

主线程的run loop由系统创建，创建子线程需要做的是：启动线程、获取run loop、添加事件处理、启动run loop

[Run Loops](/run-loops.md)

#### Synchronization Tools

Locks：使代码在任一时间最多只能被一个线程执行。最常见的是mutex lock，Cocoa提供来了几种mutex lock的变种

conditions：保证了app内任务的执行顺序。POSIX和Foundation都提供了对conditions的直接支持

atomic operations：针对数据，硬件指令支持

[Synchronization Tools](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-124887)

#### Inter-thread Communication

线程共享进程空间

按复杂度递增顺序的线程间通信

| Mechanism | Description |
| :--- | :--- |
| Direct messaging | 被发送到目标线程的selector按照顺序执行，在一个runloop里面执行所有的selector |
| Global variables, shared memory, and objects | 共享变量快速简单，容易产生线程安全问题，需要同步机制 |
| Conditions | [Using Conditions](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW4) |
| Run loop sources | Custom run loop source |
| Ports and sockets | 用run loop source实现port，可以使线程闲时休眠 |
| Message queues（OS X only） | _Multiprocessing Services Programming Guide_ |
| Cocoa distributed objects（OS X only） | 对port-based通信的高层实现，多用于进程间通信。_Distributed Objects Programming Topics_ |

## Design Tips

以下建议可以提高代码的正确性和效率

#### 避免直接创建线程

使用异步API、Operation Objects或者GCD，使用后两者时，系统会根据实时系统负载调整线程数量。[_Concurrency Programming Guide_](https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)

#### 合理的保持线程忙碌

如果手动创建和管理线程，应合理的保持线程长期存活并高产出。对于大量消耗空闲时间的线程应该及时终止，这样可以节省内存

#### 避免共享数据结构

尽量减少资源竞争可以使设计更简单效率更高

#### 线程和UI

在主线程接受事件更新UI可以简化管理UI的逻辑

关于绘制的更多信息：[_Cocoa Drawing Guide_](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)

#### 了解线程退出时的行为

进程结束时，关键操作应该用non-detached（joinable）线程

高层线程技术默认创建的是detached线程，用POSIX API可以创建non-detached线程。创建non-detached线程：[Setting the Detached State of a Thread](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW3)

在Cocoa app中，也可以用`applicationShouldTerminate:`延迟进程结束，重要任务结束后需要调用`replyToApplicationShouldTerminate:`

#### 处理异常

异常处理机制依赖于当前的call stack执行清理工作。因为每一个线程有自己的call stack，所以线程需要自己处理自己的异常。线程不能处理自己的异常的话，进程就会停止

线程不能将自己的异常扔给其他线程处理，但可以通知其他线程

有时异常处理器会被自动创建，例如@synchronized

#### 干净的结束线程

最好的结束线程方式是让线程执行到结尾，否则释放资源的代码可能执行不到，造成内存泄漏

#### 程序库里面的线程安全

设计库时应该考虑库在多线程环境被调用

如果想让程序库在app变成多线程时被通知，可以监听[NSWillBecomeMultiThreadedNotification](https://developer.apple.com/documentation/foundation/nswillbecomemultithreadednotification)

