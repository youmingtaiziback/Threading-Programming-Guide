# About Threaded Programming

OS X利用多核处理系统事件，app也可以通过线程利用多核

## What Are Threads?

进程间时间轮转，线程间同时

线程是内核级和应用级数据结构的集合。内核负责分发事物给线程并安排线程的执行；应用级存储调用栈和管理线程的属性和状态

多线程能提高app响应速度并且同一时间处理更多任务，但也有一定的开销和安全性问题

## Threading Terminology

## Alternatives to Threads

| Technology | Description |
| :--- | :--- |
| Operation objects | 单独使用或者配合queue使用 |
| GCD | 封装的task放到queueu中执行 |
| Idle-time notifications | 适用于低优先级的任务，run loop空闲时会发通知 |
| Asynchronous functions | 系统创建线程处理任务 |
| Timers | 非主线程使用时需要runloop |
| Separate processes |  |

## Threading Support

### Threading Packages

threads底层实现是Mach threads，POSIX API使用起来更方便

| Technology | Description |
| :--- | :--- |
| Cocoa threads | NSThread、NSObject支持在已有的线程上执行代码 |
| POSIX threads | C接口、使用方便、灵活可配 |
| Multiprocessing Services | 只在OS X上支持并且不建议使用 |

线程创建的内存和时间成本相对较高，建议入口函数尽量多做些事情或者通过run loop使任务可以重新开始

#### Run Loops

run loop监听事件源，事件源来了，系统唤醒线程，把事件分发给run loop。没有事件需要处理时，run loop把线程置为休眠状态器、启动run loop

主线程的run loop由系统创建，创建子线程需要做的是：启动线程、获取run loop、添加事件处理

#### Synchronization Tools

Locks：使代码在任一时间最多只能被一个线程执行。最常见的是mutex lock，Cocoa提供来了几种mutex lock的变种

conditions

atomic operations：针对数据，硬件指令支持

#### Inter-thread Communication

线程共享进程空间

| Mechanism | Description |
| :--- | :--- |
| Direct messaging |  |
| Global variables, shared memory, and objects |  |
| Conditions |  |
| Run loop sources |  |
| Ports and sockets |  |
| Message queues（OS X only） |  |
| Cocoa distributed objects（OS X only） |  |

## Design Tips



