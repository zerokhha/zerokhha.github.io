---
layout: inner
title: "Binance API 기반 미니 데이터 파이프라인 구축"
date: 2026-07-06 00:00:00 +0900
permalink: /work/binance-mini-detail/
period: "2026.05 - "
---

###  Project Overview
바이낸스(Binance) API를 활용하여 실시간 암호화폐 거래 데이터를 수집하고, 이를 안정적으로 정제 및 적재하는 **미니 데이터 파이프라인 구축 프로젝트**입니다. 데이터 엔지니어로서 API 데이터 수집부터 저장소 적재까지의 엔드투엔드(End-to-End) 흐름을 이해하고, 효율적인 데이터 아키텍처를 고민하며 진행했습니다.

---

### Tech Stacks
* **Language:** Python
* **Data Sources:** Binance API (REST / WebSocket)
* **Storage/Infrastructure:** (PostGres, Redis)
* **Orchestration (선택):** (Airflow, dbt)

---

###  Key Implementations (주요 구현 내용)

#### 1. Binance API를 통한 데이터 수집 (Part 01)
* 바이낸스에서 제공하는 API 가이드를 분석하여 실시간 시세 및 거래 대금 데이터를 안정적으로 호출하는 수집 스크립트 구현
* API Rate Limit(호출 제한)을 초과하지 않도록 예외 처리 및 백오프(Back-off) 로직 적용

#### 2. 데이터 전처리 및 적재 파이프라인 구성 (Part 02)
* 수집된 raw JSON 데이터를 분석에 용이한 스키마 구조로 파싱 및 정제(Data Cleansing)
* 대용량 데이터 유실을 방지하기 위한 안정적인 데이터 파이프라인 설계 및 타겟 저장소 적재 완료

---

###  Insights & Learned
* **API 설계 역량:** 외부 대형 플랫폼의 API 사양을 명확히 분석하고, 제한 사항(Rate Limit)에 대응하는 견고한 수집 프로세스를 설계하는 경험을 쌓았습니다.
* **데이터 모델링 정제:** 실시간으로 쏟아지는 파편화된 비정형 데이터를 DW나 DB에 적재하기에 적합한 형태로 변환하며 데이터 정제 감각을 익혔습니다.

---

###  관련 기록 (Velog)
* [Binance Mini Project - Part 01](https://velog.io/@zerokhha/binanceminiproject01)
* [Binance Mini Project - Part 02](https://velog.io/@zerokhha/binanceminiproject02)