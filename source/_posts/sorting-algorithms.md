---
title: 各种排序算法总结
date: 2020-09-16 14:00:00
categories: 算法
tags:
  - 排序
  - 算法
permalink: sorting_algorithms/
---

## 各种排序算法总结

本文主要是为了总结各种算法的写法，复杂度，以及稳定性分析。

### 排序算法时间、空间、稳定性表

| 算法 | 最好 | 最坏 | 平均 | 空间 | 稳定性 |
|------|------|------|------|------|--------|
| 冒泡排序 | O(n) | O(n²) | O(n²) | O(1) | 稳定 |
| 选择排序 | O(n²) | O(n²) | O(n²) | O(1) | 不稳定 |
| 插入排序 | O(n) | O(n²) | O(n²) | O(1) | 稳定 |
| 快速排序 | O(nlog n) | O(n²) | O(nlog n) | O(log n)-O(n) | 不稳定 |
| 归并排序 | O(nlog n) | O(nlog n) | O(nlog n) | O(n) | 稳定 |
| 希尔排序 | O(n^1.3) | O(n²) | O(nlog n)-O(n²) | O(1) | 不稳定 |
| 计数排序 | O(n+k) | O(n+k) | O(n+k) | O(n+k) | 稳定 |
| 基数排序 | O(nk) | O(nk) | O(nk) | O(n+k) | 稳定 |
| 桶排序 | O(n) | O(n) | O(n) | O(n+m) | 稳定 |
| 堆排序 | O(nlog n) | O(nlog n) | O(nlog n) | O(1) | 不稳定 |

注：稳定性是指：假定在待排序的记录序列中，存在多个具有**相同的关键字**的记录，若经过排序，这些记录的**相对次序**保持不变，即在原序列中，ri=rj，且ri在rj之前，而在排序后的序列中，ri仍在rj之前，则称这种排序算法是稳定的；否则称为不稳定的。

<!-- more -->

## 冒泡排序 Bubble Sort

### 概述

> 作为本人踏入算法殿堂学习的第一个排序算法，冒泡排序可以算是经典算法之一了，它的基本思想是：*两个数比较大小，较大的数下沉，较小的数冒起来*。

1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

### 图示

![冒泡排序](https://www.runoob.com/wp-content/uploads/2019/03/bubbleSort.gif)

### Python 算法实现

```python
def bubble(array):
    for i in range(len(array)):
        for j in range(len(array)-i-1):
            if array[j] > array[j+1]:
                array[j], array[j+1] = array[j+1], array[j]
    return array
```

### 复杂度

平均时间复杂度 O(n²)

## 选择排序 Selection Sort

### 概述

1. 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。
2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
3. 重复第二步，直到所有元素均排序完毕。

### 图示

![选择排序](https://www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif)

### Python 算法实现

```python
def selection(array):
    for i in range(len(array)):
        low = i
        for j in range(i+1, len(array)):
            if array[j] < array[low]:
                low = j
        array[i], array[low] = array[low], array[i]
    return array
```

### 复杂度

平均时间复杂度 O(n²)

## 插入排序 Insertion Sort

### 概述

> 插入排序是一种最简单直观的排序算法，它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

1. 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。
2. 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。

### 图示

![插入排序](https://www.runoob.com/wp-content/uploads/2019/03/insertionSort.gif)

### Python 算法实现

```python
def insertion(array):
    for i in range(1, len(array)):
        val = array[i]
        j = i-1
        while j >= 0 and val < array[j]:
            array[j+1] = array[j]
            j-=1
        array[j+1] = val
    return array
```

### 复杂度

平均时间复杂度 O(n²)

## 快速排序 Quick Sort

### 概述

> 快速排序的最坏运行情况是 O(n²)，比如说顺序数列的快排。但它的平摊期望时间是 O(nlogn)，且 O(nlogn) 记号中隐含的常数因子很小，比复杂度稳定等于 O(nlogn) 的归并排序要小很多。

1. 从数列中挑出一个元素，称为 "基准"（pivot）;
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。这个称为分区（partition）操作；
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；

### 图示

![快速排序](https://www.runoob.com/wp-content/uploads/2019/03/quickSort.gif)

### Python 算法实现

```python
def partition(arr, low, high):
    pivot = arr[high]
    r = low - 1
    for i in range(low, high):
        if arr[i] < pivot:
            r += 1
            arr[r], arr[i] = arr[i], arr[r]
    arr[r+1], arr[high] = arr[high], arr[r+1]
    return r+1

def quicksort(arr, left, right):
    if left >= right:
        return
    pivot = partition(arr, left, right)
    quicksort(arr, left, pivot - 1)
    quicksort(arr, pivot + 1, right)
```

### 复杂度

平均时间复杂度 O(nlog n)

## 归并排序 Merge Sort

### 概述

> 归并排序（Merge sort）是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。

1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；
2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置；
3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；
4. 重复步骤 3 直到某一指针达到序列尾；
5. 将另一序列剩下的所有元素直接复制到合并序列尾。

### 图示

![归并排序](https://www.runoob.com/wp-content/uploads/2019/03/mergeSort.gif)

### Python 算法实现

```python
def merge(array, l, r):
    i = j = k = 0
    while i < len(l) and j < len(r):
        if l[i] < r[j]:
            array[k] = l[i]
            i += 1
        else:
            array[k] = r[j]
            j += 1
        k += 1

    while i < len(l):
        array[k] = l[i]
        i += 1
        k += 1

    while j < len(r):
        array[k] = r[j]
        j+= 1
        k+= 1

    return array

def mergesort(array):
    n = len(array)
    if n == 1:
        return array
    left = mergesort(array[:n//2])
    right = mergesort(array[n//2:])
    return merge(array, left, right)
```

### 复杂度

平均时间复杂度 O(nlog n)

## Reference

- [排序算法总结 | 菜鸟教程](https://www.runoob.com/w3cnote/sort-algorithm-summary.html)
- [1.0 十大经典排序算法 | 菜鸟教程](https://www.runoob.com/w3cnote/ten-sorting-algorithm.html)
- [排序算法的稳定性及其意义](https://blog.csdn.net/u012501054/article/details/79342580)
