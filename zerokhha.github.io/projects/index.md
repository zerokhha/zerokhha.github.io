---
layout: page
title: Projects
permalink: /projects/
---

# Projects
개인 프로젝트 공간 입니다.

---

### 최근 기록들
<ul>
  {% for post in site.categories.kb %}
    <li>
      <span style="color: #666;">{{ post.date | date: "%Y-%m-%d" }}</span> — 
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

{% if site.categories.kb.size == 0 %}
<p>아직 작성된 글이 없습니다. <code>_posts</code> 폴더에 카테고리를 <code>kb</code>로 설정한 글을 추가해 보세요!</p>
{% endif %}