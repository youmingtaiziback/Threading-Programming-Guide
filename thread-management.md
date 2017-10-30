# Thread Management {#pageTitle}

## Thread Costs

每一个线程的创建需要：内核内存空间提供管理和协调线程的核心数据结构，应用内存空间线程栈和数据

一个线程的大致时空开销

| 线程类型 | 时空开销 |
| :--- | :--- |
| Kernel data structures | 1KB |
| Stack space | 主线程 1M，子线程512KB |
| 创建 | 90微秒 |

## Creating a Thread

## Configuring Thread Attributes

## Writing Your Thread Entry Routine

## Terminating a Thread



