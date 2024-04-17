---
layout: single
title: "Sort Algorithm"
category: Algorithm
tags:
    - Bubble Sort
    - Selection Sort
    - Insertion Sort
    - Quick Sort
    - Heap Sort
    - Radix Sort
    - Counting Sort
toc: true
toc_sticky: true
---

# 버블 정렬(Bubble Sort)
> **양 옆에 위치한 두 값을 비교**하면서 크기 순으로 정렬한다.

## 정렬 과정
첫 번째 요소부터 마지막 요소까지 연속된 두 값 a, b에 대해 b가 a보다 작으면(또는 크면) 위치를 바꿔 정렬한다.

## 특징
- 배열의 뒤에서부터 정렬된다.
- 데이터의 개수가 *n*일 때 $O(n^2)$의 시간복잡도를 가지며 비교적 느리지만 별도의 메모리 공간이 필요하지 않다.
<div align="center">
<img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/89a8a65a-d353-42bb-bd7e-cae73245c3ff" alt="bubble sort" width="300px">
</div>

## 구현
```python
def bubble_sort(arr):
    n = len(arr)

    # 모든 배열 요소 순회
    for i in range(n):
        swapped = False

        # 뒤에서 i번째 요소까지는 정렬된 상태이므로 n-i-1까지 정렬
        for j in range(0, n-i-1): 

            # 큰 수가 앞에 있다면 정렬 작은 수와 교환
            if arr[j] > arr[j+1]: 
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True

        # 모두 정렬되었다면 종료
        if (swapped == False): 
            break
```

# 선택 정렬(Selection Sort)
> 배열을 순회하면서 배열의 앞에서부터 차례대로 **각 인덱스에 들어갈 값을 선택해 위치**시킨다.

## 정렬 과정
1. 배열의 정렬되지 않은 부분에서 최솟값(또는 최댓값)을 찾는다.
2. 최솟값(또는 최댓값)을 정렬되지 않은 부분의 첫 번째 요소와 바꾼다.
3. 정렬된 부분의 경계를 오른쪽으로 한 칸 이동한다.
4. 배열이 완전히 정렬될 때까지 1~3단계를 반복한다.

<div align="center">
    <img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/dd1e6b5a-0dee-4ae6-a0ad-ef55ac6b6693" alt="selection sort">
</div>

## 특징
- 앞에서부터 정렬된다.
- 동일한 값을 가진 원소들의 본래 순서가 보장되지 않는 불안정정렬(unstable sort)이다.
- 기존 배열 이외의 추가적인 메모리를 거의 사용하지 않는 제자리 정렬(in-place sort)이다.
- 배열을 정렬하는데 필요한 swap 횟수를 최소화한다.
- $O(n^2)$의 시간 복잡도를 가지므로 데이터의 규모가 클 경우 비효율적이다.

## 구현
```python
# 모든 배열 요소 순회
def selection_sort(arr)
for i in range(len(arr)): 
      
    # 배열의 정렬되지 않은 부분에서 가장 작은 요소 탐색
    min_idx = i 
    for j in range(i+1, len(arr)): 
        if arr[min_idx] > arr[j]: 
            min_idx = j 
              
    # i번째 최솟값을 i번째 요소와 교환  
    arr[i], arr[min_idx] = arr[min_idx], arr[i] 
```

# 삽입 정렬(Insertion Sort)
> 배열을 앞에서부터 순회하면서 **정렬된 부분의 적절한 위치에 값을 삽입**한다.

## 정렬 과정

<div align="center">
    <img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/17b2dc19-f869-4935-a96d-fa4cc590a63b" alt="insertion sort" width="300px">
</div>

## 특징
- 중복된 값을 가지는 요소의 순서가 유지되므로 안정정렬(stable-sort)이다.
- 기존 배열 이외의 추가적인 메모리를 거의 사용하지 않는 제자리 정렬(in-place sort)이다.
- 이미 정렬되어 있는 경우나 자료의 수가 적은 경우에 효율적이다.

## 구현
```python
def insertion_sort(arr):

    # 2번째 요소부터 마지막 요소까지 순회
    for i in range(1, len(arr)):

        key = arr[i]

        # key값보다 큰 요소를 key값의 앞으로 이동
        j = i-1
        while j >= 0 and key < arr[j] :
                arr[j + 1] = arr[j]
                j -= 1
        arr[j + 1] = key
```

