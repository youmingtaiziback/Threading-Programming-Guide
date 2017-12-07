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
| Conditions |  |
| Run loop sources |  |
| Ports and sockets |  |
| Message queues（OS X only） |  |
| Cocoa distributed objects（OS X only） |  |

## Design Tips

#### 避免直接创建线程，使用Operation Objects或者GCD

#### 线程占用一定的内存，使用时尽量保持长时间运行，不用时及时停止

#### 避免共享数据结构

#### 主线程接受事件并更新UI

#### 进程结束时，关键操作应该用non-detached（joinable）线程，在Cocoa app中，也可以用applicationShouldTerminate:

#### 线程不能处理自己的异常的话，进程就会停止。线程不能将自己的异常扔给其他线程处理，但可以通知其他线程。@synchronized指令包含了一个隐士异常处理器

#### 最好的结束线程方式是让线程执行到结尾，否则释放资源的代码可能执行不到，造成内存泄漏

#### 设计库时应该考虑库在多线程环境被调用。NSWillBecomeMultiThreadedNotification



