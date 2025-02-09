---
layout:     post   				    # 使用的布局（不需要改）
title:      A Beautiful Restful Request and Respond Format   	# 标题 
subtitle:   Code and Principle of Bubble Sort #副标题
date:       2014-11-08 				# 时间
author:     Leonard Meng						# 作者
header-img: img/post-banner/post-banner-sorting.jpeg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true                       # 是否启用 MathJax
tags:								#标签
    - Algorithm
    - Sorting
    - Bubble Sort
---

# Introduction

Bubble Sort is the simplest sorting algorithm that works by repeatedly swapping the adjacent elements if they are in the wrong order. This algorithm is not suitable for large data sets as its average and worst case time complexity is quite high.

# How Bubble Sort Works?

![bubble-sort](https://www.menglingjun.com/img/in-post/bubble-sort-animation2.gif)

Consider an array int[] arr= {5, 1, 4, 2, 8}

1. First Pass: Bubble sort starts with very first two elements, comparing them to check which one is greater.

    ( 5 1 4 2 8 ) –> ( 1 5 4 2 8 ), Here, algorithm compares the first two elements, and swaps since 5 > 1.
    
    ( 1 5 4 2 8 ) –>  ( 1 4 5 2 8 ), Swap since 5 > 4
    
    ( 1 4 5 2 8 ) –>  ( 1 4 2 5 8 ), Swap since 5 > 2
    
    ( 1 4 2 5 8 ) –> ( 1 4 2 5 8 ), Now, since these elements are already in order (8 > 5), algorithm does not swap them.
2. Second Pass: Now, during second iteration it should look like this:

    ( 1 4 2 5 8 ) –> ( 1 4 2 5 8 ) 
    
    ( 1 4 2 5 8 ) –> ( 1 2 4 5 8 ), Swap since 4 > 2 
    
    ( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
    
    ( 1 2 4 5 8 ) –>  ( 1 2 4 5 8 ) 
3. Third Pass: Now, the array is already sorted, but our algorithm does not know if it is completed.The algorithm needs one whole pass without any swap to know it is sorted.

    ( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
    
    ( 1 2 4 5 8 ) –> ( 1 2 4 5 8 ) 
    
    ( 1 2 4 5 8 ) –> ( 1 2 4 5 8 )
    
    ( 1 2 4 5 8 ) –> ( 1 2 4 5 8 ) 

# Bubble Sort in Java

```java
public class BubbleSort {
    private static int[] bubbleSort(int[] arr){
        int size = arr.length;
        // Put biggest one to the right
        for(int i = 1; i < size; i++){
            for(int j = 0; j < size - i; j++){
                if(arr[j] > arr[j+1]){
                    int tmp = arr[j+1];
                    arr[j+1] = arr[j];
                    arr[j] = tmp;
                }
            }
        }
        return arr;
    }

    public static void main(String[] args) {
        int[] arr = {-1,0, 1, 9, 8, 7, 6, 5, 4, 3, 2, 0, -21, 32, 12, 4, 6, 8, 23,23};
        arr = bubbleSort(arr);
        for(int i = 0; i < arr.length; i++){
            System.out.println(arr[i]);
        }
    }
}

```

# Complexity Analysis
- Time Complexity: $O(n^2)$
- Space Complexity: $O(1)$
