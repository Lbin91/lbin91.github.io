---
title: "Hobby"
permalink: /hobby/
layout: single
author_profile: true
---

개발 모니터 밖의 일상, 진심으로 파고드는 취미 생활을 기록하는 공간입니다. 게임 플레이 경험담, 영상 콘텐츠 감상 및 다양한 취미를 다룹니다.

## 카테고리 바로가기

<div style="display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 2em;">
  <a href="#game" class="btn btn--primary">🎮 게임 (Game)</a>
  <a href="#wardogs" class="btn btn--info">⚔️ 워독스 (WARDOGS)</a>
  <a href="#video" class="btn btn--success">🎬 영상 (Video)</a>
</div>

---

<h2 id="game">🎮 게임 (Game)</h2>

PC/스팀, 콘솔 등 장르 가리지 않고 깊게 즐겨본 게임들의 플레이 후기와 공략, 잡담을 정리합니다.

<h3 id="wardogs">⚔️ 워독스 (WARDOGS)</h3>

> 영국 Bulkhead가 개발한 최대 100인 대규모 밀리터리 샌드박스 FPS. 세 개 PMC 진영 간의 3파전 전면전과 기여도 기반 장비 구매/손실 경제 시스템을 다룹니다.

{% assign wardogs_posts = site.categories['wardogs'] %}
{% if wardogs_posts.size > 0 %}
  <div class="entries-list">
    {% for post in wardogs_posts %}
      {% include archive-single.html type="list" %}
    {% endfor %}
  </div>
{% else %}
  <div class="notice">
    <p>워독스 게시글을 준비 중입니다.</p>
  </div>
{% endif %}

{% assign all_game_posts = site.categories['game'] %}
{% if all_game_posts.size > wardogs_posts.size %}
  <h4>기타 게임 포스트</h4>
  <div class="entries-list">
    {% for post in all_game_posts %}
      {% unless post.categories contains 'wardogs' %}
        {% include archive-single.html type="list" %}
      {% endunless %}
    {% endfor %}
  </div>
{% endif %}

---

<h2 id="video">🎬 영상 (Video)</h2>

영화, 드라마, 애니메이션, 유튜브 다큐 등 인상 깊게 본 영상 콘텐츠에 대한 솔직한 감상과 생각을 기록합니다.

{% assign video_posts = site.categories['video'] %}
{% if video_posts.size > 0 %}
  <div class="entries-list">
    {% for post in video_posts %}
      {% include archive-single.html type="list" %}
    {% endfor %}
  </div>
{% else %}
  <div class="notice--info">
    <p><strong>🎬 영상 카테고리 준비 중입니다!</strong></p>
    <p>영화, OTT 드라마 시리즈, 유튜브 추천 영상 등 흥미롭게 본 작품들의 리뷰가 곧 등록될 예정입니다.</p>
  </div>
{% endif %}
