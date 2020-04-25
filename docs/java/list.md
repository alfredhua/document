## ArrayList：
ArrayList a=new ArrayList();
a.add("a");
a.get(10);

- add：ArrayList在添加的时候会增加数组的大小，与10比较，如果小于10 则选择10，然后就是正常的数组copy，扩容。

- get：直接返回数组对应的值即可。


## LinkedList:

- add方法：直接往最后一个节点上存放节点即可。
- get方法：
如果index的位置比size>>1小，则从0开始到index位置查找，反之，从最后一个节点反向寻找。
```
LinkedList linkedList=new LinkedList();
linkedList.add("a");
linkedList.get(1);

//add:
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null)
        first = newNode;
    else
        l.next = newNode;
    size++;
    modCount++;
} 

//get:
Node<E> node(int index) {
    // assert isElementIndex(index);

    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```


- 安全性：ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全;
- 底层数据结构：Arraylist 底层使用的是Object数组;LinkedList 底层使用的是双向链表数据结构(JDK1.6之 前为循环链表，JDK1.7取消了循环。注意双向链表和双向循环链表的区别。

- 插入和删除是否受元素位置的影响: 
1. ArrayList 采用数组存储，所以插入和删除元素的时间复杂度受元素 位置的影响。比如:执行add(E e)方法的时候，ArrayList会默认在将指定的元素追加到此列表的末尾，这种 情况时间复杂度就是O(1)。但是如果要在指定位置i插入和删除元素的话(add(int index, E element))时 间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向 前移一位的操作。
2. LinkedList 采用链表存储，所以插入，删除元素时间复杂度不受元素位置的影响，都是 近似 O(1)而数组为近似 O(n)。

- 是否支持快速随机访问:LinkedList 不支持高效的随机元素访问，而 ArrayList 支持。快速随机访问就是通 过元素的序号快速获取元素对象(对应于get(int index)方法)。
- 内存空间占用: ArrayList的空 间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空 间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间(因为要存放直接后继和直接前驱以及数 据)。