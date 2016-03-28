# Scalable Kernel TCP Design and Implementation for Short-Lived Connections 阅读笔记

这篇文章提出了Fastsocket，一个处在内核态的与Socket接口兼容的TCP协议栈。其在24核CPU上能够实现20.4倍的加速比，性能要远超过linux的实现。

### 现有的TCP的实现的瓶颈

目前TCP的实现中有三个瓶颈：
* TCB的管理开销。由于Global Listen Table和Global Established Table的存在，有大量的锁的开销。
* 缺乏connection locality。对于一个connection，进出的包有可能在不同的CPU上被处理,会导致性能下降。
* VFS文件系统带来大量的开销。特别是对inodes和dentries的管理，会带来大量的同步开销。

### FastSocket的设计

FastSocket的设计大概如下：
1. 将global的TCB的表根据CPU完全划分开来。
2. 使用Receive Flow Deliver来将包送往所对应的CPU。
3. 让VFS认识到Fastsocket的存在，并因此减掉不必要的开销。

### FastSocket的实现细节

每个CPU有自己对应的Listen Table,当然也有global的table。正常情况下，socket都直接通过自己本地所对应的Listen Table来反应。但是，当一个Process crash了或者其他情况时，就会访问global的table来处理对应的逻辑。

至于RFD,在根据包的端口进行分类之后，通过网卡的ATR来对包进行过滤和分发。

对于VFS,我们可以知道dentry和inode数据结构对于socket是没有很多作用的，于是Fastsocket跳过了很多denty和inode的初始化过程。当然，为了保持对proc文件系统的兼容，一部分的功能还是留了下来。


