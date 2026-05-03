---
layout: post
title: "Vertica Architecture: 왜 대용량 데이터 분석에 강할까?"
date: 2026-05-03 10:00:00 +0900
category: [kb]
subcategory: "Vertica"
---

Vertica는 일반적인 행 기반(Row-oriented) 데이터베이스와 달리 **열 기반(Columnar)** 아키텍처를 가진 MPP(Massive Parallel Processing) 데이터베이스입니다. 

왜 Vertica가 수십억 건의 데이터를 눈 깜짝할 새 처리하는지, 그 핵심 아키텍처를 정리합니다.

---

## 1. Columnar Storage (열 지향 저장 방식)
가장 큰 특징은 데이터를 '행'이 아닌 '열' 단위로 저장한다는 점입니다.

*   **효율적 I/O**: 필요한 컬럼만 읽기 때문에 디스크 I/O가 획기적으로 줄어듭니다.
*   **높은 압축률**: 같은 열에는 비슷한 데이터 타입이 모여 있어 압축 효율이 극대화됩니다.


## 2. Projection: Vertica의 핵심
Vertica에는 'Index'가 없습니다. 대신 **Projection**이 그 역할을 수행합니다.

*   데이터를 미리 **정렬(Sort)**하고 **복제(Replication)** 또는 **분산(Segmentation)**하여 저장하는 물리적인 테이블 구조입니다.
*   쿼리에 최적화된 Projection이 있다면 인덱스보다 훨씬 빠른 성능을 냅니다.

## 3. K-Safety (가용성)
하나의 노드가 죽어도 서비스가 멈추지 않도록 데이터를 복제하는 방식입니다.
*   **K=1**: 노드 하나가 죽어도 데이터 유실 없음 (최소 3개 노드 필요)

---

### 마치며
Vertica는 데이터를 단순히 쌓아두는 곳이 아니라, 아키텍처 설계를 어떻게 하느냐에 따라 성능이 천차만별인 솔루션입니다. 다음 포스트에서는 데이터를 어떻게 나누어 저장하는지 **Partitioning & Segmentation**에 대해 알아보겠습니다.