# 합병 정렬(Merge Sort)
> **분할 정복** 알고리즘으로 배열을 쪼개고 정렬하면서 하나로 합병한다.

## 정렬 과정
- 분할(Divide): 리스트를 두 개의 리스트로 분할한다.
- 정복(Conquer): 분할된 리스트를 정렬한다.
- 결합(Combine): 정렬된 두 개의 리스트를 하나의 정렬된 리스트로 결합한다.
    1. 두 개의 리스트의 값을 처음부터 비교해서 작은 값을 새로운 리스트로 옮긴다.
    2. 두 개의 리스트 중 하나의 리스트가 끝날 때까지 반복한다.
    3. 하나의 리스트가 끝나게 되면 남은 리스트의 값을 모두 새로운 리스트로 옮긴다.
<div align="center">
    <img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/31cc083d-1904-4af1-9517-53288edfc19c" alt="merge sort">
</div>

## 구현
```python
def merge(arr1,arr2):
    i, j = 0, 0
    result=[]
    while(i<len(arr1) and j<len(arr2)):
        if arr2[j]>arr1[i]:
            result.append(arr1[i])
            i+=1
        else:
            result.append(arr2[j])
            j+=1
    while(i<len(arr1)):
        result.append(arr1[i])
        i+=1
    while(j<len(arr2)):
        result.append(arr2[j])
        j+=1
    
    return result
    
def merge_sort(arr):
    if len(arr)<=1:
        return arr
    mid = len(arr)//2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left,right)
```

# 퀵 정렬(Quick Sort)
> **분할 정복** 알고리즘으로 **피봇**보다 작은 값으로 구성된 배열과 피봇보다 큰 값으로 구성된 배열로 분할해 정렬한다.

## 정렬 과정

<div align="center">
    <img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/5214131e-eb05-4228-9925-150d231adb97" alt="quick sort">
</div>

## 구현
```python
def partition(array, low, high):

    # 가장 오른쪽 요소를 피봇으로 설정
    pivot = array[high]

    # 최댓값을 가리키는 변수
    i = low - 1

    # 배열의 모든 요소를 순회하며 피봇과 비교
    for j in range(low, high):
        if array[j] <= pivot:

            # 피봇보다 작은 요소를 찾으면 i가 가리키는 최댓값과 교환
            i = i + 1

            # i번째 요소와 j번째 요소 교환
            (array[i], array[j]) = (array[j], array[i])

    # 피봇을 i가 가리키는 최댓값과 교환
    (array[i + 1], array[high]) = (array[high], array[i + 1])

    # 분할이 끝난 위치 반환
    return i + 1

def quicksort(array, low, high):
    if low < high:

        # 피봇 요소를 탐색하여 피봇보다 작은 요소는 왼쪽, 큰 요소는 오른쪽에 오게 한다
        pi = partition(array, low, high)

        # 피봇의 왼쪽 배열에 대해 재귀호출
        quicksort(array, low, pi - 1)

        # 피봇의 오른쪽 배열에 대해 재귀호출
        quicksort(array, pi + 1, high)
```

# 힙 정렬(Heap Sort)
> 최대 힙이나 최소 **힙 자료구조를 이용**해 정렬한다.
<div align="center">
    <img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/c217c916-2cec-4280-a226-2d06ecaae627" alt="heap sort">
</div>

## 힙 생성 알고리즘(heapify)
> 특정 노드의 두 자식 노드 중 우선순위가 더 높은 자식 노드와 위치를 교환하는 방식

## 구현
```python
def heapify(arr, N, i):
    largest = i  # 최댓값을 루트로 설정
    l = 2 * i + 1     # left = 2*i + 1
    r = 2 * i + 2     # right = 2*i + 2

    # 루트의 왼쪽 자식이 존재하고 루트보다 큰 수가 있는지 확인
    if l < N and arr[largest] < arr[l]:
        largest = l

    # 루트의 오른쪽 자식이 존재하고 루트보다 큰 수가 있는지 확인
    if r < N and arr[largest] < arr[r]:
        largest = r

    # 필요할 경우 루트 변경
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]

        # 배열을 힙으로 만들기
        heapify(arr, N, largest)

def heap_sort(arr):
    N = len(arr)

    # 최대 힙 생성
    for i in range(N//2 - 1, -1, -1):
        heapify(arr, N, i)

    # 하나씩 요소 추출
    for i in range(N-1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)
```

