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
| Common modes | NSRunLoopCommonModes/kCFRunLoopCommonModes | 是一组可配置modes。 |

#### Input Sources

#### Timer Sources

#### Run Loop Observers

#### The Run Loop Sequence of Events

## When Would You Use a Run Loop?

## Using Run Loop Objects

## Configuring Run Loop Sources



