## IO

1. BIO 阻塞的IO，每线程每连接。

   优势： 可以同时接收很多连接。

   劣势： 线程内存浪费，CPU调度消耗（accpet recv系统操作）。
   
2. NIO new IO，nonblocking IO
    优势：解决了 BIO多线程的劣势，使一个线程就能完成所有连接的accpet和recv操作。
    
    劣势：C10K问题 极端情况：有1w个连接，但只有一个连接有数据，单线程需要向内核发送1w次recv操作，其种9999次都是无意义的资源消耗。

3. 多路复用器 select poll

   优势：解决 NIO C10K的问题，一次将多个fd传递给内核由内核来遍历以减少系统调用次数。

   劣势：1. 重复传递fds，每次都要重复把fd传递给内核。 2. 每次select poll都要全量遍历fd。
   
4. 多路复用器 epoll
   优势：在内核开辟空间保存fd，fd被创建出来后放入该控件内，通过epoll_wait 监听fd状态变化。
   

   

   
   

  

  
