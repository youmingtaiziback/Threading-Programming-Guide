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

## Design Tips



