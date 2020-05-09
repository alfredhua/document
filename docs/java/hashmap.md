- HashMap的put方法：

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```


 hash方法，计算出该存放的在数组中的位置（除以16求余道理一样）：

hashCode() 方法用于返回字符串的哈希码。
字符串对象的哈希码根据以下公式计算：
s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]

使用 int 算法，这里 s[i] 是字符串的第 i 个字符，n 是字符串的长度，^ 表示求幂。空字符串的哈希值为 0。

 h无符号位右移动 16位，相当于获取高16位，低16位舍去，
与h进行异或运算，则一定获取的是一个32位的数字。
```java
//h无符号位右移动 16位与h进行异或运算
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```


- putVal方法：

    tab[ (n - 1) & hash]：n是tab的长度，则： (n - 1) & hash：一定是一个小于n的数字。
```java

//肯定是一个小于n的数。

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
    //首次进来初始化tab大小， resize()是初始化tab的大小，确定阈值。
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
    //第一次放入value，Node在tab的第i个位置上
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
           //往红黑树中插入节点
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            //遍历插入节点
            for (int binCount = 0; ; ++binCount) {
                //寻找到最后一个节点是null的时候，存放节点   
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    //如果当前列表下的节点>7的时候，转换成为二叉树
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                //如果不是的话，那么一直遍历循环。
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}


n = tab.length
(n - 1) & hash



