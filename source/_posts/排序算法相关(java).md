title: 排序算法相关(java)
date: 2017-08-10 00:00:00
tags: [排序算法]

---

# 常见的内部排序算法
插入排序、希尔排序、选择排序、冒泡排序、归并排序、快速排序、堆排序、基数排序等
```
public class SortingDemo {

    public static void main(String[] args) {
        /* 简单排序 */
        EasySort.bubbleList();// 冒泡
        EasySort.bubbleArray();// 冒泡
        EasySort.xuanze();// 选择
        EasySort.charu();// 插入

        /* 高效排序 */
        QuickSort.quickSort();// 快排
        MergeSort.mergeSort();// 归并排序(分治递归思想)
        ShellSort.shellSort();// 希尔排序(插入排序的一种高效率的实现，也叫缩小增量排序)
        // 堆排序(升序排序就使用大顶堆，反之使用小顶堆)

        /* 线性排序 */
        // 计数排序
        // 桶排序
        // 基数排序
    }
}
```
![](http://upload-images.jianshu.io/upload_images/2419179-2804b792efe11837.gif?imageMogr2/auto-orient/strip)
![](http://upload-images.jianshu.io/upload_images/2419179-8403919aa4a9383a.gif?imageMogr2/auto-orient/strip)
![](http://upload-images.jianshu.io/upload_images/2419179-b703fa5194694a5f.gif?imageMogr2/auto-orient/strip)
```
/**
* 简单排序
*/
class EasySort {
    /**
    * 冒泡排序 List
    * <p>
    * 算法思想：遍历待排序的数组，每次遍历比较相邻的两个元素，如果他们的排列顺序错误就交换他们的位置，
    * 经过一趟排序后，最大的元素会浮置数组的末端。重复操作，直到排序完成。
    */
    static void bubbleList() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        Collections.shuffle(list);// list随机打乱
        System.out.println(list);

        for (int i = 0; i < list.size(); i++) {
            for (int j = 0; j < list.size() - 1 - i; j++) {
                int current = list.get(j);
                int comp = list.get(j + 1);
                if (current > comp) {
                    int temp = list.get(j + 1);
                    list.set(j + 1, list.get(j));
                    list.set(j, temp);
                }
            }
            System.out.println(list);
        }
    }

    /**
    * 冒泡排序 Array (循环最大值置顶)
    * <p>
    * 算法思想：遍历待排序的数组，每次遍历比较相邻的两个元素，如果他们的排列顺序错误就交换他们的位置，
    * 经过一趟排序后，最大的元素会浮置数组的末端。重复操作，直到排序完成。
    */
    static void bubbleArray() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        Collections.shuffle(list);// list随机打乱
        Integer[] values = (Integer[]) list.toArray();
        System.out.println(Arrays.asList(values));

        for (int i = 0; i < values.length; i++) {
            for (int j = 0; j < values.length - 1 - i; j++) {
                int current = values[j];
                int comp = values[j + 1];
                if (current > comp) {
                    int temp = values[j + 1];
                    values[j + 1] = values[j];
                    values[j] = temp;
                }
            }
            System.out.println(Arrays.asList(values));
        }
    }


    /**
    * 选择排序
    * <p>
    * 算法思想：重待排序的数组中选择一个最小的元素，将它与数组的第一个位置的元素交换位置。
    * 然后从剩下的元素中选择一个最小的元素，将它与第二个位置的元素交换位置，
    * 如果最小元素就是该位置的元素，就将它和自身交换位置，依次类推，直到排序完成。
    */
    static void xuanze() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        Collections.shuffle(list);// list随机打乱
        Integer[] values = (Integer[]) list.toArray();
        System.out.println(Arrays.asList(values));

        Integer[] array = values;
        for (int i = 0; i < values.length; i++) {
            int courrentMinIndex = i;// 当前最小值下标

            // 从当前下标到最后的范围,找出最小的值
            for (int j = i + 1; j < values.length; j++) {
                if (values[j] < values[courrentMinIndex]) {
                    courrentMinIndex = j;
                }
            }

            // 当前位与最小值位交换
            int temp = values[i];
            values[i] = values[courrentMinIndex];
            values[courrentMinIndex] = temp;

            System.out.println(Arrays.asList(array));
        }
    }

    /**
    * 插入排序(优先保障currentIndex前置有序)
    * <p>
    * 算法思想:从数组的第二个元素开始遍历，将该元素与前面的元素比较，如果该元素比前面的元素小，
    * 将该元素保存进临时变量中，依次将前面的元素后移，然后将该元素插入到合适的位置。
    * 每次排序完成后，索引左边的元素一定是有序的，但是还可以移动。对于倒置越少的数组，该算法的排序效率越高。
    */
    static void charu() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        Collections.shuffle(list);// list随机打乱
        Integer[] values = (Integer[]) list.toArray();
        System.out.println(Arrays.asList(values));

        for (int i = 1; i < values.length; i++) {
            for (int j = 0; j < i; j++) {
                int current = values[i];
                int comp = values[j];
                if (current < comp) {
                    int third = values[i];
                    values[i] = values[j];
                    values[j] = third;
                }
            }
            System.out.println(Arrays.asList(values));
        }
    }
}
```
![](http://upload-images.jianshu.io/upload_images/2419179-3b6bbe9a5b4a3c22.gif?imageMogr2/auto-orient/strip)
![](http://upload-images.jianshu.io/upload_images/675733-58e22d5b09c167f2.gif?imageMogr2/auto-orient/strip)
```
/**
* 快速排序
*/
class QuickSort {
    /**
    * 快速排序的基本思想：通过一趟排序将待排序记录分割成独立的两部分，其中一部分记录的关键字均比另一部分关键字小，
    * 则分别对这两部分继续进行排序，直到整个序列有序。
    */
    static void quickSort() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        Collections.shuffle(list);// list随机打乱
        System.out.println("======(分区)");
        Integer[] values = (Integer[]) list.toArray();
        System.out.println(Arrays.asList(values));
        quickSortEasy(values, 0, values.length - 1);


        System.out.println("======(交换)");
        values = (Integer[]) list.toArray();
        System.out.println(Arrays.asList(values));
        quickSortSwap(values, 0, values.length - 1);

    }

    /**
    * 分治法-分区(方便理解)
    *
    * @param values
    * @param begin
    * @param end
    */
    static void quickSortEasy(Integer[] values, int begin, int end) {
        if (begin >= end) return;

        int middleIndex = quickSortMiddleEasy(values, begin, end);

        System.out.printf("begin:%d end:%d middleIndex:%d(value=%d) %n", begin, end, middleIndex, values[middleIndex]);
        System.out.println(Arrays.asList(values));// 定位中位数时,已经完成分治

        quickSortEasy(values, begin, middleIndex - 1);
        quickSortEasy(values, middleIndex + 1, end);
    }

    static int quickSortMiddleEasy(Integer[] values, int begin, int end) {
        // 第一步,以最低位值作为中轴,将其它数进行两边分治法.结束后,中轴位不再参与后续排序(因为位置已经确定了)
        // 示例:[6, 2, 1, 8, 5, 4, 9, 3, 7]中轴值为6,分治法结果[32145]6[987](交换)/[21543]6[897](分区)
        int middleValue = values[begin];

        // 第二步,分治法.先腾出中轴值位置[0],并保持中轴值.
        List<Integer> left = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        left.addAll(Arrays.asList(Arrays.copyOfRange(values, 0, begin)));// 排序区间前
        for (int i = begin; i < end + 1; i++) {
            if (values[i] < middleValue) {
                left.add(values[i]); // 排序区间左区
            } else if (values[i] > middleValue) {
                right.add(values[i]);// 排序区间右区
            }
        }
        left.add(middleValue);// 排序区间中轴
        int middleIndex = left.size() - 1;
        left.addAll(right);// 排序区间右区也合并到单个集合
        left.addAll(Arrays.asList(Arrays.copyOfRange(values, end + 1, values.length)));// 排序区间后

        values = left.toArray(values);// 重写原数组
        return middleIndex;
    }

    /**
    * 分治法-交换(节省内存空间)
    * @param values
    * @param begin
    * @param end
    */
    static void quickSortSwap(Integer[] values, int begin, int end) {
        if (begin >= end) return;

        int middleIndex = getMiddle(values, begin, end);

        System.out.printf("begin:%d end:%d middleIndex:%d(value=%d) %n", begin, end, middleIndex, values[middleIndex]);
        System.out.println(Arrays.asList(values));// 定位中位数时,已经完成分治

        quickSortSwap(values, begin, middleIndex - 1);
        quickSortSwap(values, middleIndex + 1, end);
    }


    public static int getMiddle(Integer[] numbers, int low, int high) {

        // 示例:[6, 2, 1, 8, 5, 4, 9, 3, 7]中轴值为6,分治法结果[32145]6[987](交换)/[21543]6[897](分区)

        int temp = numbers[low]; // 数组的第一个作为中轴
        numbers[low] = null;// 原位置值已经转移,未来也会被分治法的新值覆盖,可省略.但标记null,方便理解
        while (low < high) {
            while (low < high && numbers[high] > temp) {// 先最高位与中轴值比较
                high--;
            }
            numbers[low] = numbers[high];// 比中轴小的记录移到低端
            numbers[high] = null;// 原位置值已经转移,未来也会被分治法的新值覆盖,可省略.但标记null,方便理解
            while (low < high && numbers[low] < temp) {
                low++;
            }
            numbers[high] = numbers[low]; //比中轴大的记录移到高端
            numbers[low] = null;// 原位置值已经转移,未来也会被分治法的新值覆盖,可省略.但标记null,方便理解
        }
        numbers[low] = temp; //中轴记录到尾
        return low; // 返回中轴的位置
    }

}
```
![](http://upload-images.jianshu.io/upload_images/2419179-ced6d60c4e8aa5d9.gif?imageMogr2/auto-orient/strip)
![](http://upload-images.jianshu.io/upload_images/675733-07c5e4edb9b0f8b7.gif?imageMogr2/auto-orient/strip)

---
# 归并排序
- 基本思想:
先递归划分子问题，然后合并结果。把待排序列看成由两个有序的子序列，然后合并两个子序列，然后把子序列看成由两个有序序列。
倒着来看，其实就是先两两合并，然后四四合并，最终形成有序序列。
- 做法(建议sublime中编辑,方便符号对其):
```
int mid = (beginIndex + endIndex) / 2;// 取中位index
middleSort(values, beginIndex, mid);// 左侧递归再分,到不可再分
middleSort(values, mid + 1, endIndex);// 右侧递归再分,到不可再分
```
![](http://upload-images.jianshu.io/upload_images/675733-a1330ae9ce4293a4.gif?imageMogr2/auto-orient/strip)
```
7, 6, 5, 2, 1, 3, 4, 9, 8
-  -  -  -  -  -  -  -  -8
-  -  -  -  -4
-  -  -2
-  -1
        -  -3
                -  -  -  -7
                -  -5
                        -  -6
```
- `比较交换,方法一`:
mid左侧第一位开始与mid右侧第一位开始比较,小的添加到新array.最后再按区间更新原array
如:`[5, 6, 7, 1, 2]`(中位数两侧事前已经经过排序)会拆位`[5,6,7][1,2]`(进行合并)
5<>1 => `1(小)` tempArray=[1]
5<>2 => `2(小)` tempArray=[1,2]
把没有到tempArray的大值添加进去: tempArray=`[1,2,5,6,7]`

见: `★面试中的排序算法总结 - 简书` - 归并排序`
http://www.jianshu.com/p/c360a58db21d

- `比较交换,方法二(推荐)`: 对待合并数据[5, 6, 7][1, 2]直接仅需插入排序.
```
// 待合并的二分数据,已经基本有序,所有使用`插入排序`进行合并,具备更高的性能(因为移动的位置较少)且不需要更多的内存仅需一个交换变量.
for (int i = beginIndex + 1; i < endIndex; i++) {
    for (int j = beginIndex; j < i; j++) {
        if (values[i] < values[j]) {
            int temp = values[j];
            values[j] = values[i];
            values[i] = temp;
            System.out.println("markNum:"+markNum++);
        }
    }
}
System.out.println(Arrays.asList(values));
```
见: `myServer/@java/demo-sorting/SortingDemo.java`
```
/**
* 归并排序
*/
class MergeSort {
    static void mergeSort() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        Collections.shuffle(list);// list随机打乱
        Integer[] values = (Integer[]) list.toArray();
        System.out.println(Arrays.asList(values));

        middleSort(values, 0, values.length);
    }

    static int markNum=0;

    static void middleSort(Integer[] values, int beginIndex, int endIndex) {
        if (!(beginIndex < endIndex))
            return;

        int mid = (beginIndex + endIndex) / 2;// 取中位index

        middleSort(values, beginIndex, mid);// 左侧递归再分,到不可再分
        middleSort(values, mid + 1, endIndex);// 右侧递归再分,到不可再分

        // 对最近的二分数据进行合并. 如:[5,6,7][1,2]
        // 待合并的二分数据,已经基本有序,所有使用`插入排序`进行合并,具备更高的性能(因为移动的位置较少)且不需要更多的内存仅需一个交换变量.
        for (int i = beginIndex + 1; i < endIndex; i++) {
            for (int j = beginIndex; j < i; j++) {
                if (values[i] < values[j]) {
                    int temp = values[j];
                    values[j] = values[i];
                    values[i] = temp;
                    System.out.println("markNum:"+markNum++);
                }
            }
        }
        System.out.println(Arrays.asList(values));

    }
}
```

---
# 希尔排序
基本思想是：先将整个待排记录序列分割成为若干子序列，分别进行直接插入排序。待整个序列中的记录基本有序时，再对全体记录进行一次直接插入排序。
![](http://upload-images.jianshu.io/upload_images/675733-4ea6e4e8613f769f.gif?imageMogr2/auto-orient/strip)
![](http://upload-images.jianshu.io/upload_images/675733-873e48115b182525.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
8, 7, 4, 1, 3, 5, 6, 2, 9
length/2=4
-          -
  -          -
      -          -
        -              -
            -          -
3, 5, 4, 1, 8, 7, 6, 2, 9
4/2=2
-    -
  -    -
      -    -
        -    -
      -    -    -
        -    -    -
      -    -    -    -
x    x
  1    5
      x    8
        x    7
      x    6    8
        2    5    7
      x    x    x    9
3, 1, 4, 2, 6, 5, 8, 7, 9
2/2=1
-  -
  -  -
  -  -  -
  -  -  -  -
  -  -  -  -  -
  -  -  -  -  -  -
  -  -  -  -  -  -  -
  -  -  -  -  -  -  -  -
1  3 i=1
  x  x i=2
  2  3  4 i=3  
  x  x  x  6 i=4
  x  x  x  5  6 i=5
  x  x  x  x  X  8 i=6
  x  x  x  x  X  7  8 i=7
  x  x  x  x  X  x  x  x  9 i=8
```
```
/**
* 希尔排序
*/
class ShellSort {

    static void shellSort() {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
        Collections.shuffle(list);// list随机打乱
        Integer[] values = (Integer[]) list.toArray();// List 转 Array
        System.out.println(Arrays.asList(values));

        int[] valuesI = new int[values.length];
        Arrays.stream(values).forEachOrdered(i -> valuesI[i - 1] = values[i - 1].intValue());// 封装数组转基础数组(Integer[] 转 int[])
        Arrays.stream(valuesI).forEach(value -> System.out.print(value + ", "));// 打印基础类型数组(int[])
        System.out.println();

        int[] valuesI2 = new int[]{8, 7, 4, 1, 3, 5, 6, 2, 9};// 自定义
        shellSort(valuesI2);
        Arrays.stream(valuesI2).forEach(value -> System.out.print(value + ", "));// 打印基础类型数组(int[])
    }

    /**
    * 希尔排序的一趟插入
    *
    * @param arr 待排数组
    * @param d  增量
    */
    public static void shellInsert(int[] arr, int d) {
        System.out.println("d:" + d);
        for (int i = d; i < arr.length; i++) {
            int j = i - d;
            int temp = arr[i];    //记录要插入的数据
            while (j >= 0 && arr[j] > temp) {  //从后向前，找到比其小的数的位置
                arr[j + d] = arr[j];    //向后挪动
                j -= d;
            }

            if (j != i - d)    //存在比其小的数
                arr[j + d] = temp;

            Arrays.stream(arr).forEach(value -> System.out.print(value + ", "));// 打印基础类型数组(int[])
            System.out.println();
        }
    }

    public static void shellSort(int[] arr) {
        if (arr == null || arr.length == 0)
            return;
        int d = arr.length / 2;
        while (d >= 1) {
            shellInsert(arr, d);
            d /= 2;
        }
    }

}
```

---
# 时间复杂度,空间复杂度,稳定性
![](http://upload-images.jianshu.io/upload_images/675733-f9bd437a4a3eeab7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 关于时间复杂度：
`平方阶(O(n2))排序`：各类简单排序:直接插入、直接选择和冒泡排序。
`线性对数阶(O(nlog2n))排序`：快速排序、堆排序和归并排序。
`O(n1+§))排序`：§是介于0和1之间的常数。
`线性阶(O(n))排序`：基数排序，此外还有桶、箱排序。

- 关于稳定性：
`稳定的排序算法`：冒泡排序、插入排序、归并排序和基数排序；
`不是稳定的排序算法`：选择排序、快速排序、希尔排序、堆排序。

---
# 平均时间,最坏情况,辅助存储
![](http://upload-images.jianshu.io/upload_images/675733-3f48f14f89392094.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- `平均时间`来看，`快速排序`是效率最高的。但快速排序在最坏情况下的时间性能，不如堆排序和归并排序。而后者相比较的结果是在n较大时，归并排序使用时间较少，但使用辅助空间较多。
- `简单排序`，包括除希尔排序之外的所有冒泡排序、插入排序、简单选择排序。其中`直接插入`排序最简单。但`序列基本有序或者n较小时，直接插入排序是好的方法`。因此常将它和其他的排序方法，如`快速排序、归并排序`等结合在一起使用。
- `基数排序`的时间复杂度也可以写成O(d*n)。因此它最使用于n值很大而关键字较小的的序列。若关键字也很大，而序列中大多数记录的最高关键字均不同，则亦可先按最高关键字不同，将序列分成若干小的子序列，而后进行直接插入排序。
- 从方法的`稳定性`来比较，`基数排序是稳定的内排方法`，所有时间复杂度为O(n^2)的简单排序也是稳定的。但是快速排序、堆排序、希尔排序等时间性能较好的排序方法都是不稳定的。稳定性需要根据具体需求选择。
- 上面的算法实现大多数是使用`线性存储结构`，像插入排序这种算法，用`链表`实现更好，省去了移动元素的时间。具体的存储结构，在具体的实现版本中也是不同的。


---
**参考**
`★面试中的排序算法总结 - 简书`
http://www.jianshu.com/p/c360a58db21d

`排序算法图形化比较：快速排序、插入排序、选择排序、冒泡排序 - 简书`
http://www.jianshu.com/p/70619984fbc6
`完全二叉树实现优先队列与堆排序 - 简书`
http://www.jianshu.com/p/9a456d1b59b5

`8大排序算法图文讲解`
http://www.jianshu.com/p/e6ad4423efcd

`八大排序算法 - guisu，程序人生。 逆水行舟，不进则退。 - CSDN博客`
http://blog.csdn.net/hguisu/article/details/7776068
