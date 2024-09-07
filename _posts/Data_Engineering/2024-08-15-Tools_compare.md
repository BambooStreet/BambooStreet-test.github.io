---
title: Airflow vs Spark vs Glue
author: BambooStreet
date: 2024-08-15 00:00:00 +0800
categories: [study]
tags: []
image: assets/img/avatar/testcat.jpg
---

### Airflow vs Spark vs Glue: Extract 및 Transform 지원 비교

1. Apache Airflow
   * 주요 목적: 워크플로우 오케스트레이션
   * Extract 지원:
     - 다양한 Operator를 통해 여러 소스에서 데이터 추출
     - 직접 추출보다는 추출 작업 스케줄링 및 관리에 강점
   * Transform 지원:
     - PythonOperator를 통한 간단한 변환
     - 복잡한 변환은 주로 외부 도구(예: Spark) 호출을 통해 수행
   * 특징:
     - ETL 작업 자체보다 전체 프로세스의 조율에 중점

2. Apache Spark
   * 주요 목적: 대규모 데이터 처리
   * Extract 지원:
     - 다양한 소스(파일, 데이터베이스, 스트리밍 등)에서 직접 데이터 추출
     - 배치 및 스트리밍 방식 모두 지원
   * Transform 지원:
     - 강력하고 유연한 데이터 변환 기능 제공
     - SQL, DataFrame API, RDD 등 다양한 방식으로 변환 가능
   * 특징:
     - 대용량 데이터의 복잡한 처리에 최적화

3. AWS Glue
   * 주요 목적: 완전 관리형 ETL 서비스
   * Extract 지원:
     - 내장 커넥터를 통해 다양한 AWS 서비스 및 데이터베이스에서 추출
     - Glue 크롤러를 통한 자동 스키마 추론 및 카탈로그 생성
   * Transform 지원:
     - Spark 기반의 ETL 작업 지원
     - 시각적 ETL 편집기(Glue Studio) 제공
   * 특징:
     - AWS 생태계와의 긴밀한 통합
     - 서버리스 아키텍처로 인프라 관리 불필요

### 비교 요약

| 특성           | Airflow | Spark | Glue |
|----------------|---------|-------|------|
| Extract 직접 수행 | 제한적   | 강력함 | 강력함 |
| Transform 능력  | 제한적   | 매우 강력함 | 강력함 |
| 워크플로우 관리   | 매우 강력함 | 제한적 | 중간 |
| 확장성         | 높음     | 매우 높음 | 자동 |
| 학습 난이도       | 중간     | 어려움 | 쉬움 |
| 클라우드 통합    | 다양     | 다양  | AWS 중심 |