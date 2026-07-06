---
layout: inner
title: "Vertica AI 관제 및 ChatOps 시스템 구축"
date: 2026-07-06 00:00:00 +0900
permalink: /work/vertica-ai-monitoring-detail/
period: "2026.05 - "
---

###  Project Overview
* Vertica 대용량 DW를 운영하며 반복적으로 확인해야 하는 핵심 관리 지표들을 자동화하고, OpenAI와 Telegram을 연동하여 장애 조치를 보조하는 **Vertica 특화 AI-Ops 관제 시스템**입니다. 
일반적인 인프라 모니터링 툴이 캐치하기 힘든 Vertica 고유의 아키텍처적 위험 징후(Delete Vector, Spread Retrans, Tuple Mover 등)를 탐지하고, 이상 징후 발생 시 AI가 과거 장애 이력 맥락을 파악하여 최적의 SQL 조치 가이드를 제공하도록 설계했습니다.

---

###  Tech Stacks
* **Backend:** Python, FastAPI, APScheduler
* **Frontend:** Python, Dash, Plotly
* **AI Ecosystem:** OpenAI API (gpt-4o-mini)
* **Storage / ChatOps:** SQLite, Telegram Bot API
* **Data Input:** Vertica Real-time / Daily / Monthly JSON Report

---

###  Key Features

#### 1. Vertica 특화 다차원 관제 화면 구성 (Dash)
단순 실시간 조회를 넘어 Vertica 운영 주기에 맞춘 3가지 뷰(View)를 제공합니다.
* **Real-time View:** 액티브 세션 및 쿼리 큐 대기 추이, Resource Pool, Spread Retrans 상태, Lock Wait TOP 10 모니터링
* **Daily View:** 일간 스토리지 적재 추이, Delete Vector / 통계 최신화 / Tuple Mover 상태 관리, 배치 분포 및 미인가 IP 체크
* **Monthly View:** 월간 용량 고갈 예측 알고리즘, Data Skew(편중 현상) 분석, Projection 재구성 및 ILM 대상 테이블 식별

#### 2. 장애 이력 기반 피드백 루프 구축 (Context-Aware AI Diagnosis)
* **자동 장애 감지:** 백엔드 스케줄러가 리포트를 주기적으로 분석하여 LOCK WAIT, Retrans Spread Events 등 위험 임계치 초과 시 자동 Alert 발동
* **자가 학습 구조:** 감지된 장애와 AI 조치안을 SQLite에 적재합니다. 차후 AI 진단 시 과거 장애 이력을 프롬프트 텍스트에 동적으로 포함하여, 반복 장애에 대해 더 정밀하고 연속성 있는 맥락 기반 솔루션을 출력하도록 구현했습니다.

#### 3. Telegram ChatOps를 통한 원격 모바일 대응
* 사용자가 모바일에서 `/diagnose` 또는 `/진단` 명령을 전송하면 백엔드 내부 폴링 스레드가 이를 수신합니다.
* 즉시 OpenAI API를 호출하여 현재 DW 상태 요약과 조치 가이드를 메신저로 즉시 회신하는 환경을 구축했습니다.

---

### Engineering Insights & Points
* **데이터 엔지니어 관점의 문제 해결:** 무거운 모니터링 솔루션을 직접 띄우기 힘든 환경에서, Vertica 내부 시스템 테이블 쿼리 결과(JSON)를 파이프라인화하여 가볍고 실용적인 독립 관제 아키텍처를 완성했습니다.
* **실무 지향적 AI 활용:** 모델 자체의 거대함보다 운영 데이터와 과거 이력(DB)을 프롬프트 엔지니어링 템플릿과 결합하여, 실제 인프라 운영자가 바로 사용할 수 있는 수준의 'SQL 가이드라인'을 뽑아내는 AI-Ops 흐름을 지향했습니다.

---

###  How to Run (Quick Start)
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
---