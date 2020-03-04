# GIL锁

又名：全局解释锁 Global Interpreter Lock

> GIL并不是Python的特性，由解释器CPython引入，属于解释器层级的锁。GIL是为了锁定解释器内部的全局资源
>

一个进程仅持有一个GIL

### GIL释放的情况：

1. 线程执行完毕
2. I/O阻塞的时候
3. Python 3使用计时器，计时器到达阈值时

### 解决方案：

使用`mutilprocessing ` 多进程模块；

使用Python的胶水语言特性，在py文件中加载C语言文件：

​	将c文件编译为.so文件，在Py中使用多线程加载

