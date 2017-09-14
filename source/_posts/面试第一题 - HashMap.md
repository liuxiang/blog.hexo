title:  面试第一题 - HashMap

date: 2017-07-19 00:00:00
tags: [ HashMap ]



---
# 考察
- 集合类的` 日常 使用`及场景理解：
    - 数组/link双向链表/Tree/ statck ：数据结构差异带来的`有序无序`，`可否重复`，`查询与删除效率`，` 先进后出` 差异
    - 线程安全的并发支持差异： Vector- Stack   、HashTable、Properties

    - 快速失败、安全失败的差异

- `技术实现原理认识`：
    - 默认 hash 数组长度及增长因子

    - hash数组的定位-位运算 (size-1) 取模
    - hash冲突的处理机制（拉链，Tree）
    - resize的hash数组的扩容和元素位移策略-位运算size取模
- `思想利用（举一反三）`
    - hash table 通用的hash索引技术
    - （size-1）取模，范围内随机定位的最高效方法

    - 数据结构：红黑树，优秀的最坏情况排序方法

- `进阶探索`

    - `线程安全( java.util.concurrent )`：hashTable，ConcurrentHashMap

        - 并发锁，分段锁，CAS锁

     - `skiplist（跳表）`： ConcurrentSkipListMap， ConcurrentSkipListSet    

          - 数据结构： SkipList跳表，接近红黑树的排序性能，但具备更简单的数据结构


`总结`：
不同层次的技术员，会要求不同层次的认知。更重要的是， 其它掌握的技术要求深度会于此等同。
作为一个技术员，因及时评估自己掌握或触及的知识，是否过于浅薄。把学习心态做的日常


---
#   HashMap 数据结构、扩展策略,Hash 冲突攻击如何防范,如何实现线程安全的 HashMap?
结构: `数组加链表`(java8 中链表长度超过8 时, 会改成平衡树实现)
扩展: `resize` , 当超过loadfactor 因子时,默认 0.75 , 触发扩展,rehash , 将旧的元素迁移到新的
hash collision attack : 修改hash 非随机算法, 限制post 参数个数,大小 , 防火前检测异常请求
使用 ConcurrentHashMap : `CHM 实现采用了 segment 锁` , 8 之后采用的`CAS 技术`
哈希冲突解决方法: `链表法`(java 当前实现),建立公共溢出区 , 开发`定址法`。


## 双hash
```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
对Object的hashCode进行16位的`无符号右移>>>` 再与hashCode进行`位异（^）10>1`运算
```
计算key.hashCode（）和扩展（XOR）更高的散列比特到较低。 由于该表使用二次幂掩蔽，所以仅在当前掩码之上的位数变化的散列集将总是相冲突。 （其中已知的例子是在小型表格中保持连续整数的浮点键集合）。因此，我们应用一种将高位的影响向下扩展的变换。 速度，效用和位扩展质量之间存在权衡。 因为许多常见的散列集合已经合理分配（因此不会受益于扩散），并且因为我们使用树来处理大量的冲突，所以我们只是以最便宜的方式将XOR移位，以减少系统损失， 以及包含由于表格界限而无法在索引计算中使用的最高位的影响。
```
```

Integer i=100;
int ihc =i.hashCode();
System.out.println(ihc);
System.out.println((ihc>>>16) ^ ihc);// 仅会影响到高16位存在数值的对象 
// 作用：jdk的hashCode算法原因,有可能存在大量对象hashcode的变化体现在高16位,而低16位相对稳定,如此会造成hash表分配的不均匀
```
jdk1.8 这样设计保证了对象的hashCode的高16位的变化能反应到低16位中，相比较而言减少了过多的位运算，是一种折中的设计。

` Java中hashCode()方法以及HashMap()中hash()方法 - TonyLuis - 博客园 `
http://www.cnblogs.com/tonyluis/p/5671873.html


`HashMap hash方法分析 - Java综合 - Java - ITeye论坛`
http://www.iteye.com/topic/709945

## 数组定位
```
HashMap.Node<K,V>[] tab; HashMap.Node<K,V> p; int n, i;
if ((tab = table) == null || (n = tab.length) == 0)
    n = (tab = resize()).length;
if ((p = tab[i = (n - 1) & hash]) == null)
    tab[i] = newNode(hash, key, value, null);
