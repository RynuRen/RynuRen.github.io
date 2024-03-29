---
layout: jupyter
title: 정렬(3) - 퀵, 힙, 병합
published: true
date: 2023-01-12
description: 정렬(3) - 퀵, 힙, 병합
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: 정렬(3) - 퀵, 힙, 병합
use_math: true
---
---
# 정렬

## 4. 시간복잡도 $$nlogn$$

### 퀵 정렬

* 퀵 정렬(Quick sort)-불안정  
    배열의 한 요소를 피벗(pivot)으로 골라 피벗을 기준으로 작은지 큰지에 따라 두 파티션으로 계속 나눠가며 정렬  
    정렬된 리스트에 대해서는 퀵 정렬의 불균형 분할에 의해 오히려 수행시간이 더 많이 걸림($$O(n^2)$$)
    
    ![img]({{ '/assets/images/2023/01/12/QuickSort.gif' | relative_url }})

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
def quick_sort(arr):
    def qsort(low, high):
        # 정렬할 범위가 1개 이하일 경우(low에서 high까지)
        if high - low <= 0:
            return

        mid = partition(low, high) # pivot 기준으로 비균등 분할 -분할(Divide)
        qsort(low, mid - 1) # pivot 기준 앞쪽 리스트 정렬 -정복(Conquer)
        qsort(mid, high) # pivot 기준 뒤쪽 리스트 정렬 -정복(Conquer)

    def partition(low, high):
        pivot = arr[(low + high) // 2]
        print(f'[{low}:{high}] piv {pivot}', end='\t> ')
        while low <= high:
            while arr[low] < pivot:
                low += 1
            while arr[high] > pivot:
                high -= 1
            if low <= high:
                arr[low], arr[high] = arr[high], arr[low]
                low, high = low + 1, high - 1
        print(*arr)
        return low

    return qsort(0, len(arr) - 1)

num = [50, 2, 70, 4, 3, 10, 8]
print('최초\t\t:', *num)

quick_sort(num)
print('최종\t\t:', *num)
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>

{:.output_stream}

```
최초		: 50 2 70 4 3 10 8
[0:6] piv 4	> 3 2 4 70 50 10 8
[0:2] piv 2	> 2 3 4 70 50 10 8
[1:2] piv 3	> 2 3 4 70 50 10 8
[3:6] piv 50	> 2 3 4 8 10 50 70
[3:4] piv 8	> 2 3 4 8 10 50 70
[5:6] piv 50	> 2 3 4 8 10 50 70
최종		: 2 3 4 8 10 50 70

```

### 힙 정렬

* 힙 정렬(Heap sort)  
    전체 자료를 정렬하는 것이 아니라 가장 큰 값 몇개만 필요할 때 유용
    
    ![gif]({{ '/assets/images/2023/01/12/HeapSort.gif' | relative_url }})

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
def heap_sort(arr):
    def heapify(lst, idx, heap_size):
        # 힙 성질을 만족하는지 확인하는 함수(부모노드는 자식노드보다 크거나 같다)
        print('unsorted\t:', *lst)
        largest = idx # 부모노드
        l_idx = idx * 2 + 1 # 왼쪽 자식노드
        r_idx = idx * 2 + 2 # 오른쪽 자식노드
        if l_idx < heap_size and lst[l_idx] > lst[largest]:
            largest = l_idx
        if r_idx < heap_size and lst[r_idx] > lst[largest]:
            largest = r_idx
        # 힙 성질을 만족하지 못한 경우
        # 부모노드와 해당 자식노드의 위치를 바꾸고 아래 노드의 heapify 확인
        if largest != idx:
            lst[largest], lst[idx] = lst[idx], lst[largest]
            heapify(lst, largest, heap_size)

    def hsort(lst):
        print('before max heap\t', *lst)
        n = len(lst)
        # build heap: 최대 힙 구성시 배열의 중간부터->이진트리 성질이용
        for i in range(n//2-1, -1, -1):
            heapify(lst, i, n)
        print('after max heap\t', *lst)
        
        # 최댓값(루트값)을 배열의 끝에서부터 정렬
        # (첫번째 값과 정렬되지 않은 마지막 값과 자리를 바꿈)
        # 새 루트노드에 대해 최대 힙 재구성
        for i in range(n-1, 0, -1):
            lst[0], lst[i] = lst[i], lst[0]
            heapify(lst, 0, i)
        return lst
    
    return hsort(arr)

num = [50, 2, 70, 4, 3, 10, 8]
print('최초\t:', *num)

num = heap_sort(num)
print('최종\t:', *num)
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>

{:.output_stream}

```
최초	: 50 2 70 4 3 10 8
before max heap	 50 2 70 4 3 10 8
unsorted	: 50 2 70 4 3 10 8
unsorted	: 50 2 70 4 3 10 8
unsorted	: 50 4 70 2 3 10 8
unsorted	: 50 4 70 2 3 10 8
unsorted	: 70 4 50 2 3 10 8
after max heap	 70 4 50 2 3 10 8
unsorted	: 8 4 50 2 3 10 70
unsorted	: 50 4 8 2 3 10 70
unsorted	: 50 4 10 2 3 8 70
unsorted	: 8 4 10 2 3 50 70
unsorted	: 10 4 8 2 3 50 70
unsorted	: 3 4 8 2 10 50 70
unsorted	: 8 4 3 2 10 50 70
unsorted	: 2 4 3 8 10 50 70
unsorted	: 4 2 3 8 10 50 70
unsorted	: 3 2 4 8 10 50 70
unsorted	: 2 3 4 8 10 50 70
최종	: 2 3 4 8 10 50 70

```

### 병합 정렬

* 병합;합병 정렬(Merge sort)-안정  
    배열을 계속 2등분하여 잘게 쪼갠 뒤 하나의 임시 배열에 병합하는 과정에서 값을 비교하며 정렬  
    크기가 큰 경우에는 이동 횟수가 많으므로 매우 큰 시간적 낭비를 초래, 임시 배열이 필요
    
    ![img]({{ '/assets/images/2023/01/12/MergeSort.gif' | relative_url }})

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
def merge_sort(arr):
    def msort(arr):
        if len(arr) <= 1:
            return arr
        
        mid = len(arr) // 2 # 중간 위치를 계산, 리스트를 균등 분할 -분할(Divide)
        low = msort(arr[:mid]) # 앞쪽 리스트 정렬 -정복(Conquer)
        high = msort(arr[mid:]) # 뒤쪽 리스트 정렬 -정복(Conquer)
        return merge(low, high) # 정렬된 2개의 부분 배열을 합병 -결합(Combine)

    def merge(low, high):
        temp=[]

        # temp리스트에 두 리스트의 값을 처음부터 비교하며 정렬
        while len(low) > 0 and len(high) > 0:
            if low[0] <= high[0]:
                temp.append(low.pop(0))
            else:
                temp.append(high.pop(0))

        if len(low) > 0: # 남는 앞쪽 리스트 복사
            temp += low
        # if len(high) > 0:
        else: # 남는 뒤쪽 리스트 복사
            temp += high
        print(f'조각\t:', *temp)
        return temp

    return msort(arr)

num = [50, 2, 70, 4, 3, 10, 8]
print('최초\t:', *num)

num = merge_sort(num)
print('최종\t:', *num)
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
최초	: 50 2 70 4 3 10 8
조각	: 2 70
조각	: 2 50 70
조각	: 3 4
조각	: 8 10
조각	: 3 4 8 10
조각	: 2 3 4 8 10 50 70
최종	: 2 3 4 8 10 50 70

```

위의 코드로는 list 슬라이싱과 list pop(0)에 의해 시간복잡도가 $$nlogn$$이 될 수 없다.  
참고: [파이썬 자료형 및 연산자의 시간 복잡도](https://chancoding.tistory.com/43)

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
def merge_sort(arr):
    def msort(low, high):
        if high - low < 2: # 리스트 원소가 1개 남을 때까지 분할(low에서 high-1까지)
            return
        
        mid = (low + high) // 2 # 중간 위치를 계산, 리스트를 균등 분할 -분할(Divide)
        msort(low, mid) # 앞쪽 리스트 정렬 -정복(Conquer)
        msort(mid, high) # 뒤쪽 리스트 정렬 -정복(Conquer)
        merge(low, mid, high) # 정렬된 2개의 부분 배열을 합병 -결합(Combine)

    def merge(low, mid, high):
        temp = []
        l, h = low, mid # l:앞쪽 리스트 idx, h:뒤쪽 리스트 idx
        
        # temp리스트에 두 리스트의 값을 처음부터 비교하며 정렬
        while l < mid and h < high:
            if arr[l] < arr[h]:
                temp.append(arr[l])
                l += 1
            else:
                temp.append(arr[h])
                h += 1

        while l < mid: # 남는 앞쪽 리스트 복사
            temp.append(arr[l])
            l += 1
        while h < high: # 남는 뒤쪽 리스트 복사
            temp.append(arr[h])
            h += 1
        print(f'*조각*\t:', temp)
        for i in range(low, high):
            arr[i] = temp[i - low]
            print('After\t:', *arr)
    
    return msort(0, len(arr))

num = [50, 2, 70, 4, 3, 10, 8]
print('최초\t:', *num)

merge_sort(num)
print('최종\t:', *num)
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
최초	: 50 2 70 4 3 10 8
*조각*	: [2, 70]
After	: 50 2 70 4 3 10 8
After	: 50 2 70 4 3 10 8
*조각*	: [2, 50, 70]
After	: 2 2 70 4 3 10 8
After	: 2 50 70 4 3 10 8
After	: 2 50 70 4 3 10 8
*조각*	: [3, 4]
After	: 2 50 70 3 3 10 8
After	: 2 50 70 3 4 10 8
*조각*	: [8, 10]
After	: 2 50 70 3 4 8 8
After	: 2 50 70 3 4 8 10
*조각*	: [3, 4, 8, 10]
After	: 2 50 70 3 4 8 10
After	: 2 50 70 3 4 8 10
After	: 2 50 70 3 4 8 10
After	: 2 50 70 3 4 8 10
*조각*	: [2, 3, 4, 8, 10, 50, 70]
After	: 2 50 70 3 4 8 10
After	: 2 3 70 3 4 8 10
After	: 2 3 4 3 4 8 10
After	: 2 3 4 8 4 8 10
After	: 2 3 4 8 10 8 10
After	: 2 3 4 8 10 50 10
After	: 2 3 4 8 10 50 70
최종	: 2 3 4 8 10 50 70

```

# Reference

* 위키백과: [정렬 알고리즘](https://ko.wikipedia.org/wiki/%EC%A0%95%EB%A0%AC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
* DaleSeo: [파이썬 알고리즘](https://www.daleseo.com/?tag=Python&page=4)
* Code Pumpkin: [Sorting Algorithm](https://codepumpkin.com/bubble-sort/)
* 애플리케이션: [Algorithms](http://algorithm.wiki/ko/app/)
