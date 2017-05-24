# Python全栈之路系列之算法

一个算法的优劣可以用空间复杂度与时间复杂度来衡量。

## 冒泡排序

> 冒泡排序（英语：Bubble Sort，台湾另外一种译名为：泡沫排序）是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

```python
from random import randint

li = [randint(-10, 1000) for _ in range(10)]  # 生成10个-10到100之间的随机数字
for n in range(len(li)):  # 循环列表中所有的元素
    for i in range(len(li) - 1 - n):  # 每次求最大值，放到最后，不对比匹配到最大的那个值
        if li[i] > li[i + 1]:  # 当前值大于当前值的下一个
            li[i], li[i + 1] = li[i + 1], li[i]  # 位置调换一下
print(li)  # 输出
```

## 选择排序

> 选择排序（Selection sort）是一种简单直观的排序算法。它的工作原理如下。首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

```python
from random import randint

List = [randint(-10, 1000) for _ in range(10)]  # 生成10个-10到100之间的随机数字
for Index in range(len(List)):  # 循环列表所有索引
    Sam = Index  # 最小值的索引
    for I in range(Index, len(List)):  # 每次循环都不取上次取到的最大值
        if List[Sam] > List[I]:  # 列表的某个值大于当前小循环的值
            Sam = I  # 最小值的所以赋值给sma
    List[Index], List[Sam] = List[Sam], List[Index]  # 交换位置
print(List)
```

## 插入排序

> 插入排序（英语：Insertion Sort）是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。插入排序在实现上，通常采用in-place排序（即只需用到O(1)的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。

```python
from random import randint

List = [randint(-10, 10000) for _ in range(10)]  # 生成10个-10到100之间的随机数字

for Index in range(1, len(List)):
    I = Index  # 刚开始往左边走的第一个位置
    val = List[I]  # 先把当前值存起来
    while I > 0 and val < List[I - 1]:
        List[I] = List[I - 1]
        I -= 1
    List[I] = val
print(List)
```

## 希尔排序

> 希尔排序，也称递减增量排序算法，是插入排序的一种更高效的改进版本。希尔排序是非稳定排序算法。

希尔排序是基于插入排序的以下两点性质而提出改进方法的：

1. 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率
2. 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位

```python
from random import randint

List = [randint(-20, 100) for _ in range(10)]  # 生成10个-10到100之间的随机数字
Step = int(len(List) / 2)  # 将列表切分成两半

while Step >= 1:  # 如果切分的值大于等于1
    for Index in range(len(List) - Step):
        if List[Index] > List[Index + Step]:
            List[Index], List[Index + Step] = List[Index + Step], List[Index]  # 位置交换
    Step = int(Step / 2)
else:  # 进入插入排序
    for Index in range(len(List)):
        while Index > 0 and List[Index] < List[Index - 1]:
            List[Index], List[Index - 1] = List[Index - 1], List[Index]
            Index -= 1
print(List)
```

## 快速排序

1. 在数据集之中，选择一个元素作为**基准**（pivot）。
2. 所有小于**基准**的元素，都移到**基准**的左边；所有大于**基准**的元素，都移到**基准**的右边。这个操作称为分区 (partition) 操作，分区操作结束后，基准元素所处的位置就是最终排序后它的位置。
3. 对**基准**左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

```python
#!/usr/bin/env python
# _*_ coding: utf-8 _*_

from random import randint

li = [randint(-20, 100) for _ in range(10)]

def quick_sort(List, start, end):
    print(List, start, end)
    if start >= end:
        return
    Key = List[start]  # 值
    LeftIndex = start  # 左边
    RightIndex = end  # youbian
    while LeftIndex < RightIndex:  #
        while LeftIndex < RightIndex and List[RightIndex] > Key:  # 代表要继续往左边移动小旗子
            RightIndex -= 1
        List[LeftIndex], List[RightIndex] = List[RightIndex], List[LeftIndex]
        while LeftIndex < RightIndex and List[LeftIndex] <= Key:  # 左边小旗子开始向右移动
            LeftIndex += 1
        List[LeftIndex], List[RightIndex] = List[RightIndex], List[LeftIndex]
    quick_sort(List, start, LeftIndex - 1)  # 左边
    quick_sort(List, LeftIndex + 1, end)

quick_sort(li, 0, len(li) - 1)
print(li)
```