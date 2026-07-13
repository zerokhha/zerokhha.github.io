---
layout: inner
title: "금융권 통합 DW 구축"
date: 2026-07-06 03:00:00 +0900
permalink: /work/one-dw-detail/
period: "2023.03 - 2024.06"
excerpt: "이원화 DW를 통합형 ONE DW로 재구축하고 Kafka 기반 실시간 연계와 모니터링 체계를 함께 구축했습니다."
---

<!-- callout-start -->
<div class="work-card">
  <div class="work-card__meta">
    <span class="period">2023.03 - 2024.06</span>
    <span class="tech">SQL · Shell · Python · Kafka · Grafana</span>
  </div>
  <h2 class="work-card__title">금융권 통합 DW 구축</h2>
  <p class="work-card__lede">도메인 기반 DW 재구축과 Kafka 스트리밍 연계를 통해 실시간 분석과 배치 성능을 개선했습니다.</p>
  <div class="work-card__metrics">
    <div class="metric"><div class="metric__num">70%</div><div class="metric__label">배치 시간 단축</div></div>
    <div class="metric"><div class="metric__num">30%</div><div class="metric__label">실시간 활용도 증가</div></div>
  </div>
</div>
<!-- callout-end -->

---

### 배경 및 과제 (Background)

<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **DW 구조적 한계 및 도메인 미분리:** 기존 DW가 단일 스키마 체계로 운영되어 업무 도메인 구분이 미흡했고, 이로 인한 데이터 중복과 충돌 문제가 발생했습니다.
* **성능 및 확장성 부족:** 실시간 분석 수요는 증가하는 반면, 기존 시스템의 대용량 배치 처리 성능 한계로 유연성과 확장성이 떨어졌습니다.
* **핵심 요구사항:** 업무별 도메인 중심의 마트 재구축, Kafka 기반 실시간 데이터 연계, 배치 성능 개선, 모니터링 체계 고도화까지 동시에 추진해야 했습니다.
</section>

---

### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. 도메인 기반 DW 구조 설계
* DW를 공통, 마케팅, 카드, 리스크, 자동세탁방지 등 주요 업무 도메인별로 분리해 각 특성에 맞는 독립적 스키마 구조를 자립시켰습니다.
</div>

<div class="post-step" markdown="1">
#### 2. Kafka 파이프라인 구현
* Kafka에서 실시간 수집된 JSON 로그 데이터를 DBMS 내장 함수로 파싱해 임시 테이블에 적재하고, 필드 누락 및 중복 제어를 위한 전처리 로직을 자동화했습니다.

<p align="center">
  <img src="{{ site.baseurl }}/img/posts/Kafka.png" alt="DW & Kafka Pipeline Architecture" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▲ Kafka 스트리밍 연계 및 차세대 DW 구조</small></em>
</p>
</div>

<div class="post-step" markdown="1">
#### 3. 대용량 쿼리 튜닝
* 고비용 SQL 튜닝을 통해 로직을 리팩토링하고(`NOT IN`, `NOT EXISTS` 전환, 윈도우 함수 `MIN`/`MAX` 구조 변경), 자주 활용되는 컬럼 중심의 테이블 재구조화를 진행했습니다.
* 대량 `UPDATE` 성능 병목을 해소하기 위해 임시 테이블 기반 일괄 적재(Bulk Load) 방식을 도입해 대용량 처리 성능을 개선했습니다.
</div>

<div class="post-step" markdown="1">
#### 4. Grafana 모니터링 체계 구축
<p align="center">
  <img src="{{ site.baseurl }}/img/posts/Grafana.png" alt="Vertica Monitoring" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▲ Grafana 대시보드 연계 구조</small></em>
</p>

* Python과 경량 스크립트를 활용해 시스템 자원(CPU, Memory, Disk) 및 쿼리 실행 지표를 실시간 수집하고, **Grafana 대시보드**로 이상 징후를 조기에 탐지할 수 있게 구성했습니다.
</div>
</section>

---

### 성과 (Result)

<section class="post-section post-section--result" markdown="1">
### 성과 (Result)
* **데이터 거버넌스 명확화:** 도메인 중심 DW 재구축을 통해 데이터 표준과 관리 기준을 수립하고 고질적인 데이터 중복·충돌 문제를 해소했습니다.
* **실시간 데이터 활용도 30% 증가:** Kafka 기반 스트리밍 연계 도입으로 데이터 활용 시점 지연을 줄여 비즈니스 분석 인사이트를 더 빠르게 제공했습니다.
* **배치 처리 시간 70% 단축:** 효율적인 SQL 튜닝과 데이터 재구조화를 통해 대용량 데이터 적재 안정성과 운영 효율성을 크게 높였습니다.
* **장애 대응 속도 60% 단축:** 모니터링 자동화와 통합 대시보드 구축으로 인프라 및 쿼리 이상 징후를 실시간으로 감지해 선제적으로 대응할 수 있게 했습니다.
</section>