# 기수 정렬(Radix Sort)
> 비교하지 않는 정렬 알고리즘으로 낮은 **자릿수**부터 정렬한다.

## 정렬 과정
숫자별로 **버킷(bucket)**이라는 큐를 생성하고 정렬하려는 숫자들의 각 자릿수에 해당하는 숫자를 각각의 버킷에 넣어 정렬하고 이를 자릿수만큼 반복한다.
<div align="center">
    <img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/73412d9a-a1cf-4f51-9746-a5bd7e7921d8" alt="radix sort">
</div>

## 특징
- 데이터의 개수 $n$, 최대 자릿수를 $d$일 때 $O(dn)$의 시간복잡도를 가지며 비교연산을 하지 않아 정렬 속도가 빠르다.
- 버킷을 구성하기 위한 추가 메모리가 필요하고, 정렬할 수 있는 데이터 타입이 한정적이다.

## 구현
```python
from collections import deque

def radixSort():
    nums = list(map(int, input().split(' ')))
    buckets = [deque() for _ in range(10)]
    
    max_val = max(nums)
    queue = deque(nums)
    digit = 1 # 자릿수
    
    while (max_val >= digit): # 가장 큰 수의 자릿수일 때 까지만 실행
        while queue:
            num = queue.popleft()
            buckets[(num // digit) % 10].append(num) # 각 자리의 수(0~9)에 따라 버킷에 num을 넣는다.
        
        # 해당 자릿수에서 버킷에 다 넣었으면, 버킷에 담겨있는 순서대로 꺼내와 정렬한다.
        for bucket in buckets:
            while bucket:
                queue.append(bucket.popleft())
        print(digit,"의 자릿 수 정렬: ", list(queue))
        digit *= 10 # 자릿수 증가시키기
    
    print(list(queue))
```

# 계수 정렬(Counting Sort)
> 비교하지 않는 정렬 알고리즘으로 **데이터의 개수**를 세서 정렬한다

## 특징
- 데이터의 범위가 0 또는 양의 정수여야 한다.
- 데이터의 개수 $n$, 데이터의 최댓값 $k$에 대해 $O(n+k)$의 시간복잡도를 가진다.
- 데이터가 시간 복잡도에 영향을 끼치며, 데이터 범위만큼의 메모리 공간을 필요로 한다.

## 정렬 과정
1. 데이터 범위를 인덱스로 갖는 빈 배열을 생성한다.
2. 정렬하려는 배열을 순회하면서 데이터에 해당하는 인덱스의 값을 1씩 증가시킨다.
<div align="center">
    <img src="https://github.com/seoyeon22/seoyeon22.github.io/assets/70433999/83776b60-fd31-4e86-83e1-9c4e451cf75e" alt="counting sort">
</div>

## 구현
```python
def count_sort(input_array):

    M = max(input_array) # 입력 배열의 최댓값
 
    count_array = [0] * (M + 1) # 0으로 초기화
 
    # count_array[num]를 input_array의 num 개수로 설정
    for num in input_array:
        count_array[num] += 1
 
    # 누적합 계산
    for i in range(1, M + 1):
        count_array[i] += count_array[i - 1]
 
    output_array = [0] * len(input_array) # count_array 기반으로 output_array 생성
 
    for i in range(len(input_array) - 1, -1, -1):
        output_array[count_array[input_array[i]] - 1] = input_array[i]
        count_array[input_array[i]] -= 1
 
    return output_array
```

# 정리
|비교 기반|알고리즘|
|:--:|:--:|
|O|버블 정렬, 선택 정렬, 삽입 정렬, 퀵 정렬, 힙 정렬|
|X|기수 정렬, 계수 정렬|

|시간 복잡도|알고리즘|
|:--:|:--:|
|$O(nlog_n)$|합병 정렬, 퀵 정렬, 힙 정렬|
|$O(n^2)$|버블 정렬, 선택 정렬, 삽입 정렬|


# Reference
- 이수진, 기술 면접 대비 CS 전공 핵심요약집 (IT 대기업 합격자의 비밀노트), 길벗
- https://www.geeksforgeeks.org/