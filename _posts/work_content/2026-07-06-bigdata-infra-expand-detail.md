---
layout: inner
title: "공공기관 빅데이터 분석 인프라 확대"
date: 2026-07-06 02:00:00 +0900
permalink: /work/bigdata-infra-expand-detail/
period: "2022.03 - 2022.06"
excerpt: "기존 분석 시스템의 성능 및 확장성 한계를 극복하기 위해 신규 DBMS 도입과 자동화된 마이그레이션 체계를 구축했습니다."
---

<!-- callout-start -->
<div class="work-card">
	<div class="work-card__meta">
		<span class="period">2022.03 - 2022.06</span>
		<span class="tech">SQL · Shell Script · RBAC</span>
	</div>
	<h2 class="work-card__title">공공기관 빅데이터 분석 인프라 확대</h2>
	<p class="work-card__lede">신규 DBMS 도입과 자동화된 마이그레이션 체계를 통해 분석 인프라의 확장성과 보안성을 동시에 강화했습니다.</p>
	<div class="work-card__metrics">
		<div class="metric"><div class="metric__num">90%</div><div class="metric__label">마이그레이션 자동화</div></div>
		<div class="metric"><div class="metric__num">50%</div><div class="metric__label">구축 기간 단축</div></div>
	</div>
</div>
<!-- callout-end -->

---

### 배경 및 과제 (Background)

<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **신규 DBMS 도입 및 마이그레이션 필요:** 조직 내 빅데이터 분석 수요 증가로 인해 기존 시스템의 성능과 확장성 한계를 극복하고자 신규 DBMS 도입을 추진했습니다.
* **수작업 기반 운영의 리스크:** 기존 환경의 수작업 기반 객체 생성 방식은 반복 오류와 일관성 저하를 유발했습니다.
* **보안 및 권한 관리 미흡:** 사용자 접근 권한 관리 체계가 부족해 데이터 오남용 및 보안 리스크가 존재했습니다.
</section>

---

### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. 신규 DBMS 환경 구축 및 최적화
* 신규 DBMS 설치와 초기 분석 환경 구성을 통해 기본 운영 구조를 정비했습니다.
* 사용자 유형별 역할(Role)을 정의하고 **RBAC 기반 권한 관리 체계**를 수립해 권한을 명확히 분리했습니다.
* 스키마 단위 사용자 격리를 통해 업무별 보안 경계를 확보하고 데이터 유출 위험을 낮췄습니다.
</div>

<div class="post-step" markdown="1">
#### 2. 수작업 마이그레이션 자동화 체계 설계
* 기존 DDL 생성·배포 프로세스를 자동화 가능한 로직으로 재설계했습니다.
* 신규 DBMS의 데이터 타입 특성에 맞춰 `NCHAR`/`NVARCHAR` 저장 공간 요구량을 반영한 **자동 사이징 로직(1.5배)**을 구현했습니다.
* Shell Script 기반으로 DDL 변환 → 테이블 생성 → 권한 부여까지 일괄 처리하는 자동화 파이프라인을 구축했습니다.
</div>
</section>

---

### 성과 (Result)

<section class="post-section post-section--result" markdown="1">
### 성과 (Result)
* **마이그레이션 작업의 90% 이상 자동화:** DDL 자동 변환과 배포 체계를 도입해 반복적이고 안정적인 스키마 생성이 가능해졌습니다.
* **보안성 및 거버넌스 강화:** 역할 기반 권한 분리로 데이터 유출 위험을 최소화하고 감사 대응 체계를 마련했습니다.
* **구축 기간 약 50% 단축:** DDL 마이그레이션 작업 속도를 높여 분석 환경 초기 구축 기간을 크게 단축했습니다.
</section>