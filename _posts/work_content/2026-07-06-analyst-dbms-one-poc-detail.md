---
layout: inner
title: "금융권 분석계 DW 구축 PoC"
date: 2026-07-06 03:00:00 +0900
permalink: /work/analyst-dbms-one-poc-detail/
period: "2022.07 - 2022.07"
excerpt: "기존 분석 시스템의 성능 및 확장성 한계를 극복하기 위한 신규 DBMS 도입 구축"
---

<!-- callout-start -->
<div class="work-card">
	<div class="work-card__meta">
		<span class="period">2022.07</span>
		<span class="tech">SQL · Shell · DBMS ONE · Query Optimization</span>
	</div>
	<h2 class="work-card__title">금융권 분석계 DW 구축 PoC</h2>
	<p class="work-card__lede">기존 쿼리·아키텍처 리팩토링과 병렬화로 배치 시간을 50% 이상 단축하고, DBMS ONE 도입을 성공적으로 검증했습니다.</p>
	<div class="work-card__metrics">
		<div class="metric"><div class="metric__num">50%</div><div class="metric__label">배치 시간 단축</div></div>
		<div class="metric"><div class="metric__num">PoC</div><div class="metric__label">도입 검증 · 사업 수주</div></div>
	</div>
</div>
<!-- callout-end -->

---

### 배경 및 과제 (Background)

<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **성능 및 확장성 한계 직면:** 기존 분석 시스템의 특정 RDBMS 종속 구조로 인해 대량 데이터 처리 시 쿼리 성능 저하 및 확장성 문제 발생
* **신규 DBMS 검토를 위한 PoC 필요:** 차세대 분석 플랫폼으로 'DBMS ONE' 도입을 검토하기 위해, 기존 분석 쿼리를 신규 환경에 맞춰 최적화하고 검증해야 하는 과제 수반
* **병렬 처리 및 최적화 요구:** 대량의 배치 연산 성능을 극대화하기 위해 아키텍처 관점의 병렬 처리 기법 적용이 필수적이었음
</section>

---

### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. 쿼리 구조 재설계 및 ANSI 표준화
* 기존 시스템에서 성능 병목의 주원인이었던 스칼라 서브쿼리를 **LEFT OUTER JOIN 방식**으로 전면 개편하여 데이터 스캔 효율성 극대화
* 특정 벤더에 종속되어 있던 구형 조인 구문을 **표준 ANSI JOIN**으로 전환함으로써 쿼리의 이식성과 유지보수성 대폭 강화
* 신규 DBMS ONE의 고유 아키텍처와 엔진 특성에 맞춰 전체 쿼리 로직 구조 리팩토링 수행
</div>

<div class="post-step" markdown="1">
#### 2. 대량 데이터 처리 최적화 및 병렬 기법 적용
* 시스템 락(Lock)과 트랜잭션 부하를 유발하던 기존의 대량 `UPDATE` 작업을 **임시 테이블 기반 일괄 처리(Bulk Processing) 방식**으로 전환하여 수행 효율 향상
* 대용량 배치 작업에 **병렬 처리(Parallel Processing) 기법**을 적용하여 CPU 및 I/O 자원 활용을 최적화하고 실행 시간 단축 유도
* 신규 DBMS 환경에 최적화된 **사용자 정의 함수(UDF)**를 직접 설계·구현하여 빈번하게 발생하는 반복 연산의 처리 속도 개선 및 테스트 케이스 기반 정합성 검증 완료
</div>
</section>

---

### 성과 (Result)

<section class="post-section post-section--result" markdown="1">
### 성과 (Result)

* **배치 실행 시간 50% 이상 단축:** 쿼리 재설계 및 병렬 처리 최적화를 통해 대용량 분석 데이터 처리 성능 대폭 향상
* **기술적 검증 완료 및 사업 수주 기여:** 안정적이고 성공적인 PoC 수행 결과를 제시함으로써 **신규 분석 플랫폼(DBMS ONE) 도입 확정 및 실제 사업 수주**에 결정적 기여
</section>