# 常用排序算法稳定性分析

## 1. 选择排序、快速排序、希尔排序、堆排序不是稳定的排序算法

冒泡排序、插入排序、归并排序和基数排序都是稳定的排序算法。

## 2. 研究排序算法的稳定性有何意义?

首先，排序算法的稳定性大家应该都知道，通俗地讲就是能保证排序前两个相等的数据其在序列中的先后位置顺序与排序后它们两个先后位置顺序相同。

再简单具体一点，如果`A[i] == A[j]`，`A[i]` 原来在 `A[j]` 位置前，排序后 `A[i]`  仍然是在 `A[j]` 位置前。 

下面我们分析一下稳定性的好处：

**（1）**如果排序算法是稳定的，那么从一个键上排序，然后再从另一个键上排序，第一个键排序的结果可以为第二个键排序所利用。

基数排序就是这样，先按低位排序，逐次按高位排序，那么，低位相同的数据元素其先后位置顺序即使在高位也相同时是不会改变的。详细请参见[基数排序](RadixSort.md)。

**（2）**学习排序原理时，可能编的程序里面要排序的元素都是简单类型，实际上真正应用时，可能是对一个复杂类型（自定义类型）的数组排序，

而排序的键值仅仅只是这个元素中的一个属性，对于一个简单类型，数字值就是其全部意义，即使交换了也看不出什么不同。

但是，对于复杂类型，交换的话可能就会使原本不应该交换的元素交换了。比如：一个“学生”数组，欲按照年龄排序，“学生”这个对象不仅含有“年龄”，还有其它很多属性。

假使原数组是把学号作为主键由小到大进行的数据整理。而稳定的排序会保证比较时，如果两个学生年龄相同，一定不会交换。

那也就意味着尽管是对“年龄”进行了排序，但是学号顺序仍然是由小到大的要求。

**（3）**如果排序算法稳定，对基于比较的排序算法而言，元素交换的次数可能相对会少一些（个人感觉，没有证实）。 

## 3. 各种排序算法稳定性分析

现在分析一下常见的排序算法的稳定性，每个都给出简单的理由。 

**（1）冒泡排序 **

冒泡排序就是把小的元素往前调（或者把大的元素往后调）。注意是相邻的两个元素进行比较，而且是否需要交换也发生在这两个元素之间。

所以，如果两个元素相等，我想你是不会再无聊地把它们俩再交换一下。

如果两个相等的元素没有相邻，那么即使通过前面的两两交换把两个元素相邻起来，最终也不会交换它俩的位置，所以相同元素经过排序后顺序并没有改变。

所以冒泡排序是一种稳定排序算法。 

**（2）选择排序**

选择排序即是给每个位置选择待排序元素中当前最小的元素。比如给第一个位置选择最小的，在剩余元素里面给第二个位置选择次小的，

依次类推，直到第n-1个元素，第n个元素不用选择了，因为只剩下它一个最大的元素了。

那么，在一趟选择时，如果当前锁定元素比后面一个元素大，而后面较小的那个元素又出现在一个与当前锁定元素相等的元素后面，那么交换后位置顺序显然改变了。

呵呵！比较拗口，举个例子：序列`5 8 5 2 9`， 我们知道第一趟选择第1个元素5会与2进行交换，那么原序列中两个5的相对先后顺序也就被破坏了。

所以选择排序不是一个稳定的排序算法。 

**（3）插入排序**

插入排序是在一个已经有序的小序列的基础上，一次插入一个元素。当然，刚开始这个有序的小序列只有1个元素，也就是第一个元素（默认它有序）。

比较是从有序序列的末尾开始，也就是把待插入的元素和已经有序的最大者开始比起，如果比它大则直接插入在其后面。

否则一直往前找直到找到它该插入的位置。如果遇见一个与插入元素相等的，那么把待插入的元素放在相等元素的后面。

所以，相等元素的前后顺序没有改变，从原无序序列出去的顺序仍是排好序后的顺序，所以插入排序是稳定的。 

**（4）快速排序**

快速排序有两个方向，左边的i下标一直往右走（当条件`a[i] <= a[center_index]`时），其中`center_index`是中枢元素的数组下标，一般取为数组第0个元素。

而右边的j下标一直往左走（当`a[j] > a[center_index]`时）。

如果i和j都走不动了，`i <= j`, 交换`a[i]`和`a[j]`,重复上面的过程，直到`i>j`。交换`a[j]`和`a[center_index]`，完成一趟快速排序。

在中枢元素和a[j]交换的时候，很有可能把前面的元素的稳定性打乱，比如序列为` 5 3 3 4 3 8 9 10 11 `

现在中枢元素5和3(第5个元素，下标从1开始计)交换就会把元素3的稳定性打乱。

所以快速排序是一个不稳定的排序算法，不稳定发生在中枢元素和`a[j]`交换的时刻。 

**（5）归并排序**

归并排序是把序列递归地分成短序列，递归出口是短序列只有1个元素(认为直接有序)或者2个序列(1次比较和交换)，

然后把各个有序的段序列合并成一个有序的长序列，不断合并直到原序列全部排好序。

可以发现，在1个或2个元素时，1个元素不会交换，2个元素如果大小相等也没有人故意交换，这不会破坏稳定性。

那么，在短的有序序列合并的过程中，稳定是是否受到破坏？

没有，合并过程中我们可以保证如果两个当前元素相等时，我们把处在前面的序列的元素保存在结果序列的前面，这样就保证了稳定性。

所以，归并排序也是稳定的排序算法。 

**（6）基数排序**

基数排序是按照低位先排序，然后收集；再按照高位排序，然后再收集；依次类推，直到最高位。

有时候有些属性是有优先级顺序的，先按低优先级排序，再按高优先级排序，最后的次序结果就是高优先级高的在前，高优先级相同的情况下低优先级高的在前。

基数排序基于分别排序，分别收集，所以其是稳定的排序算法。

**（7）希尔排序**

希尔排序是按照不同步长对元素进行插入排序，当刚开始元素很无序的时候，步长最大，所以插入排序的元素个数很少，速度很快；

当元素基本有序时，步长很小，插入排序对于有序的序列效率很高。所以，希尔排序的时间复杂度会比`O(N^2)`好一些。

由于多次插入排序，我们知道一次插入排序是稳定的，不会改变相同元素的相对顺序，

但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱。

所以`shell`排序是不稳定的排序算法。 

**（8）堆排序**

我们知道堆的结构是节点i的孩子为 `2*i` 和 `2*i+1` 节点，大顶堆要求父节点大于等于其2个子节点，小顶堆要求父节点小于等于其2个子节点。

在一个长为 `n` 的序列，堆排序的过程是从第 `n/2` 开始和其子节点共3个值选择最大（大顶堆）或者最小（小顶堆），这3个元素之间的选择当然不会破坏稳定性。

但当为 `n/2-1`, `n/2-2`, `...1`这些个父节点选择元素时，就会破坏稳定性。

有可能第 `n/2` 个父节点交换把后面一个元素交换过去了，而第 `n/2-1` 个父节点把后面一个相同的元素没有交换，那么这2个相同的元素之间的稳定性就被破坏了。

所以，堆排序不是稳定的排序算法。