---
title: "Tech"
permalink: /tech/
layout: single
author_profile: true
---

최신 AI 모델, 실전 개발 도구, 홈랩 인프라와 개발 경험을 다루는 기술 블로그 공간입니다.

## 카테고리 바로가기

<div style="display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 2em;">
  <a href="/categories/#ai" class="btn btn--primary">🤖 AI</a>
  <a href="/categories/#devtools" class="btn btn--info">🛠 DevTools</a>
  <a href="/categories/#server" class="btn btn--success">🏠 Server / Home Lab</a>
  <a href="/categories/#development" class="btn btn--warning">💻 Development</a>
</div>

---

## 🤖 AI

최신 프론티어 LLM, 로컬 모델, 에이전트 워크플로우에 대한 실전 분석과 리뷰입니다.

{% assign ai_posts = site.categories['ai'] %}
<div class="entries-list">
  {% for post in ai_posts limit:5 %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
{% if ai_posts.size > 5 %}
  <p><a href="/categories/#ai" class="btn btn--inverse btn--small">AI 글 전체보기 (총 {{ ai_posts.size }}개) &rarr;</a></p>
{% endif %}

---

## 🛠 DevTools

Claude Code, Cursor, Git, 터미널 생산성 도구 등 실전 개발 도구 활용기입니다.

{% assign dev_posts = site.categories['devtools'] %}
{% if dev_posts.size > 0 %}
<div class="entries-list">
  {% for post in dev_posts limit:5 %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
{% if dev_posts.size > 5 %}
  <p><a href="/categories/#devtools" class="btn btn--inverse btn--small">DevTools 글 전체보기 (총 {{ dev_posts.size }}개) &rarr;</a></p>
{% endif %}
{% endif %}

---

## 🏠 Server & Home Lab

맥미니 홈서버 운영, PM2, 네트워크 및 인프라 자동화 구축 기록입니다.

{% assign server_posts = site.categories['server'] %}
{% if server_posts.size > 0 %}
<div class="entries-list">
  {% for post in server_posts limit:5 %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
{% if server_posts.size > 5 %}
  <p><a href="/categories/#server" class="btn btn--inverse btn--small">Server 글 전체보기 (총 {{ server_posts.size }}개) &rarr;</a></p>
{% endif %}
{% endif %}
