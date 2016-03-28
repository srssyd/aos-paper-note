# Non-scalable locks are dangerous 阅读笔记

本篇文章有以下几个主要的贡献：

* 说明了Non-scalable locks会导致性能的急剧下降，即使临界区特别短
* 提出了一个基于Markov的模型，解释了ticket lock的性能表现
* 分析了各种scalable lock的性能

### 现有的锁机制的scalability的问题

作者通过测试多种API，得到了结论。在最开始，随着核数的增加，性能会提升。但是，当核数到达一定程度时，只要增加一点点核，性能就会急剧地下降。即使临界区中的指令数很少，在核数比较少的时候性能也会急剧下降。

### 基于Markov的模型

这个模型基于现有的intel XEON和AMD的cache coherence的协议。每一个cache line有3种状态：I代表没有被cache；S代表这个cache line和内存中的内容一致，而且1个或多个CPU核拥有这个cache line；M代表一个CPU核拥有这个cache line，而且这个cache line中的数据和内存中的不一致。每当进行相应的读或者取操作时，这些状态之间会相互转换。

在本文中，设计了一个马尔科夫模型。在这个模型中，每一个状态即为当前的ticket的值。设n为CPU核数，a为单核CPU连续两次Lock之间时间的平均值，k为正在等待的CPU核的个数,c为对一个cache line请求所需的时间。那么，从第k个状态转移到第k+1个的状态的概率为(n-k)/a，从第k+1个状态转移会第一个状态的概率为1/(s+ck/2)。 则从以上式子我们就可以推倒得到论文中的那个式子。

经过验证，这个模型能够很好的解释实际中的情况。


### scalable-lock

以MCS Lock为首的一些Lock，及其变种，相比于Ticket lock，提供了更好的scalability。这些Lock，都通过维持一个队列来实现。

最后通过实验发现，这些Lock相比ticket lock拥有更好的scalability。

