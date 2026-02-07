---
layout: about
title: <strong>Jialiang</strong> Fan
permalink: /
subtitle: PhD student, University of Notre Dame

profile:
  align: right
  image: jialiangfan1.jpg
  image_circular: false # crops the image to make it circular
  more_info: >


news: true # includes a list of news items
selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page
academic_service: false
selected_honor: fasle
---
{% assign current_year = site.time | date: "%Y" | plus: 0 %}{% assign phd_year = current_year | minus: 2024 %}{% if phd_year == 1 %}{% assign year_text = "first" %}{% elsif phd_year == 2 %}{% assign year_text = "second" %}{% elsif phd_year == 3 %}{% assign year_text = "third" %}{% elsif phd_year == 4 %}{% assign year_text = "fourth" %}{% elsif phd_year == 5 %}{% assign year_text = "fifth" %}{% else %}{% assign year_text = phd_year %}{% endif %}I am currently a {{ year_text }}-year PhD student at the [University of Notre Dame](https://www.nd.edu/) majoring in Computer Science Engineering, supervised by Professor [Fanxin Kong](https://sites.google.com/site/fanxink). I received the B.E. degree in Software Engineer from [Shandong University](https://www.en.sdu.edu.cn/), Jinan, China, in 2019. I obtained the Master's degree, majoring in Computer Technology with the School of Information Science and Engineering, [Lanzhou University](https://en.lzu.edu.cn/), Lanzhou, China, in 2023. My main research interests include cyber-physical systems, robotics, reinforcement learning, safe learning, and LLM post-training for safety alignment.
