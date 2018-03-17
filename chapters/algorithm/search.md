## 搜索

### 二分搜索算法

#### 算法实现

```c
int binary_search(const int arr[], int start, int end, int khey) {
    if (start > end)
        return -1;

    int mid = start + (end - start) / 2;
    if (arr[mid] > khey)
        return binary_search(arr, start, mid - 1, khey);
    else if (arr[mid] < khey)
        return binary_search(arr, mid + 1, end, khey);
    else
        return mid;
}
```

#### STL

* `std::binary_search`：Test if value exists in sorted sequence.
* `std::lower_bound`：Return iterator to lower bound.
* `std::upper_bound`：Return iterator to upper bound.
* `std::equal_range`：Get subrange of equal elements.

