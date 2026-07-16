---
layout: inner
title: "Binance API 기반 미니 데이터 파이프라인 구축"
date: 2026-07-06 00:00:00 +0900
published: false
permalink: /work/binance-mini-project/
period: "2026.05 - "
excerpt: "Binance API로 실시간 암호화폐 거래 데이터를 수집하고 정제·적재하는 미니 데이터 파이프라인을 구축했습니다."
---

<!-- callout-start -->
<div class="work-card">
	<div class="work-card__meta">
		<span class="period">2026.05 -</span>
		<span class="tech">Python · Binance API · Redis</span>
	</div>
	<h2 class="work-card__title">Binance API 기반 미니 데이터 파이프라인 구축</h2>
	<p class="work-card__lede">Binance API로 실시간 거래 데이터를 수집·정제해 안정적인 파이프라인을 구축했습니다.</p>
	<div class="work-card__metrics">
		<div class="metric"><div class="metric__num">Real-time</div><div class="metric__label">데이터 수집</div></div>
		<div class="metric"><div class="metric__num">ETL</div><div class="metric__label">전처리·적재</div></div>
	</div>
</div>
<!-- callout-end -->

---

### 배경 및 과제 (Background)

<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **실시간 데이터 수집 요구:** Binance API를 통해 암호화폐 시세와 거래 데이터를 지속적으로 확보해야 하는 필요성이 있었습니다.
* **파이프라인 안정성 확보:** 수집된 데이터를 정제하고 저장소에 안정적으로 적재하는 엔드투엔드 흐름을 설계해야 했습니다.
* **운영 가시성 확보:** API 호출 제한과 데이터 유실 가능성을 방지하며 지속 가능한 파이프라인을 구성해야 했습니다.
</section>

---

### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. Binance API를 통한 데이터 수집
* 바이낸스 제공 API 가이드를 분석해 실시간 시세와 거래 대금 데이터를 안정적으로 호출하는 수집 스크립트를 구현했습니다.
* API Rate Limit을 초과하지 않도록 예외 처리와 백오프 로직을 적용해 호출 안정성을 높였습니다.
</div>

<div class="post-step" markdown="1">
#### 2. 데이터 전처리 및 적재 파이프라인 구성
* 수집된 raw JSON 데이터를 분석에 용이한 스키마 구조로 파싱하고 정제했습니다.
* 대용량 데이터 유실을 방지하기 위한 안정적인 파이프라인 설계와 저장소 적재 흐름을 구성했습니다.
</div>
</section>

---

### 성과 (Result)

<section class="post-section post-section--result" markdown="1">
### 성과 (Result)
* **API 기반 데이터 수집 역량 확보:** 실시간 데이터 소스와의 연동 경험을 통해 안정적인 수집 프로세스를 설계할 수 있었습니다.
* **데이터 파이프라인 설계 역량 강화:** 비정형 데이터 정제와 저장소 적재까지 이어지는 엔드투엔드 흐름을 직접 경험하며 데이터 엔지니어링 감각을 높였습니다.
</section>

---

### 관련 기록 (Velog)
* [Binance Mini Project - Part 01](https://velog.io/@zerokhha/binanceminiproject01)
* [Binance Mini Project - Part 02](https://velog.io/@zerokhha/binanceminiproject02)