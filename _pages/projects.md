---
layout: default
title: Thomas Wells - Portfolio
permalink: /projects/
---

<section class="wrap page-header">
  <p class="eyebrow">Portfolio</p>
  <h1>Projects</h1>
  <p class="lead">Design, analysis, and build work, newest first.</p>
</section>

<section class="wrap">
  <div class="project-grid">
    {%- include ordered-projects.html -%}
    {%- for project in ordered_projects -%}
      {% include project-card.html project=project %}
    {%- endfor -%}
  </div>
</section>
