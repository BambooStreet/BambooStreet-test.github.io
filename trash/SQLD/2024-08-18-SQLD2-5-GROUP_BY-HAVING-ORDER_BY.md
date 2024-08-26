---
title: SQLD - GROUP BY, HAVING, ORDER BY
author: BambooStreet
date: 2024-08-18 00:00:00 +0800
categories: [study]
tags: []
image: assets/img/posts/20240804/testcat.jpg
---


## GROUP BY 절
* 각 행을 특정 조건에 따라 그룹으로 분리하여 계산하도록 하는 구문식
* GROUP BY 절에 그룹을 지정할 컬럼을 전달(여러 개 전달 가능)
* 만약 그룹 연산에서 제외할 대상이 있다면 미리 WHERE 절에서 해당 행을 제외한다.(WHERE 절이 GROUP BY 절보다 먼저 수행된다.)
* 그룹에 대한 조건은 WHERE 절에서 사용할 수 없다.
* SELECT 절에 집계 함수를 사용해 그룹 연산 결과를 표현한다.
* GROUP BY 절을 사용하면 데이터가요약되므로 요약 전 데이터와 함께 출력할 수 없다.


### 예시



## HAVING 절
* 그룹 함수 결과를 조건으로 사용할 떄 사용


### 예시



## ORDER BY 절
* 출력되는 행의 순서를 사용자가 변경하고자 할 때 사용
* 기본값은 오름차순이다.
* 유일하게 SELECT 절에 정의한 컬럼 별칭 사용 가능


### 복합 정렬
* 먼저 정렬한 값이 동일한 경우 2차 정렬 가능

### 예시
