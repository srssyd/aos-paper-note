# IX: A Protected Dataplane Operating System for High Throughput and Low Latency 阅读笔记

为了在维持OS的保护机制的前提下提升操作系统的I/O性能，Adam Belay等人在实现了IX。IX的I/O性能相对于linux有了极大的提高。IX在拥有极高的吞吐量的同时，也拥有很低的延时。

### IX的设计

IX为了提升性能，用了以下的方法：
* 将内核分为control plane和data plane。control plane主要用于资源调度，监控等任务，而data plane 负责运行网络协议栈。
* 在拥塞控制的时候使用batch
*  使得网络协议能够从头运行到底，节省了系统调用的开销，同时使得系统能够充分利用cache。
*  使用自己特有的API，实现了数据的zero-copy
*  对于每个线程维持自己本地特有的池，减少了同步的开销
*  另外，IX使用了RCU来提高多线程访问时的读性能。

同时，在IX中，分为Elastic线程和background线程，elastic线程上的所有操作都是non-blocking的。


在安全性方面，IX中application的代码都运行在ring3 中，data plane本身运行在non root 模式的ring 0中。


### IX的性能
经过评测，在网络带宽足够大的时候，IX的吞吐量远胜于linux，同时也高于mTCP等user space的network stack。