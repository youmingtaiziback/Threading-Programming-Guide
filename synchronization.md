# Synchronization {#pageTitle}

## Synchronization Tools

#### Atomic Operations

不阻塞竞争的线程

#### Memory Barriers and Volatile Variables

优化过程中，编译器重新排列汇编级指令，但有可能会出现错误结果

Memory Barriers是非阻塞工具，内存围栏前面的内存操作总是先于内存围栏后面的操作，OSMemoryBarrier

Volatile variables，每次使用变量时从内存加载而不是从寄存器

memory barriers和volatile variables会减少编译器带来的性能优化

## Synchronization Costs and Performance

## Thread Safety and Signals

## Tips for Thread-Safe Designs

## Using Atomic Operations

## Using Locks

## Using Conditions



