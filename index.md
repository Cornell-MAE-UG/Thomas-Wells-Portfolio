---
layout: default
title: Thomas Wells
---

<section class="hero">
  <div class="wrap hero-inner">
    <div class="hero-text">
      <p class="eyebrow">Mechanical &amp; Aerospace Engineering · Cornell</p>
      <h1>{{ site.name }}</h1>
      <p class="lead">{{ site.subtitle }}</p>
      <div class="hero-actions">
        <a class="btn btn-accent" href="{{ "/projects/" | relative_url }}">View projects <i class="bi bi-arrow-right"></i></a>
        <a class="btn btn-outline" href="{{ "/assets/CV.pdf" | relative_url }}"><i class="bi bi-download"></i> Download CV</a>
      </div>
    </div>
    <img class="profile-image hero-photo" src="{{ "/assets/images/profile-pic.jpg" | relative_url }}" alt="Portrait of {{ site.name }}">
  </div>
</section>

<section class="wrap section">
  <div class="section-head">
    <h2>About me</h2>
  </div>
  <div class="about-body">
    <p>Hi! My name is {{ site.name }}, and I’m a young aspiring engineer with a strong foundation in mechanical design and rapid prototyping. I'm deeply passionate about high precision mechatronics and additive manufacturing. I have a growing track record of delivering complex projects from concept through completion. My experience spans hands-on design work, technical analysis, and cross-functional project management. I've developed a unique ability to bridge creative problem-solving with practical execution.</p>
    <p>Within this portfolio, you’ll find examples of CAD models, prototype builds, design iterations, and finalized solutions that showcase my approach: user-focused design, data-driven decisions, and clean, manufacturable engineering. Whether it’s designing mechanisms, refining parts and assemblies for manufacturability, or keeping a project on track, I’m driven by curiosity, precision, and the desire to build things that work well.</p>
  </div>
</section>

<section class="wrap section">
  <div class="section-head">
    <h2>Selected work</h2>
    <a href="{{ "/projects/" | relative_url }}">All projects <i class="bi bi-arrow-right"></i></a>
  </div>
  <div class="project-grid">
    {%- include ordered-projects.html -%}
    {%- for project in ordered_projects limit:3 -%}
      {% include project-card.html project=project %}
    {%- endfor -%}
  </div>
</section>