//初始化大小，还有扩容
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap, newThr = 0;
    if (oldCap > 0) {
        //2的n次方必须小于1 << 30------》2^30
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        //DEFAULT_INITIAL_CAPACITY 默认大小是 1<<4位，即16。
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    }
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    else { // zero initial threshold signifies using defaults
        newCap = DEFAULT_INITIAL_CAPACITY;
        //阈值时 DEFAULT_LOAD_FACTOR*16 =12 。 3/4。
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    if (oldTab != null) {
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```


问题：

1.列表转换红黑树阈值时8，红黑树转换列表阈值是6。为什么？

通过源码我们得知HashMap源码作者通过泊松分布算出，当桶中结点个数为8时，出现的几率是亿分之6的，因此常见的情况是桶中个数小于8的情况，此时链表的查询性能和红黑树相差不多，因为转化为树还需要时间和空间，所以此时没有转化成树的必要。

既然个数为8时发生的几率这么低，我们为什么还要当链表个数大于8时来树化来优化这几乎不会发生的场景呢？

首先我们要知道亿分之6这个几乎不可能的概率是建立在什么情况下的 答案是：建立在良好的hash算法情况下，例如String，Integer等包装类的hash算法、如果一旦发生桶中元素大于8，说明是不正常情况，可能采用了冲突较大的hash算法，此时桶中个数出现超过8的概率是非常大的，可能有n个key冲突在同一个桶中，此时再看链表的平均查询复杂度和红黑树的时间复杂度，就知道为什么要引入红黑树了，
举个例子，若hash算法写的不好，一个桶中冲突1024个key，使用链表平均需要查询512次，但是红黑树仅仅10次，红黑树的引入保证了在大量hash冲突的情况下，HashMap还具有良好的查询性能。
红黑树的时间复杂度：
红黑树的插入、删除和遍历的最坏时间复杂度都是log(n)，
列表的时间复杂度：n

2.hashMap的扩容过程是怎么样子的，扩容的大小是什么样的？

那么hashmap什么时候进行扩容呢？当hashmap中的元素个数超过数组大小*loadFactor时，就会进行数组扩容，loadFactor的默认值为0.75，也就是说，默认情况下，数组大小为16，那么当hashmap中元素个数超过16*0.75=12的时候，就把数组的大小扩展为2*16=32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知hashmap中元素的个数，那么预设元素的个数能够有效的提高hashmap的性能。比如说，我们有1000个元素new HashMap(1000), 但是理论上来讲new HashMap(1024)更合适，不过上面annegu已经说过，即使是1000，hashmap也自动会将其设置为1024。 但是new HashMap(1024)还不是更合适的，因为0.75*1000 < 1000, 也就是说为了让0.75 * size > 1000, 我们必须这样new HashMap(2048)才最合适，既考虑了&的问题，也避免了resize的问题。

3.hash的冲突是如何处理的？

如果persons.put(“1”,”jack”);persons.put(“2”,”john”); 同时计算到的hash值都为123，那么jack先放在第一列的第一个位置Node-jack，persons.put(“2”,”john”);执行时会将Node-jack的next(Node) = Node(john)，Jack的下个节点将指向Node(john)。
那么取的时候呢，persons.get(“2”)，这个时候取得的hash值是123，即table[123]，这时table[123]其实是Node-jack，Key值不相等，取Node-jack的next下个Node，即Node-John，这时Key值相等了，然后返回对应的person.

4.hashMap的多线程的环境下会引发什么样的情况？（列表环路，为什么？画图说明）

1. put的时候导致的多线程数据不一致。
2. 多线程put后可能导致get死循环：
```java
void transfer(Entry[] newTable) {
     Entry[] src = table;                   //src引用了旧的Entry数组
     int newCapacity = newTable.length;
     for (int j = 0; j < src.length; j++) { //遍历旧的Entry数组
         Entry<K,V> e = src[j];             //取得旧Entry数组的每个元素
         if (e != null) {
             src[j] = null;//释放旧Entry数组的对象引用（for循环后，旧的Entry数组不再引用任何对象）
             do {
                 Entry<K,V> next = e.next;
                 int i = indexFor(e.hash, newCapacity); //！！重新计算每个元素在数组中的位置
                 e.next = newTable[i]; //标记[1]
                 newTable[i] = e;      //将元素放在数组上
                 e = next;             //访问下一个Entry链上的元素
             } while (e != null);
         }
     }
 }
```

1. 对索引数组中的元素遍历

2. 对链表上的每一个节点遍历：用 next 取得要转移那个元素的下一个，将 e 转移到新 Hash 表的头部，使用头插法插入节点。
3. 循环2，直到链表节点全部转移
4. 循环1，直到所有索引数组全部转移
经过这几步，我们会发现转移的时候是逆序的。假如转移前链表顺序是1->2->3，那么转移后就会变成3->2->1。这时候就有点头绪了，死锁问题不就是因为1->2的同时2->1造成的吗？所以，HashMap 的死锁问题就出在这个transfer()函数上。
单线程情况下：

    当线程1已经拿到了Key：3的下一个节点为key:7，
但是此时，线程1已经扩容完成，由于扩容要进行列表反转，此时的key:3的下一个节点已经指向了key：7。所以会导致死循环。


5.hashMap的初始化大小是多少？如果自定义初始化大小为会如何？
初始化大小是：16，

最大定义是2的30次方。超过这个大小则为2的30次方，
自定义为3 的话，则初始化大小为4，为最近的2的n次方。
如果HashMap需要放置1024个元素，由于没有设置容量初始大小，随着元素不断增加，容量7次被迫扩大，resize需要重建hash表，严重影响性能。

6.为什么hashMap的初始化大小会设置为2的n次方？

为了减少hash碰撞，因为tab的存放位置是(n - 1) & hash，2的n次方发生hash碰撞的几率要小，能均匀分布。
为了能让 HashMap 存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀。我们上面也讲到了过了，Hash 值的 范围值-2147483648到2147483647，前后加起来大概40亿的映射空间，只要哈希函数映射得比较均匀松散，一般应 用是很难出现碰撞的。但问题是一个40亿长度的数组，内存是放不下的。所以这个散列值是不能直接拿来用的。用之 前还要先做对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。这个数组下标的计算 方法是“ (n - 1) & hash ”。(n代表数组长度)。这也就解释了 HashMap 的长度为什么是2的幂次方。



