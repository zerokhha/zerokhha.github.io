---
layout: inner
title: "Vertica AI 관제 및 ChatOps 시스템 구축"
date: 2026-07-06 00:00:00 +0900
permalink: /work/vertica-ai-monitoring-system/
period: "2026.05 - "
excerpt: "Vertica 특화 대시보드와 AI 기반 ChatOps를 결합해 운영 지표 자동화와 장애 대응을 지원하는 관제 시스템을 구축했습니다."
---

<!-- callout-start -->
<div class="work-card">
  <div class="work-card__meta">
    <span class="period">2026.05 -</span>
    <span class="tech">Python · FastAPI · OpenAI · Dash</span>
  </div>
  <h2 class="work-card__title">Vertica AI 관제 및 ChatOps 시스템 구축</h2>
  <p class="work-card__lede">Vertica 특화 모니터링과 ChatOps를 결합해 이상징후 탐지와 대응을 자동화했습니다.</p>
  <div class="work-card__metrics">
    <div class="metric"><div class="metric__num">AI</div><div class="metric__label">관제 자동화</div></div>
    <div class="metric"><div class="metric__num">ChatOps</div><div class="metric__label">원격 대응</div></div>
  </div>
</div>
<!-- callout-end -->

---

### 배경 및 과제 (Background)

<section class="post-section post-section--background" markdown="1">
### 배경 및 과제 (Background)
* **Vertica 운영 지표 자동화 필요:** 대용량 DW 운영 중 반복적으로 확인해야 하는 핵심 관리 지표를 자동화해 운영 부담을 줄일 필요가 있었습니다.
* **AI 기반 장애 보조 요구:** 일반적인 모니터링 도구로는 포착하기 어려운 Vertica 고유 위험 징후를 빠르게 식별하고, 대응 가이드를 자동으로 제시할 수 있어야 했습니다.
</section>

---

### 해결 과정 (Action)

<section class="post-section post-section--action" markdown="1">
<div class="post-step" markdown="1">
#### 1. Vertica 특화 다차원 관제 화면 구성 (Dash)
* 단순 실시간 조회를 넘어 Vertica 운영 주기에 맞춘 3가지 뷰를 구성해 운영 상황을 한눈에 파악할 수 있게 했습니다.
* **Real-time View:** 액티브 세션, 큐 대기, Resource Pool, Spread Retrans, Lock Wait TOP 10을 모니터링합니다.
* **Daily View:** 일간 스토리지 적재 추이, Delete Vector, 통계 최신화, Tuple Mover 상태, 배치 분포, 미인가 IP를 관리합니다.
* **Monthly View:** 월간 용량 고갈 예측, Data Skew 분석, Projection 재구성, ILM 대상 테이블 식별을 지원합니다.
</div>

<div class="post-step" markdown="1">
#### 2. 장애 이력 기반 피드백 루프 구축 (Context-Aware AI Diagnosis)
* 백엔드 스케줄러가 리포트를 주기적으로 분석해 LOCK WAIT, Retrans Spread Events 등 위험 임계치를 초과하면 자동 Alert를 발동하도록 구성했습니다.
* 감지된 장애와 AI 조치안을 SQLite에 적재해, 이후 진단 시 과거 이력을 반영한 맥락 기반 조치 가이드를 제공하도록 구현했습니다.
</div>

<div class="post-step" markdown="1">
#### 3. Telegram ChatOps를 통한 원격 모바일 대응
* 사용자가 모바일에서 `/diagnose` 또는 `/진단` 명령을 전송하면 백엔드 폴링 스레드가 이를 수신하고, 즉시 OpenAI API를 호출해 현재 DW 상태 요약과 조치 가이드를 메신저로 회신하도록 구성했습니다.
</div>
</section>

---

### 성과 (Result)

<section class="post-section post-section--result" markdown="1">
### 성과 (Result)
* **운영 지표 자동화:** Vertica 운영 상 핵심 이슈를 자동 수집·시각화해 반복 확인 부담을 줄였습니다.
* **장애 대응 속도 향상:** AI 기반 진단과 ChatOps 연동으로 이상 징후 발생 시 신속한 대응 경로를 확보했습니다.
* **실무 적용성 강화:** 운영 데이터와 과거 이력을 활용한 AI 가이드라인으로 실제 인프라 운영자의 의사결정을 돕는 흐름을 만들었습니다.
</section>

---

### 실행 방법 (Quick Start)
```python
# Env Settings
OPENAI_API_KEY=your_openai_api_key
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
TELEGRAM_CHAT_ID=your_telegram_chat_id

# Run Backend (Port: 8000)
python backend.py

# Run Frontend (Port: 8050)
python frontend.py
```

---

### 구현 화면
<p align="center">
  <img src="{{ site.baseurl }}/img/posts/vertica_ai_mon1.png" alt="Vertica AI Monitoring" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <div></div>
  <img src="{{ site.baseurl }}/img/posts/vertica_ai_mon2.png" alt="Vertica AI Monitoring" width="100%" style="max-width: 800px; border: 1px solid #ddd; border-radius: 8px;">
  <br>
  <em><small>▼ Vertica ChatBot</small></em>
  <br>
  <img src="{{ site.baseurl }}/img/posts/vertica_chatbot.png" alt="Vertica AI Monitoring" width="100%" style="max-width: 500px; border: 1px solid #ddd; border-radius: 8px;">
</p>