else {
```
关键：`tab[i = (n - 1) & hash])` 
数字长度与hash进行` 位与( & 11>1 )` 运算（取模运算） ，作用：结果值一定会在（n-1）的范围以内
分配均匀，通过利用hashCode的高16位和低16位的`位异(^)`来保障。如下图示意：



![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/15348420.jpg)
 
## 数组位已经占用，处理机制
-  hash相同，key内存地址相同或key值equals相同则覆盖
```
if (p.hash == hash &&
        ((k = p.key) == key || (key != null && key.equals(k))))
    e = p;
```


-  如果对象类型是TreeNode类型，说明已经是红黑树代替了链表，并向其中插入结点
```
else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
```


-  非空，非树，那么就是链表，将新值插入到next中。逐个比较，hash相同，key内存地址相同或key值equals相关进行替换，为null时直接存放。
```
else {
    for (int binCount = 0; ; ++binCount) {
        if ((e = p.next) == null) {
            p.next = newNode(hash, key, value, null);
            if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                treeifyBin(tab, hash);
            break;
        }
        if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
            break;
        p = e;
    }
}
```


---
## resize
- `调用 时机`：超出负载值
```
if (++size > threshold)
    resize();
```


- `处理内容`：计算扩容大小 - 新建hash表 - 将老数据填充过去
`技巧`：扩容前后的hash 位与(`&`)运算，结果为0即位置不变，否则位置+原hash表length



```

int threshold; 
static final int MAXIMUM_CAPACITY = 1 << 30; 
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16 
static final float DEFAULT_LOAD_FACTOR = 0.75f; 
final float loadFactor; 
 
