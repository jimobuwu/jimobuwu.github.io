---
title: 二分查找
date: 2016-12-04 00:57:09
tags:
categories: 算法
---
### 迭代
```c
        int binary_search(int arr[], int size, int key)
        {
            int low, high, mid;
            low = 0;
            high = size - 1;

            while (low <= high)
            {
                mid = (low + high) / 2;
                if (arr[mid] == key)
                {
                    return mid;
                }
                if (arr[mid] < key)
                {
                    low = mid + 1;
                }
                if (arr[mid] > key)
                {
                    high = mid - 1;
                }
            }

            return -1;
        }
```
<!--more-->
### 递归
```c
        int binary_search_recursion(int arr[], int key, int low, int high)
        {
            if (low > high)
                return -1;
            int mid = (low + high) / 2;
            if (arr[mid] == key)
                return mid;
            if (arr[mid] > key)
            {
                return binary_search_recursion(arr, key, low, high - 1);
            }
            else
            {
                return binary_search_recursion(arr, key, low+1, high);
            }
        }
```