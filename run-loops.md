# Run Loops {#pageTitle}

有事时保持线程忙碌，没事时让线程休眠； Cocoa 和 Core Foundation都提供run loop对象，每个线程有一个对应的run loop

## Anatomy of a Run Loop

![](/assets/import.png)

只能使用Core Foundation添加run-loop的观察者

#### Run Loop Modes

mode可自定义，利用mode可以过滤一些事件

| Mode | Name | Description |
| :--- | :--- | :--- |
| Default | NSDefaultRunLoopMode/kCFRunLoopDefaultMode |  |
| Connection | NSConnectionReplyMode | 开发者基本不用 |
| Modal | NSModalPanelRunLoopMode |  |
| Event tracking | NSEventTrackingRunLoopMode |  |
| Common modes | NSRunLoopCommonModes/kCFRunLoopCommonModes | 是一组可配置modes。Cocoa中默认包含default、modal和event tracking modes；Core Foundation默认只包含default mode，可通过[CFRunLoopAddCommonMode](https://developer.apple.com/documentation/corefoundation/1542137-cfrunloopaddcommonmode)修改 |

#### Input Sources

* Port-Based Sources：内核触发

  * Cocoa：as is

  * Core Foundation：手动创建port和对应的run loop source

* Custom Input Sources：其他线程手动触发，用Core Foundation的接口创建

* Cocoa Perform Selector Sources：Cocoa的Custom Input Source，运行后从run loop删除。每一个run loop处理所有selector

#### Timer Sources

对于重复的timer，如果延迟触发的过程中经过了多个时间间隔，也只会触发一次

#### Run Loop Observers

观察者可监听的事件通知：

* 进入runloop
* 即将处理timer
* 即将处理input source
* 即将休眠
* 被唤醒
* 退出runloop

观察者可以被单次或重复使用

#### The Run Loop Sequence of Events

1. Notify observers that the run loop has been entered.

2. Notify observers that any ready timers are about to fire.

3. Notify observers that any input sources that are not port based are about to fire.

4. Fire any non-port-based input sources that are ready to fire.

5. If a port-based input source is ready and waiting to fire, process the event immediately. Go to step 9.

6. Notify observers that the thread is about to sleep.

7. Put the thread to sleep until one of the following events occurs:

   * An event arrives for a port-based input source.

   * A timer fires.

   * The timeout value set for the run loop expires.

   * The run loop is explicitly woken up.

8. Notify observers that the thread just woke up.

9. Process the pending event.

   * If a user-defined timer fired, process the timer event and restart the loop. Go to step 2.

   * If an input source fired, deliver the event.

   * If the run loop was explicitly woken up but has not yet timed out, restart the loop. Go to step 2.

10. Notify observers that the run loop has exited.

添加一个非port的input source可以立即唤醒runloop，input source可以被立即执行

## When Would You Use a Run Loop?

主线程的run loop有系统自动创建

子线程在以下情况下需要使用run loop：

* 用port或者自定义input source和其他线程通信
* 使用定时器
* 使用performSelector…
* 保活周期性处理任务

## Using Run Loop Objects

#### Getting a Run Loop Object

获取当前线程runloop的方法

* \[Cocoa currentRunLoop\]
* CFRunLoopGetCurrent

NSRunLoop和CFRunLoopRef之间可相互转换

#### Configuring the Run Loop

run loop如果不添加timer或者input source，运行后会立即退出

添加run loop的observer只能用Core Foundation

#### Starting the Run Loop

无条件启动run loop: 最不建议使用，唯一停止run loop的方法是杀死他，没办法指定mode

以固定时间启动: 时间到了或者有事件发生时run loop结束

以特定mode启动: 与时间启动不冲突，可同时使用

#### Exiting the Run Loop

* 设置时间段：推荐，run loop会正常处理事务，包括发通知
* 主动停止run loop：CFRunLoopStop，作用同上，可以停止无条件启动的run loop

#### Thread Safety and Run Loop Objects

## Configuring Run Loop Sources



