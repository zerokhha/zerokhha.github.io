---
layout: inner
title: "금융권 정보계 DW 성능 개선"
date: 2026-07-06 04:00:00 +0900
permalink: /work/one-dw-performance-improve-detail/
period: "2025.01 - 2025.05"
excerpt: "대용량 테이블 조회 시 발생하는 디스크 I/O 병목을 해결하고, 자원 과점유 비정형 쿼리를 실시간으로 탐지해 자동 제어하는 인프라 최적화 프로세스를 구축했습니다."
---

<!-- callout-start -->
<div class="work-card">
  <div class="work-card__meta">
    <span class="period">2025.01 - 2025.05</span>
    <span class="tech">DBMS Architecture · Direct Read · Concurrency Control</span>
  </div>
  <h2 class="work-card__title">금융권 정보계 DW 성능 개선</h2>
  <p class="work-card__lede">캐시 디스크 우회와 자동 제어를 통해 디스크 I/O 병목을 해소하고 안정적인 분석 환경을 확보했습니다.</p>
  <div class="work-card__metrics">
    <div class="metric"><div class="metric__num">70%</div><div class="metric__label">배치 안정화</div></div>
    <div class="metric"><div class="metric__num">60%</div><div class="metric__label">장애 대응 속도 개선</div></div>
  </div>
</div>
<!-- callout-end -->

---

### CASE 1. 캐시 디스크 병목 해결 및 직접 조회(Direct Read) 최적화

<section class="post-section post-section--background" markdown="1">
#### 배경 및 과제 (Background)
* **캐시 디스크 I/O 100% 점유:** 대량 테이블(1TB 이상) 조회 시, 백그라운드에서 발생하는 데이터 캐시 디스크 업로드 작업으로 인해 Disk I/O가 100% 가득 차는 성능 저하 문제가 발생했습니다.
* **스토리지 부하 전이 리스크:** 단순 우회 시 스토리지 레이어 자체에 과도한 부하가 걸릴 수 있어 성능 최적화와 안정적인 자원 제어 정책이 동시에 요구되었습니다.
</section>

<p align="center">
  <img src="{{ site.baseurl }}/img/posts/diskio_kill.png" alt="Direct Read & Concurrency Limit Architecture" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▲ 캐시 업로드 병목 해소를 위한 Direct Read 전환 및 동시 쿼리 제한 정책 구조</small></em>
</p>

<section class="post-section post-section--action" markdown="1">
#### 해결 과정 (Action)
<div class="post-step" markdown="1">
##### 1. 캐시 레이어 우회를 통한 쿼리 실행 경로 최적화
* I/O 병목의 근본 원인이었던 캐시 디스크 쓰기 과정을 생략하고, 스토리지에서 데이터를 직접 조회하는 **직접 조회(Direct Read) 방식**으로 쿼리 실행 경로를 재설계했습니다.
* 이를 통해 대용량 테이블 스캔 시 백그라운드 캐시 적재 단계(`Compute` $\leftrightarrow$ `Cache` $\leftrightarrow$ `Storage`)에서 유발되던 디스크 경합 현상을 원천적으로 차단했습니다.
</div>

<div class="post-step" markdown="1">
##### 2. 사용자별 동시 쿼리 실행 수(Concurrency Limit) 제한 정책 도입
* 직접 조회 방식으로 전환된 후 스토리지 레이어에 집중될 수 있는 물리적 부하를 제어하기 위한 안전장치를 마련했습니다.
* 사용자 및 업무 단위별로 동시 실행 가능한 쿼리 수의 임계치를 정의하는 **동시성 제어(Concurrency Limit) 정책**을 엔진 레벨에 적용해 특정 세션의 자원 독점을 방지했습니다.
</div>
</section>

<section class="post-section post-section--result" markdown="1">
#### 성과 (Result)
* **디스크 병목 현상 근본적 해결:** 대량 조회 쿼리가 수행되더라도 디스크 I/O 100% 임계치 도달 현상이 발생하지 않도록 안정화했습니다.
* **시스템 가용성 보장:** 스토리지 직접 조회 최적화와 동시성 제한 정책의 연계를 통해 대용량 분석 환경에서도 안정적인 가용성을 확보했습니다.
</section>

---

### CASE 2. 자원 과점유 비정형 쿼리 자동 제어(Auto-kill) 시스템 구축

<section class="post-section post-section--background" markdown="1">
#### 배경 및 과제 (Background)
* **비정형(Ad-hoc) 쿼리의 자원 독점:** 사용자들이 임의로 실행하는 고부하 비정형 쿼리가 과도하게 자원을 점유하면서 전체 클러스터의 성능 저하와 가용성 마비를 유발했습니다.
* **물리적 격리 조치의 한계:** 1차적으로 리소스 풀을 조정하고 CPU 격리를 시행했으나, 하드웨어 물리적 자원 한계로 근본적인 악성 부하 차단에는 한계가 있었습니다.
* **구조적 차단 프로세스 필요:** 시스템 마비를 일으키는 명확한 고부하 유발 쿼리 패턴을 자동으로 조기 식별하고 강제 제어하는 시스템이 필요했습니다.
</section>

<p align="center">
  <img src="{{ site.baseurl }}/img/posts/session_kill.png" alt="Ad-hoc Query Auto-kill Architecture" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▲ 3분 주기 실행 계획 모니터링 기반의 악성 세션 자율 제어 파이프라인</small></em>
</p>

<section class="post-section post-section--action" markdown="1">
#### 해결 과정 (Action)
<div class="post-step" markdown="1">
##### 1. 실행 계획 기반 악성 쿼리(CROSS JOIN 등) 실시간 탐지 로직 구현
* 현업 부서와 긴밀히 협의해 클러스터 정체의 주범이 되는 고부하 유발 악성 쿼리 패턴 2종을 명확히 정의했습니다.
* 3분 주기로 배치 프로세스를 구동해 `dc_explain_plans` 시스템 뷰에서 카테시안 곱을 유발하는 **`CROSS JOIN` 키워드**와 리소스 과점유 패턴을 자동 식별하는 적재 쿼리 파이프라인을 구축했습니다.
</div>

<div class="post-step" markdown="1">
##### 2. 셸 및 프로시저 기반 세션 자동 종료(Auto-kill) 및 알림 루프 구축
* 식별된 악성 세션의 이력 중 미조치 건을 대상으로 `LOOP`를 돌며 해당 세션을 즉시 강제 종료하는 **`close_session(ss_id)` 프로시저 및 스크립트**를 연동했습니다.
* 정상적인 일반 쿼리는 정상 종료 단계를 밟도록 필터링하고, 자원 과점유 세션에 대해서만 정밀하게 자동 종료 조치를 수행해 운영 투명성을 확보했습니다.
</div>
</section>

<section class="post-section post-section--result" markdown="1">
#### 성과 (Result)
* **이상 쿼리로 인한 자원 지연 해소:** 고부하 비정형 쿼리가 클러스터 전체를 마비시키기 전에 3분 단위 샌드박스 안에서 자동 진압되도록 체계화했습니다.
* **안정적인 데이터 분석 환경 확보:** 시스템 전체 자원의 예측 가능성을 확보해 현업 사용자와 실시간 분석 인프라에 공평한 컴퓨팅 파워 분배 환경을 완성했습니다.
</section>