---
layout: inner
title: "Databricks 기반 데이터 마트·AI 질의 PoC"
date: 2026-07-06 18:00:00 +0900
permalink: /work/databricks-data-mart-genie/
period: "2026.08 - 2026.09"
excerpt: "현업 매출 대시보드 요구를 Genie 자연어 질의로 조회할 수 있도록 Lakehouse 파이프라인과 데이터 마트를 구축하고 정답률을 검증한 PoC"
---

<!-- callout-start -->
<div class="work-card">
  <div class="work-card__meta">
    <span class="period">2026.08 - 2026.09</span>
    <span class="tech">Databricks · SQL · Genie</span>
  </div>
  <h2 class="work-card__title">Databricks 기반 데이터 마트·AI 질의 PoC</h2>
  <p class="work-card__lede">연도별 Silver 테이블과 매출 메트릭 뷰를 구성하고, Genie 자연어 질의의 정답률을 개선·검증했습니다.</p>
  <div class="work-card__metrics">
    <div class="metric"><div class="metric__num">8개</div><div class="metric__label">데이터 마트 테이블</div></div>
    <div class="metric"><div class="metric__num">40~50% → 90%</div><div class="metric__label">Genie 정답률 개선</div></div>
  </div>
</div>
<!-- callout-end -->

---
<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **현업 매출 대시보드 질의 검증:** 현업의 매출 관련 질문을 Genie 자연어 질의로 처리할 수 있는 AI 데이터 조회 환경 PoC 수행
* **분석용 데이터 구조 마련:** retail 팩트와 코드성 디멘전을 연도별 Silver 테이블로 정리하고, Genie가 활용할 매출 지표와 데이터 맥락 정의
* **질의 정확도 개선:** 초기 Genie 정답률 40~50%에서 주요 질문에 대한 정답률을 90% 수준으로 높이기 위한 반복 검증 필요
</section>

---
### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. Lakehouse 파이프라인 구성
* retail 팩트 테이블과 코드성 디멘전 테이블을 연도별 Silver 테이블로 구성하고, Databricks Lakehouse 파이프라인으로 분석용 데이터를 정제
* 총 8개 테이블, 100만 건 이내의 데이터 범위에서 팩트와 디멘전을 조인한 매출 메트릭 뷰를 구축
* 메트릭 뷰에 매출 지표의 계산식과 집계 기준을 정의해 질의별 지표 해석이 일관되도록 구성
</div>

<div class="post-step" markdown="1">
#### 2. 데이터 마트 구축 및 Genie 질의 학습·검증
* 테이블·컬럼 설명을 정리하고, Instructions에 최소 필요 컬럼과 조회 날짜 조건을 명시해 Genie가 불필요한 데이터 조회를 줄이도록 구성
* 현업 주요 질문 20개에 대한 정답 쿼리를 Example SQL로 등록해 자연어 질문과 SQL 간 맥락을 보강
* 벤치마크로 오답 유형을 반복 검증하고 메트릭 정의, Instructions, Example SQL을 보완해 정답률을 개선
</div>
</section>

---
<section class="post-section post-section--result" markdown="1">
### 성과 (Result)
* **Genie 정답률 40~50% → 90% 개선:** 매출 지표 정의, Instructions, 주요 질문 20개의 Example SQL을 정비하고 벤치마크 기반으로 반복 검증
* **현업 매출 데이터 조회 환경 검증:** 연도별 Silver 테이블과 메트릭 뷰를 기반으로 자연어 질의 결과를 확인하고, 8개 테이블·100만 건 이내 규모의 데이터 마트 구축
* **고객 도입 검토 진행 중:** PoC 결과를 바탕으로 고객 환경 도입 가능성과 후속 적용 범위를 검토
</section>