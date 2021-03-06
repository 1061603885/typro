### 理解javascript事件执行机制

​       众所周知，js是一个单线程的语言，这意味着同一时间只能做一件事，但是我们又说js是异步的。首先，单线程并不是没有优点。作为浏览器脚本语言，JavaScript 的主要用途是与用户互动，以及操作 DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript 同时有两个线程，一个线程在某个 DOM 节点上添加内容，另一个线程删除了这个节点，那最后应该以哪个为准呢？  所以，为了避免复杂性，从一诞生，JavaScript 就是单线程，这已经成了这门语言的核心特征，将来也不会改变。 

​    实际上，主线程只会做一件事情，就是从消息队列里面取消息、执行消息，再取消息、再执行。当消息队列为空时，就会等待直到消息队列变成非空。而且主线程只有在将当前的消息执行完成后，才会去取下一个消息。这种机制就叫做事件循环机制，取一个消息并执行的过程叫做一次循环 。就如图所示一样



![1571118784222](C:\Users\小win\AppData\Roaming\Typora\typora-user-images\1571118784222.png)

  如果说js只有一个主线程，那么它应该有三个辅助的子线程，分别为事件处理线程、http网络请求线程、定时器处理线程。这些线程就是实现js异步的关键，比如主线程内有程序正在运行，这个时候后面有一个定时器在等待，那么主线程肯定不会检测这个定时器的时间是否达到要求的，这样会消耗性能和时间，所以就交给定时器处理线程，当setTimeout的时间达到时，它就会把这个定时器里的函数（其实这就是回调函数了）放到任务队列里，当主线程把执行栈中的任务都执行完以后，执行栈为空了，就会从任务队列里找，执行里面的回调，如此循环往复，这就是时间循环。由于执行任务还是只有一个主线程可以做，所以有时候即使定时器触发的事件已经到了，但是它的回调函数也只能在任务队列中等待，这导致最后函数触发的事件往往比设置的时间长，这也是我们说定时器准确度不高的原因。