// 扩容方法，不能被重写 
     final Node<K, V>[] resize() { 
           Node<K, V>[] oldTab = table; 
           // 获取当前容量 
           int oldCap = (oldTab == null) ? 0 : oldTab.length; 
           // 获取扩容阀值，默认0 
           int oldThr = threshold; 
           // newCap 新容量 newThr 新阀值 
           int newCap, newThr = 0; 
 
           if (oldCap > 0) { 
                // 判断其容量如果大于最大值就将扩容阀值设置为Integer最大值 
                if (oldCap >= MAXIMUM_CAPACITY) { 
                      threshold = Integer.MAX_VALUE; 
                      return oldTab; 
                      // 判断当前容量的两倍是否小于最大容量限定，并且当前容量是否大于等于默认的16 ，满足条件就将当前容量扩大一倍 
                } else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY && oldCap >= DEFAULT_INITIAL_CAPACITY) 
                      newThr = oldThr << 1; // double threshold 
           } else if (oldThr > 0) // initial capacity was placed in threshold 
                newCap = oldThr; 
           // 走到这代表是个新map首次创建 
           else { // zero initial threshold signifies using defaults 
                      // 初始化容量16 
                newCap = DEFAULT_INITIAL_CAPACITY; 
                // 初始化计算扩容阀值12 
                newThr = (int) (DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY); 
           } 
           if (newThr == 0) { 
                float ft = newCap * loadFactor; 
                newThr = (newCap < MAXIMUM_CAPACITY && ft < MAXIMUM_CAPACITY ? (int) ft : Integer.MAX_VALUE); 
           } 
           threshold = newThr; 
           @SuppressWarnings({ "rawtypes", "unchecked" }) 
           Node<K, V>[] newTab = new Node[newCap]; 
           // 赋值扩容完的node[]给teble 
           table = newTab; 
           // 判断当前table是否为空，如果为null说明是新建，否则为扩容 
           if (oldTab != null) { 
                // 根据当前容量循环Node 
                for (int j = 0; j < oldCap; ++j) { 
                      Node<K, V> e; 
                      // 先将老对象赋值给成员变量，然后判断其是否为null 
                      if ((e = oldTab[j]) != null) { 
                           // 将老对象设置为null，方便垃圾回收 
                           oldTab[j] = null; 
                           // 如果当前元素没有下一个元素，计算出其index并赋值给新node[] 
                           if (e.next == null) 
                                 newTab[e.hash & (newCap - 1)] = e; 
                           // 判断其是否为红黑树 
                           else if (e instanceof TreeNode) 
                                 ((TreeNode<K, V>) e).split(this, newTab, j, oldCap); 
                           else { // preserve order 保持顺序 
                                 Node<K, V> loHead = null, loTail = null; 
                                 Node<K, V> hiHead = null, hiTail = null; 
                                 Node<K, V> next; 
                                 do { 
                                      next = e.next; 
                                      // 根据hash oldCap计算出结果，将符合结果的元素组建成为新的链表lo 
                                      if ((e.hash & oldCap) == 0) { 
                                            if (loTail == null) 
                                                 loHead = e; 
                                            else 
                                                 loTail.next = e; 
                                            loTail = e; 
                                      } else { 
                                            // 将不为0的元素组建成为链表hi 
                                            if (hiTail == null) 
                                                 hiHead = e; 
                                            else 
                                                 hiTail.next = e; 
                                            hiTail = e; 
                                      } 
                                 } while ((e = next) != null); 
                                 // 将链表放到原位 
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
` 基础知识(一) HashMap 源码详解 - 我勒个去啊_强 - CSDN博客`


http://blog.csdn.net/luqiang81191293/article/details/62417147



- 链表位置重新计算
```
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
``` ` if ((e.hash & oldCap) == 0) `  
作用：判断是否会在下次扩容后，重新去摸hash表位置是否会发生变化。巧妙之处可见下文分析二进制位运算。理解还在进行中. 原来的hash值新增的那个bit是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+oldCap”



由于`新增的1bit是0还是1可以认为是随机的`，因此resize的过程，`均匀的把之前的冲突的节点分散到新的bucket了`。这一块就是JDK1.8新增的优化点。有一点注意区别，JDK1.7中rehash的时候，旧链表迁移新链表的时候，如果在新表的数组索引位置相同，则链表元素会倒置，但是从上图可以看出，JDK1.8不会倒置。



---
- 对`16-32-64-128-256`几次扩容的位运算解析
```
Integer i=100;
int ihc =i.hashCode();
System.out.println("            "+Integer.toBinaryString(ihc));// 1100100
System.out.println("hash表分配(15):"+Integer.toBinaryString(15)+" "+(ihc&15)+" "+Integer.toBinaryString((ihc&15)));    // hash表分配(15):1111 4 100
System.out.println("resize分配(16):"+Integer.toBinaryString(16)+" "+(ihc&16)+" "+Integer.toBinaryString((ihc&16)));// resize分配(16):10000 0 0
System.out.println("hash表分配(31):"+Integer.toBinaryString(31)+" "+(ihc&31)+" "+Integer.toBinaryString((ihc&31)));// hash表分配(31):11111 4 100
System.out.println("resize分配(32):"+Integer.toBinaryString(32)+" "+(ihc&32)+" "+Integer.toBinaryString((ihc&32)));// resize分配(32):100000 32 100000
System.out.println("resize分配(63):"+Integer.toBinaryString(63)+" "+(ihc&63)+" "+Integer.toBinaryString((ihc&63)));// resize分配(63):111111 36 100100
System.out.println("resize分配(64):"+Integer.toBinaryString(64)+" "+(ihc&64)+" "+Integer.toBinaryString((ihc&64)));// resize分配(64):1000000 64 1000000
System.out.println("resize分配(127):"+Integer.toBinaryString(127)+" "+(ihc&127)+" "+Integer.toBinaryString((ihc&127)));// resize分配(127):1111111 100 1100100
System.out.println("resize分配(128):"+Integer.toBinaryString(128)+" "+(ihc&128)+" "+Integer.toBinaryString((ihc&128)));// resize分配(128):10000000 0 0
System.out.println("resize分配(255):"+Integer.toBinaryString(255)+" "+(ihc&255)+" "+Integer.toBinaryString((ihc&255)));// resize分配(255):11111111 100 1100100
System.out.println("resize分配(256):"+Integer.toBinaryString(256)+" "+(ihc&256)+" "+Integer.toBinaryString((ihc&256)));// resize分配(256):100000000 0 0
```


- 随机测试，hashMap在大于12位后resize扩容。`观察新老数据的重新去摸定位hash表情况`
```
HashMap hashMap=new HashMap();
for (int i = 0; i < 50; i++) {
    int kv = (int)(Math.random()*10000);
    System.out.println(i+" "+kv);
    hashMap.put(kv,kv);
}
```


---
**参考**
` Java 8系列之重新认识HashMap - 简书 `
http://www.jianshu.com/p/24f5bd05b860
http://www.jianshu.com/p/30bffabb2e5c


` Java集合干货系列-（三）HashMap源码解析 - 简书 `
http://www.jianshu.com/p/e6536af1018f

`java的一份面试题 - 今日头条(www.toutiao.com)`
http://www.toutiao.com/i6442518207210193421/

` 5分钟快速实现一个哈希表 - longmon的个人页面`
https://my.oschina.net/longmon/blog/804684


` Java集合中那些类是线程安全的 - Mexican的博客 - CSDN博客`
http://blog.csdn.net/mexican_ok/article/details/12859351


` Java集合（实现类线程安全性） - yuyibinggooo的博客 - CSDN博客 `
http://blog.csdn.net/yuyibing888/article/details/50565021
