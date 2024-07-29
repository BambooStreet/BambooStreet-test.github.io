---
title: BFS(Breadth-First Search), DFS(Depth-First Search)
author: BambooStreet
date: 2024-07-23 15:18:00 +0800
categories: [algorithm]
tags: [algorithm, 알고리즘, BFS, DFS]
---

## Breadth-First Search (BFS)란?
시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어진 정점을 나중에 방문하는 탐색 방법이다.

동작 방식은 다음과 같다.
* 큐(Queue)를 사용해 구현한다.
* 시작 정점을 큐에 넣고 방문 표시를 한다.
* 큐에서 정점을 꺼내 그 정점과 인접한 모든 미방문 정점을 큐에 넣고 방문 표시를 한다.
* 큐가 빌 때까지 이 과정을 반복한다.

수도 코드 예시
![BFS_pseudocode](assets\img\posts\20240723\BFS_pseudocode.png)
<br>
<br>

![BFS_example](assets\img\posts\20240723\BFS_example.png)
<br>


