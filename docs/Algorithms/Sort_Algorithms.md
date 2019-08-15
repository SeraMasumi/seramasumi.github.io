---
layout: default
title: Sort Algorithms
nav_order: 5
---

# Complexity

| Sort      | Time avg                                        | Time best                           | Time worst                                                   | Space                                           | Stable |
| --------- | ----------------------------------------------- | ----------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ------ |
| insertion | n^2(折半插入可以减少比较次数，但是移动次数不变) | n(排好序时，对于每个a[i]只比较一次) | n^2                                                          | 1                                               | Yes    |
| selection | n^2                                             | n^2                                 | n^2                                                          | 1                                               | No     |
| bubble    | n^2                                             | n(一趟没交换就可以结束)             | n^2                                                          | 1                                               | Yes    |
| merge     | nlogn                                           | nlogn                               | nlogn                                                        | n(临时数组)                                     | Yes    |
| quick     | nlogn(unifromly partition, tree hight is logn)  | nlogn                               | n^2(逆序，递归树不分叉，高n。每次扫描只比上次扫描少一个元素，故(1+…+n)~=n^2)(avoid by shuffle)( 当划分产生的两个子问题分别包含 n-1 和 0 个元素时，最坏情况发生, n/2, n/2时最好情况发生) | best = logn, worst = n(递归次数，stack所占空间) | No     |



# Code

## Insertion Sort

a[0:i-1] done, insert a[i] to a[0:i-1] (using swap).

```java
private static void insertionSort(int[] array) {
    for(int i = 1; i < array.length; i++) {
        for(int j = i; j > 0; j--) {
            if(array[j] < array[j-1]) {
                int temp = array[j];
                array[j] = array[j-1];
                array[j-1] = temp;
            }
        }
    }
}
```



## Selection Sort

a[0:i-1] done, select min a[j] from a[i:n-1], swap with a[i].

```java
private static void selectionSort(int[] array) {
    for(int i = 0; i < array.length; i++) {
        int smallestIndex = i;
        for(int j = i + 1; j < array.length; j++) {
            if(array[j] < array[smallestIndex]) {
                smallestIndex = j;
            }
        }
        int temp = array[i];
        array[i] = array[smallestIndex];
        array[smallestIndex] = temp;
    }
}
```



## Bubble Sort

a[i:n-1] done, swap adjacent elements in a[0:i-1] if a[j] > a[j+1].

```java
private static void bubbleSort(int[] array) {
    for(int i = array.length - 1; i > 0; i--) {
        for(int j = 0; j < i; j++) {
            if(array[j] > array[j+1]) {
                int temp = array[j];
                array[j] = array[j+1];
                array[j+1] = temp;
            }
        }
    }
}
```



## Merge Sort

```java
private static void mergeSort(int[] array, int left, int right) {
    if(left >= right) {
        return;
    }
    int mid = left + (right - left) / 2;
    mergeSort(array, left, mid);
    mergeSort(array, mid + 1, right);
    merge(array, left, mid, right);
}

private static void merge(int[] array, int left, int mid, int right) {

    int lenLeft = mid - left + 1;
    int lenRight = right - mid;

    int[] tempLeft = new int[lenLeft];
    int[] tempRight = new int[lenRight];

    for(int i = 0; i < lenLeft; i++) {
        tempLeft[i] = array[i + left];
    }

    for(int i = 0; i < lenRight; i++) {
        tempRight[i] = array[mid + i + 1];
    }

    int i = 0;
    int j = 0;
    int index = left;

    while(i < lenLeft && j < lenRight) {
        if(tempLeft[i] < tempRight[j]) {
            array[index++] = tempLeft[i++];
        } else {
            array[index++] = tempRight[j++];
        }
    }

    while(i < lenLeft) {
        array[index++] = tempLeft[i++];
    }

    // 其实不需要这一part，因为array里后半部分顺序本来就和tempRight的一样
    // while(j < lenRight) {
    //     array[index++] = tempRight[j++];
    // }
}
```



## Quick Sort

choose one element as pivot, move a[i] <= pivot to left, a[i] >= pivot to right, then recursively solve two part.

```java
private static void quickSort(int[] array, int left, int right) {
    if(left >= right) {
        return;
    }

    int i = left; 
    int j = right;
    int pivot = array[left + (right - left) / 2];

    while(i <= j) {
        while(i <= j && array[i] < pivot) {
            i++;
        }
        while(i <= j && array[j] > pivot) {
            j--;
        }
        if(i <= j) {
            int temp = array[i];
            array[i] = array[j];
            array[j] = temp;
            i++;
            j--;
        }
    }

    quickSort(array, left, j);
    quickSort(array, i, right);
}
```

