## 比较排序

| **排序方法** | **平均时间复杂度** | **最优时间复杂度** | **最坏时间复杂度** | **辅助空间** | **稳定性** |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 冒泡排序 | $$O(n^2)$$ | $$O(n)$$ | $$O(n^2)$$ | $$O(1)$$ | 稳定 |
| 简单选择排序 | $$O(n^2)$$ | $$O(n^2)$$ | $$O(n^2)$$ | $$O(1)$$ | 不稳定 |
| 直接插入排序 | $$O(n^2)$$ | $$O(n)$$ | $$O(n^2)$$ | $$O(1)$$ | 稳定 |
| 希尔排序 | - | $$O(n)$$ | $$O(n{\log^2 n})$$ | $$O(1)$$ | 不稳定 |
| 堆排序 | $$O(n{\log n})$$ | $$O(n{\log n})$$ | $$O(n{\log n})$$ | $$O(1)$$ | 不稳定 |
| 归并排序 | $$O(n{\log n})$$ | $$O(n{\log n})$$ | $$O(n{\log n})$$ | $$O(n)$$ | 稳定 |
| 快速排序 | $$O(n{\log n})$$ | $$O(n{\log n})$$ | $$O(n^2)$$ | $$O({\log n})$$~$$O(n)$$ | 不稳定 |

### 冒泡排序

![](/assets/bubble-sort.gif)

### 简单选择排序

![](/assets/selection-sort.gif)

黄色是已排序的元素。红色是未排序的元素中最小的元素。蓝色是当前元素。

### 直接插入排序

![](/assets/insertion-sort.gif)

### 希尔排序

**希尔排序**，也称**递减增量排序算法**，是插入排序的一种更高效的改进版本。希尔排序是不稳定排序算法。

**希尔排序**是基于插入排序的以下两点性质而提出改进方法的：

* 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率
* 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位

### 归并排序

![](/assets/merge-sort.gif)

#### 算法描述

1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列
2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置
3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置
4. 重复步骤3直到某一指针到达序列尾
5. 将另一序列剩下的所有元素直接复制到合并序列尾

#### 算法实现

```cpp
template<typename T>
void merge_sort_recursive(T arr[], T reg[], int start, int end) {
	if (start >= end)
		return;
	int len = end - start, mid = (len >> 1) + start;
	int start1 = start, end1 = mid;
	int start2 = mid + 1, end2 = end;
	merge_sort_recursive(arr, reg, start1, end1);
	merge_sort_recursive(arr, reg, start2, end2);
	int k = start;
	while (start1 <= end1 && start2 <= end2)
		reg[k++] = arr[start1] < arr[start2] ? arr[start1++] : arr[start2++];
	while (start1 <= end1)
		reg[k++] = arr[start1++];
	while (start2 <= end2)
		reg[k++] = arr[start2++];
	for (k = start; k <= end; k++)
		arr[k] = reg[k];
}

template<class T>
void merge_sort(T arr[], const int len) {
	T reg = new T[len];
	merge_sort_recursive(arr, reg, 0, len - 1);
	delete[] reg;
}
```

### 快速排序

#### 算法描述

1. 从数列中挑出一个元素，称为"基准"（pivot）
2. 重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（相同的数可以到任何一边）。在这个分区结束之后，该基准就处于数列的中间位置。这个称为分区（partition）操作
3. 递归地（recursively）把小于基准值元素的子数列和大于基准值元素的子数列排序

#### 算法实现

```cpp
template <class T>
void quick_sort_recursive(T arr[], int start, int end) {
    if (start >= end)
        return;
    T mid = arr[end];
    int left = start, right = end - 1;
    while (left < right) {
        while (arr[left] < mid && left < right)
            left++;
        while (arr[right] >= mid && left < right)
            right--;
        std::swap(arr[left], arr[right]);
    }
    if (arr[left] < arr[end])
        left++;
    else
        std::swap(arr[left], arr[end]);

    quick_sort_recursive(arr, start, left - 1);
    quick_sort_recursive(arr, left + 1, end);
}

template <class T>
void quick_sort(T arr[], int len) {
    quick_sort_recursive(arr, 0, len - 1);
}
```

#### 算法效率

* 快速排序的代码紧凑，常数因子小，局部性良好
* 归并排序需要额外空间大，是一种稳定快速的排序方法；在外部排序的情况下，比快速排序更好，原因是快速排序依赖的是数据的随机存取速度，而归并是顺序存取，对外存比较友好
* 堆排序的局部性差导致缓存命中率低



