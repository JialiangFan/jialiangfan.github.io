---
permalink: /
title: "About"
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

I am a PhD student (2024 - present) at [University of Notre Dame](https://www.nd.edu/) in the Department of Computer Science and Engineering, supervised by [Prof. Fanxin Kong](https://sites.google.com/site/fanxink). I also work closely with [Prof. Insup Lee](https://www.cis.upenn.edu/~lee/) and [Prof. Oleg Sokolsky](https://www.cis.upenn.edu/~sodarisa/). I received my B.E. in Software Engineering from [Shandong University](https://en.wikipedia.org/wiki/Shandong_University) in 2019 and my M.S. in Computer Technology from [Lanzhou University](https://en.wikipedia.org/wiki/Lanzhou_University) in 2023.

My research interests include **cyber-physical systems**, **robotics**, **reinforcement learning**, **safe learning**, and **LLM post-training for safety alignment**.

## News

- **[Feb 2026]** Our paper "[SafeGen-LLM: Enhancing Safety Generalization in Task Planning for Robotic Systems](https://arxiv.org/abs/2602.24235)" is now available on arXiv! (First author)
- **[Feb 2026]** Our paper "[Vulnerability Analysis of Safe Reinforcement Learning via Inverse Constrained Reinforcement Learning](https://arxiv.org/abs/2602.16543)" is now available on arXiv! (First author)
- **[Jan 2026]** Our paper "SafeNet: A Neural-Symbolic Network for Safe Planning in Robotic Systems using Formal Method-Guided LLM Fine-Tuning" has been accepted to **ICRA 2026**! (Second author)
- **[Jan 2026]** Our paper "Vulnerability Exploration of Safe Reinforcement Learning in Cyber-Physical Systems via STL Mining" has been accepted to **ICCPS 2026**! (First author)
- **[Apr 2025]** I went to Orlando for the SPIE Defense + Commercial Sensing conference.
- **[Jun 2024]** I joined the University of Notre Dame as a PhD student supervised by [Prof. Fanxin Kong](https://sites.google.com/site/fanxink).

## Project Videos

<div class="video-grid-home">
{% for post in site.portfolio reversed %}
<div class="video-card-home">
  {% if post.youtube_id %}
  <div class="video-container">
    <iframe src="https://www.youtube.com/embed/{{ post.youtube_id }}" frameborder="0" allowfullscreen allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"></iframe>
  </div>
  {% endif %}
  <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
  {% if post.excerpt %}<p>{{ post.excerpt }}</p>{% endif %}
</div>
{% endfor %}
</div>

<style>
.video-grid-home {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
  gap: 1.5em;
  margin-top: 1em;
}
.video-card-home {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  padding-bottom: 0.5em;
}
.video-card-home h3 {
  padding: 0.5em 1em 0;
  margin: 0;
}
.video-card-home p {
  padding: 0 1em;
  margin: 0.25em 0;
  font-size: 0.9em;
  color: #555;
}
.video-container {
  position: relative;
  padding-bottom: 56.25%;
  height: 0;
  overflow: hidden;
}
.video-container iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
