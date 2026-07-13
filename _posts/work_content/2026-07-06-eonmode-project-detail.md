---
layout: inner
title: "금융권 비즈니스 업무 이관"
date: 2026-07-06 00:00:00 +0900
permalink: /work/eonmode-project/
period: "2021.06 - 2021.10"
excerpt: "Hadoop 내 대규모 분석 데이터를 Vertica로 이관하기 위한 병렬 적재 아키텍처 설계와 자동화 프로세스를 구축했습니다."
---

<!-- callout-start -->
<div class="work-card">
  <div class="work-card__meta">
    <span class="period">2021.06 - 2021.10</span>
    <span class="tech">Hadoop(HDFS) · Vertica · Shell Script</span>
  </div>
  <h2 class="work-card__title">금융권 비즈니스 업무 이관</h2>
  <p class="work-card__lede">Hadoop에서 Vertica로 대규모 데이터 이관을 병렬 적재 아키텍처와 자동화로 완성했습니다.</p>
  <div class="work-card__metrics">
    <div class="metric"><div class="metric__num">7TB</div><div class="metric__label">6시간 적재</div></div>
    <div class="metric"><div class="metric__num">Auto</div><div class="metric__label">프로세스 자동화</div></div>
  </div>
</div>
<!-- callout-end -->

---

### 배경 및 과제 (Background)

<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **대규모 데이터 이관 필요:** Hadoop 내에 저장된 대규모 분석 데이터를 일정 기한 내에 Vertica로 이관해야 하는 성능 및 운영 효율성 과제가 있었습니다.
</section>

---

### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<p align="center">
  <img src="{{ site.baseurl }}/img/posts/Hadoop_to_Vertica.png" alt="Hadoop to Vertica Architecture" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▲ Hadoop → Vertica 데이터 이관 아키텍처</small></em>
</p>

<div class="post-step" markdown="1">
#### 1. 파티션 단위의 병렬 적재 아키텍처 설계
* 데이터 분산 처리를 극대화하기 위해 파티션 단위 병렬 적재 방식을 도입해 I/O 병목 현상을 해소했습니다.
* 이를 통해 **7TB 규모의 대용량 데이터**를 전처리 과정을 포함해 **6시간 이내에 빠르게 적재**했습니다.
</div>

<div class="post-step" markdown="1">
#### 2. 이관 프로세스 자동화 구축
* 반복적인 이관 작업의 리소스를 줄이기 위해 DDL 자동 생성 및 데이터 적재 프로세스 전반을 **Shell 스크립트로 자동화**했습니다.
</div>
</section>

---

### 성과 (Result)

<section class="post-section post-section--result" markdown="1">
### 성과 (Result)
* **대규모 이관을 안정적으로 완료:** 병렬 적재와 자동화 체계를 통해 짧은 기간 안에 대량 데이터 이관을 마무리했습니다.
* **운영 효율성 향상:** 반복적이고 위험한 이관 절차를 자동화해 인적 리스크와 처리 시간을 크게 줄였습니다.
</section>
