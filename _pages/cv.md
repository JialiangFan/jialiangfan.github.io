---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

You can download my full CV [here](/files/CV_Jialiang_Fan-update.pdf).

Education
======
* Ph.D. in Computer Science and Engineering, University of Notre Dame, 2024 - present
* M.S. in Computer Technology, Lanzhou University, 2020 - 2023
* B.E. in Software Engineering, Shandong University, 2015 - 2019

Research Interests
======
* Cyber-physical systems
* Robotics
* Reinforcement learning
* Safe learning
* LLM post-training for safety alignment

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
