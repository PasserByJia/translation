# 深入理解现代浏览器（part 1）

> 翻译： 路人贾
>
> 文章来源： https://developers.google.com/web/updates/2018/09/inside-browser-part1#at_the_core_of_the_computer_are_the_cpu_and_gpu

## CPU,GPU,内存和多进程架构

​		在这四部分博客中，我们将从高级架构到渲染管道细节方面深入Chrome浏览器。如果你曾经想知道浏览器如何将你的代码转换成功能性网站，或者你不确定为什么建议使用特定的技术来提高性能，这四部分博客将适合你。

​		作为系列的第一部分，我们将着眼于核心计算术语与Chrome多进程架构。

> Note:如果你熟悉CPU / GPU和进程/线程的概念，你可以跳至浏览器架构章节。

## 计算机的核心是CPU和GPU

​		为了理解浏览器的运行环境，我们需要了解一些计算机的组成部分以及他们是如何工作的。

### CPU

![CPU](https://developers.google.com/web/updates/images/inside-browser/part1/CPU.png)

> 图1：CPU的四颗核心作为办公室里的员工坐在桌子前处理传入的任务

​		首先是中央处理单元也叫CPU。CPU可以看做你的计算机的大脑。一个CPU核心被描绘成办公室员工处理一个又一个不同的任务。它可以处理从数学到艺术的任何问题，同时也知道如何给客户回电话。在过去大多数CPU是单独的一个芯片。而核心就相当于生活在同一个芯片上的另一个CPU。在现代计算机硬件上，你通常会获得多个CPU核心，给予你的手机和笔记本更强的性能。

### GPU

![GPU](https://developers.google.com/web/updates/images/inside-browser/part1/GPU.png)

> 图2：许多带着扳手的GPU核心表明他们只能处理有限的任务。

​		图形处理单元或者叫GPU是组成计算机的另一部分。不像CPU，GPU更擅长同时跨越多个内核的处理简单任务。正如其名字所示，它最初是为了处理图形而开发的。这就是为什么在图形环境中使用GPU或者GPU支持会与快速渲染和流畅交互联系在一起。近年来随着GPU加速计算，更多的计算在GPU上单独进行变成了可能。

​		当你在电脑或者手机上启动一个应用时，CPU和GPU将为应用的运行提供支持。通常应用程序是使用操作者系统提供的机制在CPU和GPU上运行的。

![Hardware, OS, Application](https://developers.google.com/web/updates/images/inside-browser/part1/hw-os-app.png)

> 图3：计算机的三层架构。计算机硬件在底层，操作系统在中间，应用在顶层。

## 在线程和进程上执行程序

![process and threads](https://developers.google.com/web/updates/images/inside-browser/part1/process-thread.png)

> 图4：进程像一个鱼缸，线程向是在进程中游动的抽象的鱼。

