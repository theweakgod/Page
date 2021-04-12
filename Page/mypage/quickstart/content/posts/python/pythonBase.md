---
title: "python基础学习"
date: 2021-04-12T12:56:36+08:00

lastmod: 2021-04-12T12:56:50+08:00
draft: false
author: "mxw"
authorLink: "http://www.mengcfunk.com"
description: ""
license: ""
images: []

tags: ["python基础"]
categories: ["python基础","多线程"]
featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  # ...
math:
  enable: true
  # ...
mapbox:
  accessToken: ""
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---
懒人回归，开始写一下所学，怕忘记，好记性不如烂笔头。


python被称为解释性语言，那么他是如何实现这种解释的呢？python解释之后是怎么运行的呢？他是怎么实现一些基本操作的？

#### python的运行
python是怎么运行的呢

执行一个 .py 文件会经历如下过程

<img src='/images/ceval.png'>

如图所示，python是有编译的，所有的python源文件都会在内存中被编译器翻译成由`opcode`组成字节码指令集, 而 `import`目录下会被保存成`.pyc`后缀名的文件，并缓存在执行目录，下次启动程序如果源代码没有修改过，则直接加载这个pyc文件，`opcode`可以说是一条字节码指令，这种字节码指令都是一个整体，无法分割。

在python虚拟机中，解释器主要在一个很大的循环中，不停地读入 `opcode`, 并根据`opcode`执行对应的指令，当执行完所有指令虚拟机退出，程序也就结束了。


``` c
main_loop:
    for (;;) {
        ...
            
        switch (opcode) {
 
        /* BEWARE!
           It is essential that any operation that fails must goto error
           and that all operation that succeed call [FAST_]DISPATCH() ! */
 
        case TARGET(NOP): {
            FAST_DISPATCH();
        }
 
        case TARGET(LOAD_FAST): {
            PyObject *value = GETLOCAL(oparg);
            if (value == NULL) {
                format_exc_check_arg(PyExc_UnboundLocalError,
                                     UNBOUNDLOCAL_ERROR_MSG,
                                     PyTuple_GetItem(co->co_varnames, oparg));
                goto error;
            }
            Py_INCREF(value);
            PUSH(value);
            FAST_DISPATCH();
        }
 
        case TARGET(LOAD_CONST): {
            PREDICTED(LOAD_CONST);
            PyObject *value = GETITEM(consts, oparg);
            Py_INCREF(value);
            PUSH(value);
            FAST_DISPATCH();
        }
        ...
    }
}
```
我们来简单看下 LOAD_CONST 这个指令, 在上面的 switch case 里面。

可以看到，这个指令通过 `GETITEM` 从 `oparg`中获取到了一个 python 对象的指针，这个指针的类型是  `PyObject *` 

`Py_INCREF` 作用是把这个 `PyObject * `对象的引用计数器加一, 关于引用计数器可以参考

PUSH 的作用是把这个刚刚创建的  `PyObject *`  push 到 当前的 frame 的 stack 上面，以便下一个指令从这个 stack 上面获取
##### GIL(全局解释锁)
什么是全局解释锁？首先python多线程在运行时，会有一把锁，每个线程都会想要获得这把锁来运行。但CPython并不会允许一个线程独占解释器，它会轮流执行各个线程，这就是我们平时说的伪并行。

<img src='/images/python_GIL_thread_run.png'>

上面这张图，就是 GIL 在 Python 程序的工作示例。其中，Thread 1、2、3 轮流执行，每一个线程在开始执行时，都会锁住 GIL，以阻止别的线程执行；同样的，每一个线程执行完一段后，会释放 GIL，以允许别的线程开始利用资源。

读者可能会问，为什么 Python 线程会去主动释放 GIL 呢？毕竟，如果仅仅要求 Python 线程在开始执行时锁住 GIL，且永远不去释放 GIL，那别的线程就都没有运行的机会。其实，CPython 中还有另一个机制，叫做间隔式检查（check_interval），意思是 CPython 解释器会去轮询检查线程 GIL 的锁住情况，每隔一段时间，Python 解释器就会强制当前线程去释放 GIL，这样别的线程才能有执行的机会。

注意，不同版本的 Python，其间隔式检查的实现方式并不一样。早期的 Python 是 100 个刻度（大致对应了 1000 个字节码）；而 Python 3 以后，间隔时间大致为 15 毫秒。当然，我们不必细究具体多久会强制释放 GIL，读者只需要明白，CPython 解释器会在一个“合理”的时间范围内释放 GIL 就可以了。

整体来说，每一个 Python 线程都是类似这样循环的封装，来看下面这段代码：
``` c
for (;;) {
    if (--ticker < 0) {
        ticker = check_interval;   
        /* Give another thread a chance */
        PyThread_release_lock(interpreter_lock);
        /* Other threads may run now */   
        PyThread_acquire_lock(interpreter_lock, 1);
    }
    bytecode = *next_instr++;
    switch (bytecode) {
        /* execute the next instruction ... */
    }
}
```
从这段代码中可以看出，每个 Python 线程都会先检查 ticker 计数。只有在 ticker 大于 0 的情况下，线程才会去执行自己的代码。

##### 什么是原子性操作
原子操作(atomic operation)指不会被线程调度机制打断的操作，这种操作一旦开始，就一直运行到结束，中间不会切换到其他线程。

我们来看看python在这方面怎么做的。
首先看看a+=1编译之后的代码
<img src="/images/python_a.png">
其中
LOAD_NAME 将 a 当前的值加载进自己的运行栈；

LOAD_CONST 将常量 1 加载到运行栈；

INPLACE_ADD 对栈上两个操作数进行加法运算；

STORE_NAME 将计算结果保存；

下面看看`test.append('1')`
<img src="/images/python_list_append.png">
对于a+=1来说，这个操作并不是原子性的，例如，某个线程test_1拿到了a的数据，进行自增之后，加到了2，此时正好时间片走完，这些正好在INPLACE_ADD操作之后切换线程到了test_2，test_2也是读取数据自增到二，如果两个线程都只运行一次自增，那么a的结果一定会是2。这就是并发操作产生的竞争态。

但是`test.append('1')`是原子性操作，为什么呢，因为在列表中，写入并不会存在竞争，不会存在脏数据的可能。

#### 摘自：
[python有了GIL为什么还需要加锁？](https://wangzun.dev/article/2/) \
[python 源码分析 基本篇](https://blog.csdn.net/qq_31720329/article/details/86751412) \
[Python GIL全局解释器锁详解（深度剖析）](http://c.biancheng.net/view/5537.html)