---
title: "[ALgorithm] Quick Sort"
date: 2020-02-08
categories: [Algorithm]
tag: [sorting, quicksort, 퀵소트]
---

대표적으로 O(logN)의 시간 복잡도를 가지는 소팅 알고리즘

## 방법

분할 정복(Devide & Conquer)을 이용하는 알고리즘이다.
하나의 피봇(pivot)을 기준으로, 피봇 보다 작은 값들은 왼쪽에, 큰 값들을 왼쪽으로 옮기는 작업을 
재귀로 수행한다.
병합 정렬(Merge Sort)의 경우 분할을 한 후에 병합이 시작되며 값의 비교가 시작되나,
퀵 정렬의 경우 분할 시점에 비교 연산이 수행되기 때문에 이론적으로는 병합 정렬보다 빠르게 동작할 수 있다.
Python이나 Java의 리스트 정렬 알고리즘은 기본적으로 퀵 정렬을 사용한다.

### Example

1. 만일 아래와 같이 8개의 숫자로 이루어진 배열이 있을 경우
    [5, 2, 7, 3, 1, 10, 4]
2. 하나의 피봇을 정한다. 여기에서는 가운데 값인 3으로 정한다
    [5, 2, 7] [3] [ 1, 10, 4]
     ^                     ^
     left index            right index
3. 피봇 3을 기준으로 left, right index가 교차 할 때 까지 옮기며 왼쪽이 피봇 보다 작고, 오른 쪽이 피봇 보다 클 경우 서로의 위치를 교환한다.
    [5, 2, 7] [3] [1, 10, 4]
     ^             ^
    left           right
    [1, 2, 7] [3] [5, 10, 4]
           ^   ^
           left right
    [1, 2, 3] 7 [5, 10, 4]	
4. 이후 left와 right가 교차 할 경우 left의 값을 return하여, 이를 기준으로 분할 후 다시 비교를 시작한다. 여기서 왼쪽 배열은 이미 정렬이 되었으므로 더 진행되지 않는다.
    [1, 2, 3] [7, 5, 10, 4]
5. 오른 쪽 배열 역시 동일하게 피보팅을 잡고 진행한다. 
    [7, 5] [10] [4]
    [7, 5, 4] [10]	
6. 오른 쪽은 하나밖에 없으므로 정렬이 끝난다. 마지막 배열에 대해서 정렬을 수행한다.
    [7, 5, 4]
    [7] [5] [4]
    [4, 5, 7]	
7. 각 배열을 모두 보면 정렬이 완료되었다.
    [1, 2, 3, 4, 5, 7, 10]
## 알고리즘의 특성

1. 피봇이 어느 값으로 선택되냐에 따라 O(N^2)의 시간 복잡도를 가질 수 있다. (최악의 경우)

## 알고리즘의 구현

- 기본적으로 재귀를 이용하여 알고리즘을 구현한다.

```java
    void QuickSort(int[] arr){
    	sort(arr, 0, arr.length-1);
    }
    
    void sort(int[] arr, int left, int right) {
        if (left>= right) return;
    
        int mid = partition(arr, left, right);
        sort(arr, left, mid- 1);
        sort(arr, mid, right);
    }
    
    int partition(int[] arr, int left, int right){
    	int pivot = arr[(left+right) / 2];
    	while(left <= right){
    		while(arr[left] < pivot) left++;
    		while(arr[right] > pivot) right--;
    		if(left <= rght){
    			swap(arr, left, right);
    			left++;
    			right--;
    		}
    	}
    	return left;
    }
    
    void swap(int[] arr, int left, int right){
    	int temp = arr[left];
    	arr[left] = arr[right];
    	arr[right] = temp;
    }

    int[] arr = {5, 2, 7, 3, 1, 10, 4};
    QuickSort(arr);
    
    for(int i = 0; i < arr.length; i++) {
    	System.out.print(arr[i] + " ");
    }
```    
    [1 2 3 4 5 7 10]
