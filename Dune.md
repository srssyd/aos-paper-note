# Dune: Safe User-level Access to Privileged CPU Features 阅读笔记

为了使普通的应用程序能够直接访问CPU的特权指令，以方便一些应用程序，如垃圾回收器，沙盒等程序的设计，同时为了减小这些程序进行系统调用时的开销，Adam Belay等人设计了Dune。Dune能够在保证安全的同时，让应用程序能够直接访问硬件的特权指令，进而简化了这些程序的设计，并提升了性能。同时Dune还保留了对操作系统的系统调用访问的接口。

### Dune的设计

为了使进程在访问CPU的特权指令的同时也能保证安全，Dune利用了CPU的虚拟化技术，如intel的VT-X 指令。实际实现中，Kernel运行在root,ring 0模式，普通的进程运行在root, ring 3模式，而Dune所开启的进程运行在non root，ring 0模式。Dune的进程可以使用CPU的特权指令，但是这些指令由VT-x所模拟，不会影响到其他进程。同时，DUNE使用了EPT，使得Dune进程在能够操作页表的同时，拥有独立地址空间。

系统调用则是通过VMCALL指令来实现的，这样可以调用kernel本来的系统调用。同时，为了性能考虑，Dune限制了一些指令的运行。


### Dune的性能
因为VMCALL指令的性能开销要大于SYSCALL，所以Dune在执行一些kernel的系统调用时性能会有一些下降。但是，由于Dune能够直接访问特权指令，很多操作省去了陷入内核等等操作，进而使得对于一些应用性能有了大幅度的提高。