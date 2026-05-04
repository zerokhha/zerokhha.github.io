---
layout: page
title: Knowledge Base
permalink: /kb/
---

<div class="kb-container">
  <p class="kb-description">배우고 익힌 내용들을 정리하는 공간입니다.</p>
  
  <div class="kb-divider"></div>

  <h3 class="kb-subtitle">최근 기록들</h3>
  
  <ul class="kb-list">
    {% for post in site.categories.kb %}
      <li class="kb-item">
        <span class="kb-date">{{ post.date | date: "%Y.%m.%d" }}</span>
        <a class="kb-link" href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>

  {% if site.categories.kb.size == 0 %}
    <p class="kb-empty">아직 등록된 노트가 없습니다. 첫 글을 작성해 보세요!</p>
  {% endif %}
</div>

<style>
  /* 전체 컨테이너 중앙 정렬 및 여백 */
  .kb-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px 0;
    text-align: center;
  }

  .kb-description {
    color: #888;
    font-size: 1.1rem;
    margin-bottom: 20px;
  }

  /* 구분선 */
  .kb-divider {
    height: 1px;
    background: linear-gradient(to right, transparent, #444, transparent);
    margin: 30px 0;
  }

  .kb-subtitle {
    font-size: 1.5rem;
    margin-bottom: 30px;
    font-weight: 600;
  }

  /* 리스트 스타일 제거 및 간격 */
  .kb-list {
    list-style: none;
    padding: 0;
    margin: 0;
    text-align: left; /* 리스트 내용은 왼쪽 정렬이 읽기 편해요 */
  }

  .kb-item {
    display: flex;
    align-items: center;
    padding: 15px 10px;
    border-bottom: 1px solid #222;
    transition: background 0.3s ease;
  }

  .kb-item:hover {
    background: #111; /* 마우스 올렸을 때 살짝 밝아짐 */
  }

  /* 날짜 스타일 */
  .kb-date {
    font-family: 'Courier New', Courier, monospace;
    color: #666;
    font-size: 0.9rem;
    margin-right: 20px;
    min-width: 100px;
  }

  /* 제목 스타일 */
  .kb-link {
    color: #eee;
    text-decoration: none;
    font-size: 1.1rem;
    font-weight: 400;
    transition: color 0.3s ease;
  }

  .kb-link:hover {
    color: #3498db; /* 테마의 포인트 컬러가 있다면 변경 가능 */
    text-decoration: underline;
  }

  .kb-empty {
    color: #555;
    margin-top: 50px;
  }
</style>