---
title: "[ALgorithm] Merge Sort"
date: 2020-02-08
categories: [Algorithm]
tag: [sorting, mergesort, 머지소트]
---

대표적으로 O(logN)의 시간 복잡도를 가지는 소팅 알고리즘

## 방법

분할 정복(Devide & Conquer)을 이용하는 알고리즘이다.
주어진 배열을 원소가 하나 밖에 남지 않을 때 까지 1/2로 쪼갠 후에, 재귀를 이용해 돌아가며 원래 크기의 배율로 합칠 때 정렬을 수행한다.

### Example

1. 만일 아래와 같이 8개의 숫자로 이루어진 배열이 있을 경우
    [5, 2, 7, 3, 8, 1, 10, 4]
2.  하나의 배열을 둘로 쪼갠다.
    [5, 2, 7, 3] [8, 1, 10, 4]
3. 이후 두 개의 배열을 다시 각각 둘로 쪼개고
    [5, 2] [7, 3] [8, 1] [10, 4]
4. 네 개의 배열을 다시 각각 둘로 쪼개면 한개씩의 배열이 만들어진다.
    [5] [2] [7] [3] [8] [1] [10] [4]
5. 더 이상 쪼갤 수 없으므로 두 개씩 합치기를 시작한다. 합칠 때는 작은 수를 앞으로 한다.
    [2, 5] [3, 7] [1, 8] [4, 10]
6. 다시 네 개의 배열을 두 개로 합친다. 합칠 배열의 각 값을 비교하여 작은 숫자를 먼저 넣으면 정렬된 배열이 만들어진다.
    [2, 3, 5, 7] [1, 4, 8, 10]
7. 두 개의 배열을 하나로 합치면 최종적으로 정렬이 완료된 배열이 생긴다.
    [1, 2, 3, 4, 5, 7, 8, 10]

## 알고리즘의 특성

1. 알거리즘은 Split과 Merge 단계로 나뉘어 구성이 된다
2. Split은 절반씩 분해해 나가기 때문에 O(logN)의 시간 복잡도를 가지며, Merge의 경우 모든 값을 비교해야 하기 때문에 O(N) 시간 복잡도를 가진다.
즉, 총 시간 복잡도는 O(NlogN)이 된다.
3. 추가적인 공간을 가지고 해당 공간에 값을 담기 때문에, swap이 발생하지 않는다.

## 알고리즘의 구현

- 기본적으로 재귀를 이용하여 알고리즘을 구현한다.
- 배열을 더 이상 나눌 수 없을 때 까지 split한 후, merge를 수행하며, 이 때의 재귀 기저 조건은 배열의 크기가 1 (기본 배열에 따라 크기가 2보다 작을 때로 한다)일 때 이다.

```java
    public static void MergeSort(int[] arr) {
    		sort(arr, 0, arr.length);
    	}
    	
    private static void sort(int[] arr, int low, int high) {
    	if(high - low < 2) {
    		return;
    	}
    	
    	int mid = (high + low) / 2;
    	sort(arr, 0, mid);
    	sort(arr, mid, high);
    	merge(arr, low, mid, high);
    }
    
    private static void merge(int[] arr, int low, int mid, int high) {
    	int[] temp = new int[high-low];
    	int idx = 0, l = low, h = mid;
    	
    	while(l < mid && h < high) {
    		if(arr[l] < arr[h]) {
    			temp[idx++] = arr[l++];
    		}else {
    			temp[idx++] = arr[h++];
    		}
    	}
    	
    	while(l < mid) {
    		temp[idx++] = arr[l++];
    	}
    	while(h < high) {
    		temp[idx++] = arr[h++];
    	}
    	
    	for(int i = low; i < high; i++) {
    		arr[i] = temp[i-low];
    	}
    }

    int[] arr = {5, 2, 7, 3, 8, 1, 10, 4};
    MergeSort(arr);
    
    for(int i = 0; i < arr.length; i++) {
    	System.out.print(arr[i] + " ");
    }
```
	[1 2 3 4 5 7 8 10]
