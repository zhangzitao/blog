---
categories: [tech, Dev]
tags: [go]
---

Fyne is a pure golang gui framework, which is easy to learn and use. 

I made a small gui app with Fyne. You need to install gcc if you want to use Fyne to build. The build speed is not very fast. 

Found a problem: if you define widgets.Entry outside the main function, there is no problem in building progress. But when you run app, nil pointer problem appear, wrong with goroutine.

    panic: runtime error: invalid memory address or nil pointer dereference
    [signal 0xc0000005 code=0x0 addr=0x50 pc=0x4f90a3]

    goroutine 1 [running, locked to thread]:

The xorm is also easy to use. But I think it's not very sutiable for uuid, you need transfer uuid to string. There are some weak points need to be improved.
