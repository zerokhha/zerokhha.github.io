---
layout: inner
title: "[성능 비교] Spark(AWS) vs Vertica(On-Premise) 6천만 건 데이터 튜닝기"
date: 2026-08-19 09:00:00 +0900
permalink: /work/spark-vertica-performance/
period: "2026.08"
excerpt: "NYC 택시 6,099만 건을 기준으로 Spark와 Vertica의 집계 및 조인 성능을 비교하고, 셔플과 네트워크 이동을 줄이는 방식으로 튜닝한 기록입니다."
---

<!-- callout-start -->
<div class="work-card">
  <div class="work-card__meta">
    <span class="period">2026.08</span>
    <span class="tech">Spark · AWS EC2 · Vertica · SQL</span>
  </div>
  <h2 class="work-card__title">Spark(AWS) vs Vertica(On-Premise) 성능 비교</h2>
  <p class="work-card__lede">60.9M rows를 대상으로 집계와 조인 성능을 비교하고, 셔플과 네트워크 이동을 줄이는 방향으로 튜닝했습니다.</p>
  <div class="work-card__metrics">
    <div class="metric"><div class="metric__num">60.9M</div><div class="metric__label">분석 데이터 규모</div></div>
    <div class="metric"><div class="metric__num">7.7x</div><div class="metric__label">Vertica 조인 단축</div></div>
  </div>
</div>
<!-- callout-end -->

---

### 배경 및 과제 (Background)

<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **대용량 데이터 처리 비교 필요:** NYC Yellow Taxi 약 6,099만 건을 기준으로 AWS 기반 Apache Spark와 온프레미스 Vertica의 집계 및 조인 성능을 직접 비교했습니다.
* **네트워크 병목 확인:** 분산 환경에서는 연산 자체보다 노드 간 데이터 이동, 즉 Shuffle과 네트워크 전송이 성능을 크게 좌우했습니다.
* **핵심 목표:** 같은 데이터셋에서 기본 실행과 튜닝 후 성능을 비교해, 어떤 최적화가 실제 체감 성능에 가장 큰 영향을 주는지 검증하는 것이 목표였습니다.
</section>

---

### 테스트 환경 (Environment)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. Spark (AWS Cloud)
* AWS EC2 기반 소형 클러스터에서 `c7i-flex.large`, `m7i-flex.large` 등 2 vCPU 인스턴스를 활용했습니다.
* 클라우드 환경의 유연한 확장성과 Spark 엔진의 기본 최적화 동작을 함께 확인했습니다.
</div>

<div class="post-step" markdown="1">
#### 2. Vertica (On-Premise)
* 온프레미스 서버 환경에서 동일한 워크로드를 수행해 저장 구조와 쿼리 설계가 성능에 미치는 영향을 비교했습니다.
</div>
</section>

---

### 성능 비교 결과 (Benchmark)

<section class="post-section post-section--result" markdown="1">
### 성능 비교 결과 (Benchmark)

| 테스트 시나리오 | Spark (기본) | Spark (최적화) | Vertica (기본) | Vertica (최적화) | 핵심 튜닝 내용 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **GroupBy (단순 집계)** | 3.06초 | **0.82초** | 0.86초 | **0.74초** | 파티션 수 조절, 필요한 컬럼만 스캔 |
| **Join (택시 6천만 건 x 메타 데이터)** | 5.52초 | **1.62초** | 5.10초 | **0.66초** | 브로드캐스트 조인, 사전 집계로 셔플 제거 |

* Spark는 엔진 레벨 최적화 덕분에 복잡한 쿼리 수정 없이도 성능 향상이 가능했습니다.
* Vertica는 물리적 저장 구조와 쿼리 재작성의 효과가 매우 크게 나타나, 조인과 집계 단계에서 압도적인 속도 차이를 만들었습니다.
</section>

---

### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. Spark 최적화: 브로드캐스트 조인 적용
* 대규모 트립 데이터와 소형 Zone 메타 데이터를 조인할 때 발생하는 Shuffle 비용이 병목이었습니다.
* 작은 메타 데이터를 각 워커 메모리에 복제하는 Broadcast Join을 사용해 네트워크 이동을 줄이고, 조인 시간을 크게 단축했습니다.
</div>

<div class="post-step" markdown="1">
#### 2. Vertica 최적화: 사전 집계 후 조인
* 조인 후 Borough 기준으로 바로 집계하면 6천만 건이 네트워크를 타고 다시 이동해 비용이 컸습니다.
* 먼저 `PULocationID` 기준으로 노드 내부에서 집계해 데이터를 265건 수준으로 줄인 뒤 조인하여, 최종 실행 시간을 0.66초까지 낮췄습니다.
</div>

<div class="post-step" markdown="1">
#### 3. 네트워크 이동 최소화 관점 정리
* 집계와 조인 성능 차이는 단순히 CPU 계산량보다, 분산 노드 간 이동하는 데이터 크기를 얼마나 줄이느냐에 의해 결정된다는 점을 확인했습니다.
</div>
</section>

---

### SQL 예시

<section class="post-section post-section--background" markdown="1">
### SQL 예시

```sql
-- 튜닝 전: 6천만 건을 조인하고 통째로 집계
SELECT z.Borough, AVG(t.fare_amount)
FROM yellow_taxi_trips t
JOIN taxi_zone_lookup z ON t.PULocationID = z.LocationID
GROUP BY z.Borough;

-- 튜닝 후: 먼저 집계해서 데이터를 줄인 뒤 조인
SELECT 
    z.Borough,
    SUM(agg.sum_fare) / SUM(agg.cnt) AS avg_fare
FROM (
    SELECT PULocationID, SUM(fare_amount) AS sum_fare, COUNT(*) AS cnt
    FROM yellow_taxi_trips
    GROUP BY PULocationID
) agg
JOIN taxi_zone_lookup z ON agg.PULocationID = z.LocationID
GROUP BY z.Borough;
```
</section>

---

### 결론 및 배운 점

<section class="post-section post-section--result" markdown="1">
### 결론 및 배운 점
* **Spark의 강점:** 복잡하게 쿼리를 바꾸지 않아도 Catalyst 기반 최적화가 잘 동작해 다루기 편했습니다.
* **Vertica의 강점:** 저장 구조와 쿼리를 조금만 맞춰도 네이티브 엔진 성능이 크게 드러나, 0.6초대의 높은 실행 속도를 확인했습니다.
* **분산 처리의 핵심:** 연산 자체보다 노드 간 데이터 이동을 줄이는 것이 성능 최적화의 핵심이라는 점을 실험으로 검증했습니다.
</section>