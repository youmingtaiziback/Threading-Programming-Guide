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
| Mutex | lock是信号量的一种 |
| Recursive lock | 是mutex lock的变体，每一个线程可以多次获得锁。只有当一个线程的锁都被释放时，其他线程才能获得锁，多用于递归 |
| Read-write lock | 有线程读数据时不能写，有线程写数据时不能读。只通过POSIX threads支持 |
| Distributed lock | 进程级别提供互斥访问，并不直接阻塞进程，在锁忙碌时告知其他进程 |
| Spin lock |  |
| Double-checked lock |  |

```
Read-write lock（shared-exclusive lock），有线程读数据时不能写，有线程写数据时不能读。只通过POSIX threads支持

Distributed lock，进程级别提供互斥访问，并不直接阻塞进程，在锁忙碌时告知其他进程

Spin lock，轮询锁的状态而不直接阻塞线程，系统不提供任何形式的Spin lock

Double-checked lock，潜在不安全，不建议使用

大多数锁结合了Memory Barriers
```

## Synchronization Costs and Performance

## Thread Safety and Signals

## Tips for Thread-Safe Designs

## Using Atomic Operations

## Using Locks

## Using Conditions



