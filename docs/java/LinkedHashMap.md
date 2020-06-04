<img src="http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/d77e4b7c8c044cc1a365e43386c64d83.png" alt="image" style="zoom:50%;" />

第一张图为LinkedHashMap整体结构图

![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/a1469e9607564accb59708d719a2ad4b.png)

第二张图专门把循环双向链表抽取出来，直观一点，注意该循环双向链表的头部存放的是最久访问的节点或最先插入的节点，尾部为最近访问的或最近插入的节点，迭代器遍历方向是从链表的头部开始到链表尾部结束，在链表尾部有一个空的header节点，该节点不存放key-value内容，为LinkedHashMap类的成员属性，循环双向链表的入口。

