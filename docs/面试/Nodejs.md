## CPU密集 VS I/O密集

- CPU密集：压缩、解压、加密、解密
- I/O密集：文件操作、网络操作、数据库

## 为什么用Nodejs做web服务

- 前端职责范围变大，统一开发体验

- 在处理高并发、I/O密集场景性能优势明显

  可以解决高并发，它是单线程，当访问量很多时，将访问者分配到不同的内存中，不同的内存区做不同的事，以快速解决这个线程。就像医院的分科室看病人。效率快，但消耗内存大、异步和事件驱动。概扩起来就三点：单线程、异步I/O、事件驱动。

回调模式下的异步是有明显缺陷的，程序的执行顺序必须依靠回调来保证，没有层层回调，就没有可以保障的逻辑顺序，这也就注定了，node不能做复杂的业务逻辑。javascript语言本身也一直在和回调做斗争，promise，generator都可以将回调包装起来，在代码的某个部分形成形式同步，但是这种模式进化的还不完全，还不能做到与回调完全割裂，做到完全的形式同步。但是形式同步肯定是发展的方向，这种模式即可以获得异步的好处，又可以有效回避回调带来的编程困难，在业务逻辑上可以更简单的表达。

