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

观察者可以被单词或重复使用

#### The Run Loop Sequence of Events

## When Would You Use a Run Loop?

## Using Run Loop Objects

## Configuring Run Loop Sources



