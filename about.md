---
layout: page
title: About
permalink: /about/
---

<!-- Last updated: 2026-07-07 -->

<style>
.about-shell {
  max-width: 1000px;
  margin: 0 auto;
  padding: 0 0 40px;
  font-family: "Pretendard", "Noto Sans KR", sans-serif;
}
.about-hero {
  padding: 34px 0 28px;
  border-bottom: 1px solid #ececec;
  margin-bottom: 24px;
}
.about-hero .eyebrow {
  font-size: 0.9rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #7c3aed;
  font-weight: 700;
  margin-bottom: 10px;
}
.about-hero h2 {
  font-size: 2.25rem;
  font-weight: 700;
  margin-bottom: 12px;
  color: #1f2937;
  line-height: 1.3;
}
.about-hero p {
  font-size: 1.18rem;
  line-height: 1.9;
  color: #6b7280;
  margin-bottom: 0;
}
.about-meta {
  margin-top: 16px;
  color: #9ca3af;
  font-size: 1.08rem;
}
.about-card {
  background: #fff;
  border: 1px solid #f0f0f0;
  border-radius: 16px;
  padding: 24px 26px;
  margin-bottom: 16px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.03);
}
.about-card h3 {
  margin-top: 0;
  margin-bottom: 12px;
  font-size: 1.25rem;
  font-weight: 700;
  color: #1f2937;
}
.about-card p,
.about-card li {
  color: #6b7280;
  line-height: 1.9;
  font-size: 1.14rem;
}
.about-card ul {
  margin-bottom: 0;
}
.about-links {
  margin-top: 14px;
}
.about-links a {
  display: inline-block;
  margin-right: 14px;
  color: #5d5d5e;
  text-decoration: none;
  font-weight: 600;
  font-size: 1.05rem;
}
.about-links a:hover {
  text-decoration: underline;
}
.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 8px;
}
.tag-list span {
  background: #f3f4f6;
  color: #6b7280;
  border: 1px solid #e5e7eb;
  padding: 7px 11px;
  border-radius: 999px;
  font-size: 1.05rem;
}
.timeline {
  list-style: none;
  padding-left: 0;
  margin: 0;
}
.timeline li {
  padding: 16px 0;
  border-bottom: 1px solid #f3f3f3;
}
.timeline li:last-child {
  border-bottom: none;
}
.timeline strong {
  color: #1f2937;
}
.timeline .project-period {
  display: block;
  margin-bottom: 4px;
  color: #8b97a3;
  font-size: 0.95rem;
  font-weight: 700;
  letter-spacing: 0.02em;
}
.timeline .project-name {
  display: block;
  color: #1f2937;
  font-weight: 700;
}
.timeline .project-summary {
  display: block;
  margin-top: 6px;
}
@media (max-width: 768px) {
  .about-hero h2 {
    font-size: 1.9rem;
  }
  .about-hero p,
  .about-card p,
  .about-card li {
    font-size: 1.08rem;
  }
  .about-card {
    padding: 20px 18px;
  }
}
</style>

<div class="about-shell">
  <section class="about-hero">
    <p class="eyebrow">about me</p>
    <h2>일하는 방식과 경험</h2>
    <p>
      단순히 데이터를 쌓는 데 그치지 않고, 실제 운영 환경에서 오래 활용될 수 있는 구조를 만드는 일을 중요하게 생각합니다.
    </p>
    <div class="about-links">
      <a href="mailto:zerokhha@gmail.com">Email</a>
      <a href="https://github.com/zerokhha" target="_blank" rel="noopener">GitHub</a>
      <a href="https://www.linkedin.com/in/kim-hayoung-1a170634a" target="_blank" rel="noopener">LinkedIn</a>
    </div>
  </section>

  <div class="about-card">
    <h3>소개</h3>
    <p>
      데이터는 결국 의사결정과 운영 효율로 연결되어야 한다고 믿습니다.
      그래서 단순한 적재/조회가 아니라, 모델링부터 배치 안정화, 장애 대응,
      성능 개선까지 운영 가능한 구조로 완성하는 데 집중해왔습니다.
    </p>
    <p>
      최근에는 기존 DW 업무에 자동화와 AI 기반 관제를 접목해,
      반복 운영 업무를 줄이고 이상 징후를 빠르게 탐지하는 방식으로 확장하고 있습니다.
    </p>
  </div>

  <div class="about-card">
    <h3>일하는 방식</h3>
    <ul>
      <li>문제의 원인을 먼저 구조적으로 정의하고, 재현 가능한 방식으로 해결합니다.</li>
      <li>운영 환경에서 오래 버티는 설계를 우선하고, 변경 비용을 줄이는 선택을 합니다.</li>
      <li>성능 수치와 품질 지표를 함께 보며 개선 효과를 검증합니다.</li>
    </ul>
  </div>

  <div class="about-card">
    <h3>Skills</h3>
    <div class="tag-list">
      <span>대용량 DW</span>
      <span>성능 최적화</span>
      <span>운영 자동화</span>
      <span>데이터 관제</span>
      <span>AI 활용</span>
      <span>시스템 안정성</span>
    </div>
  </div>

  <div class="about-card">
    <h3>프로젝트</h3>
    <ul class="timeline">
      <li><span class="project-period">2021.03 - 현재</span><strong class="project-name">금융권 DW 운영 컨설팅</strong><span class="project-summary">Vertica 기반 대용량 DW 상시 운영·성능·안정성</span></li>
      <li><span class="project-period">2026.04 - 2026.05</span><strong class="project-name">Vertica 신규 클러스터 구축 (AI 러닝 플랫폼)</strong><span class="project-summary">고객사 환경 현장 설치·초기 구성 · 운영 가이드 인수인계</span></li>
      <li><span class="project-period">2025.01 - 2025.05</span><strong class="project-name">정보계 DW 성능 개선</strong><span class="project-summary">Direct Read 전환 + Auto-kill 시스템 · 악성 쿼리 자원 독점 차단 및 장애 전파 차단</span></li>
      <li><span class="project-period">2023.03 - 2024.06</span><strong class="project-name">통합 DW 구축</strong><span class="project-summary">도메인 재설계 + MyData Kafka 준실시간 연계 · 배치 시간 70% 단축</span></li>
      <li><span class="project-period">2022.07</span><strong class="project-name">분석계 DW 구축 PoC</strong><span class="project-summary">쿼리 리팩토링·병렬화 · 배치 50%↓ · 사업 수주 기여</span></li>
      <li><span class="project-period">2022.03 - 2022.06</span><strong class="project-name">공공 빅데이터 인프라 확대</strong><span class="project-summary">RBAC + 마이그레이션 자동화 90% · 구축기간 50%↓</span></li>
      <li><span class="project-period">2021.06 - 2021.10</span><strong class="project-name">비즈니스 업무 이관</strong><span class="project-summary">Hadoop→Vertica · 7TB / 4시간 이내 병렬 적재 자동화</span></li>
    </ul>
  </div>

  <div class="about-card">
    <h3>관심 영역</h3>
    <p>
      데이터 플랫폼의 안정성과 관측 가능성(Observability), 데이터 품질 관리,
      그리고 생성형 AI를 활용한 운영 자동화에 지속적으로 관심을 두고 있습니다.
    </p>
  </div>
</div>
