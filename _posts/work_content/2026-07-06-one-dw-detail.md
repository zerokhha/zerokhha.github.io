---
layout: inner
title: "금융권 통합 DW 구축"
date: 2026-07-06 03:00:00 +0900
permalink: /work/one-dw-detail/
period: "2023.03 - 2024.06"
excerpt: "이원화 DW > ONE DW 구축"
---


### Project Overview

* **기간:** 2023.03 - 2024.06
* **기술:** SQL, Shell Script, Python, Kafka, Grafana, Data Warehouse

---

### 배경 및 과제 (Background)
* **DW 구조적 한계 및 도메인 미분리:** 기존 DW가 단일 스키마 체계로 운영되어 업무 도메인 구분이 미흡했고, 이로 인한 데이터 중복 및 충돌 문제 발생
* **성능 및 확장성 부족:** 실시간 분석 수요는 점차 증가하는 반면, 기존 시스템의 대용량 배치 처리 성능 한계로 인해 시스템 전반의 유연성과 확장성이 결여됨
* **핵심 요구사항:** 업무별 도메인 중심의 마트 재구축, Kafka 기반 실시간 데이터 연계, 배치 성능 개선, 모니터링 체계 고도화 프로세스 동시 추진 필요

---

### 해결 과정 (Action)

#### 1. 도메인 기반 DW 구조 설계 
* DW를 공통, 마케팅, 카드, 리스크, 자동세탁방지 등 주요 업무 도메인별로 분리하고 각 특성에 맞는 독립적 스키마 구조 자립화

#### 2. Kafka 파이프라인 구현
* Kafka에서 실시간 수집된 JSON 로그 데이터를 DBMS 내장 함수로 파싱하여 임시 테이블에 적재하고, 필드 누락 및 중복 제어를 위한 전처리 로직 자동화 구축

<p align="center">
  <img src="{{ site.baseurl }}/img/posts/Kafka.png" alt="DW & Kafka Pipeline Architecture" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▲ Kafka 스트리밍 연계 및 차세대 DW 구조</small></em>
</p>

#### 3. 대용량 쿼리 튜닝
* 고비용 SQL 튜닝을 통해 로직 리팩토링 수행 (`NOT IN`, `NOT EXISTS` 전환, 윈도우 함수 `MIN`/`MAX` 최적화 구조 변경) 및 자주 활용되는 컬럼 중심의 테이블 재구조화
* 대량 `UPDATE` 성능 병목을 해소하기 위해 임시 테이블을 활용한 일괄 적재(Bulk Load) 방식으로 대용량 처리 기법 최적화

#### 4. Grafana 모니터링 체계 구축
<p align="center">
  <img src="{{ site.baseurl }}/img/posts/Grafana.png" alt="Vertica Monitoring" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▲ Grafana 대시 보드 연계 구조</small></em>
</p>


* Python과 가벼운 스크립트를 활용해 시스템 자원(CPU, Memory, Disk) 및 쿼리 실행 지표를 실시간 수집하고, **Grafana 대시보드**를 통해 이상 징후 조기 탐지 및 시각화 환경 구현

---

### 성과 (Result)

* **데이터 거버넌스 명확화:** 도메인 중심 DW 재구축을 통해 데이터 표준 및 관리 기준을 수립하고, 고질적인 데이터 중복 및 충돌 문제 완벽 해소
* **실시간 데이터 활용도 30% 증가:** Kafka 기반 스트리밍 연계 도입으로 데이터 활용 시점 지연을 최소화하여 비즈니스 분석 인사이트 적시 제공
* **배치 처리 시간 70% 단축:** 효율적인 SQL 튜닝 및 데이터 재구조화를 통해 대용량 데이터 적재 안정성 및 운영 효율성 극대화
* **장애 대응 속도 60% 단축:** 모니터링 자동화 및 통합 대시보드 구축으로 인프라 및 쿼리 이상 징후의 실시간 감지 및 선제적 대응 체계 확보