---
title: 완전탐색
author: BambooStreet
date: 2024-07-29 14:00:00 +0800
categories: [algorithm]
tags: [algorithm, 알고리즘, 완전탐색]
---

## 완전탐색이란?
정의 : 가능한 모든 경우의 수를 빠짐없이 조사해 문제의 해를 찾는 알고리즘 기법
특징 : 
* 모든 가능성을 고려하므로 항상 최적의 해를 찾을 수 있다.
* 문제의 크기가 작을 때 효과적이다.
* 구현이 비교적 간단하다.
* 시간 복잡도가 높아 큰 입력에 대해서는 비효율적이다.

## 주요 방법 :
1. 부르트 포스(Brute Force)
2. 백트래킹(Backtracking)
3. 순열(Permutation), 조합(Combination) 생성


## 예시 '부분집합의 합' 문제

문제 : 주어진 정수 배열에서 원소들의 부분집합 중 그 합이 특정 값 K가 되는 부분집합이 있는지 찾는 문제

예시 입력:
배열: [3, 34, 4, 12, 5, 2]
목표 합: K = 9


1. 부르트 포스 방식
```python
def subset_sum_brute_force(arr,k):
    n = len(arr)
    for i in range(1,1 < n):
        subset_sum = sum(arr[j] for j in range(n) if (i & (1<<j)))
        if subset_sum == k:
            return True

    return False

arr = [3,34,4,12,5,2]
K = 9
print("브루트 포스:", subset_sum_brute_force(arr, K))
```

반복문을 통해 모든 가능한 부분집합을 생성해 확인한다. 
시간복잡도는 O(2^n)d이다.

2. 백트래킹
```python
def subset_sum_brute_force(arr,k):
    def backtrack(index, current_sum):
        print(index, current_sum)
        if current_sum == k:
            return True #K를 찾는 경우
        if index == len(arr) or current_sum > K:
            return False #배열의 끝 혹은 값이 K를 초과한 경우
        
        #현재 원소를 선택하는 경우와 선택 안하는 경우 두 가지를 재귀적으로 탐색한다.
        return backtrack(index+1, current_sum+arr[index]) or backtrack(index+1, current_sum)
        
    return backtrack(0,0)

arr = [3,34,4,12,5,2]
K = 9
print("백트래킹:", subset_sum_brute_force(arr, K))
```

백트래킹은 가능한 해결책을 찾으려 전진하다가 아니라고 판단되면 뒤로 돌아가 다른 가능성을 시도한다.
위 예시 문제에서 return에서 or 논리연산자를 통해 경우를 나누고 옳은 경로인지 판단한다.
시간복잡도는 브루트 포스와 같이 O(2^n)이지만 평균적으로 더 빠르다. 

3. 재귀를 이용한 조합 생성 
```python
def subset_sum_recursive(arr,k):
    def generate_subsets(index, current_sum):
        if current_sum == K:
            return True
        if index == len(arr): #배열의 끝에 도달한 경우 False 반환
            return False
        return generate_subsets(index+1,current_cum+arr[index]) or generate_subsets(index+1,current_sum)
    return generate_subsets(0,0)

arr = [3,34,4,12,5,2]
K = 9
print("재귀 조합:", subset_sum_recursive(arr, K))
```

재귀 알고리즘의 특징은 모든 가능한 부분집합을 생성한다는 것이다.
백트래킹과 달리 합이 K를 초과해서 계속 탐색을 진행한다.
재귀 호출을 통해 백트래킹에 비해 더 많은 경우의 수를 탐색한다.
시간 복잡도는 O(2^n)이다.


4. 동적 계획법(DP) - 참고용 
```python
def subset_sum_dp(arr,K): 

    #dp 테이블 초기화 n+1 X k+1
    n = len(arr)
    dp = [[False for _ in range(K+1)] for _ in range(n+1)]


    for i in range(n+1):
        dp[i][0] = True

    for i in range(1,n+1): #고려 중인 배열의 원소 인덱스
        for j in range(1,K+1): #검사 중인 부분합
            if arr[i-1] <= j: #현재 원소가 현재 검사 중인 합 j보다 작거나 같은지 확인
                ap[i][j] = ap[i-1][j - arr[i-1]] or dp[i-1][j]  
            else :
                dp[i][j] = dp[i-1][j] #현재 원소가 너무 크면 이전 상태 그래도 유지

    return dp[n][K]

arr = [3, 34, 4, 12, 5, 2]
K = 9
print("동적 계획법:", subset_sum_dp(arr, K))
```

DP 테이블을 활용한 계산은 중복 계산을 피하며 효율적이다.
모든 가능한 부분합을 한 번에 계산한다.
시간 복잡도는 O(nK)로 다른 방법들보다 효율적이다.
단, 추가 메모리(테이블)가 필요하고, 구현이 다소 복잡하다는 단점이 있다.
