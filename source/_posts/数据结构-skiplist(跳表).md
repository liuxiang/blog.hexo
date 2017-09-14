title:  数据结构-skiplist(跳表)


date: 2017-07-22 00:00:00
tags: [skiplist ]



---

# 跳表介绍
Skip List是 William Pugh 在1989年创建出来的(又见一个位神牛), 主要的目的就像他描述的那样，是`用来替代平衡树`的。跳表是一种随机性的数据结构，`相对于平衡树来说，跳表更加的简单`，能一口气实现红黑树,AVL这样的平衡树的人，还是太少了，而且内部确实复杂，调试, 用起来太麻烦。 同样跳表还可以做到平衡树那样的查找时间，特别是在并发的场景下面，由于红黑树的插入或者删除会做rebalance这样操作，那么影响的数据就会变多，锁的粒度就变大。但是跳表的插入或者删除操作影响的数据会很小，锁的粒度就会小，这样在大数据量的情况下，跳表的性能自然就会比红黑树要好。


- `ConcurrnetSkipListMap`的实现
其实ConcurrentSkipListMap的实现就是实现了一个无锁版的跳表，主要是利用无锁的链表的实现来管理跳表底层，同样利用CAS来完成替换。以后会带来无锁的设计实现。


`从零单排 Java Concurrency, SkipList&ConcurrnetSkipListMap - 推酷`
http://www.tuicool.com/articles/ZJRrEzq


---
#  插入操作从而形成一个skiplist的过程
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-22/54436659.jpg)

` Redis内部数据结构详解(6)——skiplist - 推酷 `
http://www.tuicool.com/articles/NRFBzq

---
# 跳跃表的应用
Skip list(跳表）是一种可以代替平衡树的数据结构，`默认是按照Key值升序`的。Skip list让已排序的数据分布在多层链表中，以`0-1随机数决定一个数据的向上攀升与否`，通过“空间来换取时间”的一个算法，在每个节点中增加了向前的指针，在插入、删除、查找时可以忽略一些不可能涉及到的结点，从而提高了效率。

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-22/62551869.jpg)

- 对于19，查找过程是：

```
与9相比，大于9
向右与21相比，小于21
向下与17相比，大于17
向右与21相比，小于21
向下与19相比，找到
```
- 对于8，查找过程是：

```
与9相比，小于9
向下与6相比，大于6
向右与9相比，小于9
向下与7相比，大于7
向右与9相比，小于9，不能再向下，没找到
```
` 计算机程序的思维逻辑 (75) - 并发容器 - 基于SkipList的Map和Set - 推酷 `
http://www.tuicool.com/articles/y2ArA3N

---
# 随机化
SkipList是一种概率算法，非常依赖于生成的随机数。这里不能用rand() % MAX_LEVEL的简单做法，而要用满足p=1/2几何分布的随机数 ```
int SkipList::RandomLevel(void) {
    int level = 0;
    while(rand() % 2 && level < MAX_LEVEL - 1)
        ++level;
    return level;
}
```

SkipList层数合适时自顶向下搜索，理想情况下`每下降一层，搜索范围减小一半，达到类似二分查找的效果`，效率为O(lgn)；最坏情况下也只是curr从head移动到tail，效率为O(n)


我的实现里最大层数是通过MAX_LEVEL静态指定的，也可以让最大层数动态增长——RandomLevel里不设置最大值，`插入`节点时得到的`level比当前SkipList层数大时就在顶上再加一层`，`删除`节点时如果`只有这个节点在高层就去掉高层`。


` 深夜学算法之SkipList：让链表飞 - 推酷 `
http://www.tuicool.com/articles/qUfueqJ


---
# 体验 ` ConcurrentSkipListMap`，`  ConcurrentSkipListSet`
```
package skiplist;
 
import java.util.Map;
import java.util.Random;
import java.util.Set;
import java.util.concurrent.ConcurrentSkipListMap;
import java.util.concurrent.ConcurrentSkipListSet;
 
public class SkiplistDemo {
 
    static void m_ConcurrentSkipListMap() {
        Map skipListMap = new ConcurrentSkipListMap();
        for (int i = 0; i < 10; i++) {
            int kv = (int) (Math.random() * 100);
            skipListMap.put(kv, kv);
        }
        System.out.println(skipListMap);// {4=4, 9=9, 13=13, 30=30, 44=44, 49=49, 59=59, 66=66, 89=89, 95=95}
    }
 
    static void m_ConcurrentSkipListSet() {
        Set skipListSet = new ConcurrentSkipListSet();
        for (int i = 0; i < 10; i++) {
            int kv = new Random().nextInt(100);
            skipListSet.add(kv);
        }
        System.out.println(skipListSet);// [4, 14, 31, 45, 48, 52, 54, 59]
    }
 
    public static void main(String[] args) {
        m_ConcurrentSkipListMap();
        m_ConcurrentSkipListSet();
    }
}
 
/**
* 在Java的API中已经有了实现：分别是
* ConcurrentSkipListMap(在功能上对应HashTable、HashMap、TreeMap);
* ConcurrentSkipListSet(在功能上对应HashSet);
* <p>
* 跳跃表Skip List的原理和实现-博客-云栖社区-阿里云
* https://yq.aliyun.com/articles/38381
*/
```



--- 
**参考**
`★  Redis为什么用跳表而不用平衡树？ `
http://www.tuicool.com/articles/bqquUfI


` JAVA SkipList 跳表 的原理和使用例子 - 大树叶 技术专栏 - CSDN博客 `
http://blog.csdn.net/bigtree_3721/article/details/51291974


` 跳跃表原理 - thrillerz - 博客园 `
http://www.cnblogs.com/thrillerz/p/4505550.html

` 跳跃表Skip List的原理和实现-博客-云栖社区-阿里云 `
https://yq.aliyun.com/articles/38381


` 【算法导论33】跳跃表（Skip list）原理与java实现 - 博客频道 - CSDN.NET `
http://blog.csdn.net/brillianteagle/article/details/52206261




