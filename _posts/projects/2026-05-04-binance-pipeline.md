---
layout: post
title: "Mini Project"
date: 2026-05-04 10:00:00 +0900
category: [projects]
---

#  Binance 실시간 암호화폐 데이터 파이프라인

실시간 체결 데이터를 수집·적재·변환·스케줄링하는 end-to-end 데이터 파이프라인 프로젝트입니다.

---

## 아키텍처

```
Binance WebSocket
    └─→ Redis (버퍼)
        └─→ PostgreSQL (market_indicators)
                └─→ dbt staging (stg_market_indicators)
                          └─→ dbt marts
                                ├─ mrt_ohlcv
                                └─ mrt_vol_imbalance
                                └─→ Airflow (1시간 주기 스케줄링)

장애 발생 시
    └─→ backfill.py (Binance Kline REST API → PostgreSQL 직접 적재)
```

---

## 기술 스택

| 영역 | 기술 |
|------|------|
| 데이터 수집 | Python, Binance WebSocket API |
| 버퍼링 | Redis |
| 저장소 | PostgreSQL 15 |
| 변환 | dbt (dbt-postgres) |
| 스케줄링 | Apache Airflow 3.x |
| 운영 | systemd |

---

## 프로젝트 구조

```
binance/
├── 01_redis_producer.py       # WebSocket 실시간 수집 → Redis 적재
├── 02_redis_consumer.py       # Redis 소비 → PostgreSQL 60초 집계 적재
├── 03_backfill.py             # 장애 구간 Kline REST API 복구
└── binance_dbt/
    └── models/
        ├── staging/
        │   ├── sources.yml
        │   └── stg_market_indicators.sql
        └── marts/
            ├── mrt_ohlcv.sql
            └── mrt_vol_imbalance.sql
```

---

## 데이터 흐름 상세

### 1. 실시간 수집 (producer)
- Binance WebSocket `@trade` 스트림으로 5개 심볼 실시간 체결 데이터 수신
- 수신 즉시 `asyncio.Queue`에 적재 후 Redis Pipeline으로 배치 전송
- 대상 심볼: `BTCUSDT`, `ETHUSDT`, `SOLUSDT`, `XRPUSDT`, `BNBUSDT`

### 2. 집계 적재 (consumer)
- Redis `crypto_stream` 큐를 60초 주기로 소비
- 심볼별 매수/매도 거래량, 평균가, 체결 강도(vol_power) 집계
- PostgreSQL `market_indicators` 테이블에 Bulk Insert

### 3. 데이터 변환 (dbt)

**Staging 레이어** (`stg_market_indicators`)
- 원본 테이블 타입 정제 및 컬럼 표준화
- `total_vol` 파생 컬럼 추가

**Marts 레이어**
- `mrt_ohlcv`: 1분봉 OHLCV 캔들 테이블 (incremental)
- `mrt_vol_imbalance`: 매수/매도 불균형 분석 및 시장 시그널 분류

### 4. 스케줄링 (Airflow)
- 1시간 주기 DAG으로 `dbt run → dbt test` 순차 실행
- systemd로 백그라운드 상시 운영

### 5. 장애 복구 (backfill)
- WebSocket 장애 발생 시 Binance Kline REST API로 해당 구간 복구
- 복구 데이터는 1분봉 단위 근사치로 적재 (tick 단위 정밀도와 차이 있음)
- 향후 `is_backfill` 플래그로 실제 데이터와 구분 가능

---

## 설계 의도

### Redis를 버퍼로 사용한 이유
WebSocket 수신 속도와 PostgreSQL 적재 속도 간의 차이를 완충하기 위해 Redis를 중간 버퍼로 도입했습니다. 수신 루프를 최소화하고 별도 워커에서 배치 처리함으로써 데이터 유실 위험을 줄였습니다.

### 60초 집계 후 적재하는 이유
tick 단위 전체를 적재하면 PostgreSQL 부하가 과도해지므로, 60초 단위로 집계 후 Bulk Insert 방식으로 적재해 성능과 저장 효율을 확보했습니다.

### dbt 레이어를 staging/marts로 분리한 이유
원본 데이터 변경 없이 정제 로직을 staging에서 담당하고, 비즈니스 로직은 marts에서만 관리함으로써 관심사를 분리하고 재사용성을 높였습니다.

### incremental 모델을 적용한 이유
매 실행 시 전체 테이블을 재생성하는 방식은 데이터가 쌓일수록 실행 시간이 증가하므로, 마지막 적재 시각 이후의 신규 데이터만 추가하는 incremental 방식으로 전환해 성능을 확보했습니다.

### backfill을 별도 스크립트로 분리한 이유
실시간 수집(producer)과 장애 복구(backfill)는 역할과 실행 주기가 다르므로 분리했습니다. backfill은 필요할 때만 일회성으로 실행하며, Redis를 거치지 않고 PostgreSQL에 직접 적재해 복구 속도를 높였습니다.

---

## 실행 방법

### 환경 설정
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install python-binance redis sqlalchemy pandas aiohttp apache-airflow dbt-postgres
```

### 수집 파이프라인 실행
```bash
# producer (WebSocket 수집)
python3 01_redis_producer.py

# consumer (Redis → PostgreSQL)
python3 02_redis_consumer.py
```

### dbt 실행
```bash
cd binance_dbt
dbt run
dbt test
```

### Airflow 실행
```bash
export AIRFLOW_HOME=~/airflow
airflow standalone
```

### 장애 구간 backfill
```bash
# 03_backfill.py 내 start/end 시각 수정 후 실행
python3 03_backfill.py
```

---

## 한계 및 개선 방향

- backfill 데이터는 Kline 1분봉 기반 근사치로, tick 단위 정밀도와 차이가 있음
- `is_backfill` 플래그 컬럼 추가로 실제 데이터와 구분 예정
- WebSocket 이중화로 장애 발생 자체를 최소화할 수 있음
- Spark 기반 대용량 배치 처리로 확장 가능