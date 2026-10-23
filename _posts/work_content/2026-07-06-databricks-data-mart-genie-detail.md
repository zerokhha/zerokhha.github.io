---
layout: inner
title: "Databricks 기반 데이터 마트·AI 질의 PoC"
date: 2026-07-06 18:00:00 +0900
permalink: /work/databricks-data-mart-genie/
period: "2026.08 - 2026.09"
excerpt: "현업 자연어 질의 기반 AI 데이터 조회 환경 검증을 위해 Databricks Lakehouse 파이프라인과 데이터 마트를 구축한 PoC"
---

<!-- callout-start -->
<div class="work-card">
  <div class="work-card__meta">
    <span class="period">2026.08 - 2026.09</span>
    <span class="tech">Databricks · SQL · Genie</span>
  </div>
  <h2 class="work-card__title">Databricks 기반 데이터 마트·AI 질의 PoC</h2>
  <p class="work-card__lede">Lakehouse 파이프라인과 데이터 마트를 구성하고 Genie Agent 기반 자연어 질의 환경을 학습·검증했습니다.</p>
  <div class="work-card__metrics">
    <div class="metric"><div class="metric__num">8개</div><div class="metric__label">데이터 마트 테이블</div></div>
    <div class="metric"><div class="metric__num">40~50% → 90%</div><div class="metric__label">Genie 정답률 개선</div></div>
  </div>
</div>
<!-- callout-end -->

---
<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **AI 데이터 조회 환경 검증:** 현업 사용자가 자연어로 데이터를 질의하고 결과를 확인할 수 있는 환경의 실효성을 검증하기 위한 PoC 수행
* **데이터 마트 구축 필요:** 원천 데이터를 분석 목적에 맞게 정제하고, Genie가 안정적으로 질의할 수 있는 구조로 구성
* **질의 정확도 개선:** 초기 Genie 정답률 40~50% 수준에서 주요 업무 질문에 대한 정답률을 높이는 학습·검증 과정 필요
</section>

---
### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. Lakehouse 파이프라인 구성
* Databricks Volume에 원천 데이터를 업로드하고, **Medallion 아키텍처(Bronze → Silver → Gold)** 기반의 파이프라인을 구성
* Bronze, Silver, Gold 계층별 정제 및 변환 과정을 거쳐 분석용 데이터 마트를 구축하고, 8개 테이블 및 100만 건 이내의 데이터를 처리
</div>

<div class="post-step" markdown="1">
#### 2. 데이터 마트 구축 및 Genie 질의 학습·검증
* 데이터 마트의 테이블 구조와 컬럼 의미를 정리해 Genie 질의에 필요한 데이터 컨텍스트를 구성
* Genie Agent 기반으로 주요 질문 20개에 대한 정답 SQL을 작성하고 학습시켜 자연어 질의 결과를 개선
* 벤치마크 기능으로 질의 결과를 검증하고, 오답 쿼리를 수정하는 과정을 반복해 정답률을 개선
</div>
</section>

---
<section class="post-section post-section--result" markdown="1">
### 성과 (Result)
* **Genie 정답률 40~50% → 90% 개선:** 주요 질문 20개를 기준으로 정답 SQL 학습 및 벤치마크 검증을 수행해 질의 정확도 향상
* **AI 데이터 조회 환경 검증:** 현업 자연어 질의 기반 데이터 조회 가능성을 확인하고, Databricks Lakehouse 기반의 데이터 마트 운영 구조를 검증
* **고객 도입 검토 진행:** PoC 결과를 바탕으로 고객 환경 도입 가능성 및 후속 적용 범위를 검토 중
</section>