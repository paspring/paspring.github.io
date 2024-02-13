---
layout: home
entries_layout: grid
classes: wide
author_profile: true
feature_row_portfolio:
  - image_path: /assets/images/portfolio.jpg
    alt: "Portfolio"
    excerpt: "Here is a showcase of my professional expertise in data science, analytics, and visualization. This portfolio provides a comprehensive overview of my proficiency across various data roles, featuring examples of past projects and highlighting areas I aspire to explore in the future."
    url: "/portfolio/"
    btn_class: "btn--primary"
    btn_label: "Enter Portfolio"

features_extras_about_me: 
  - image_path: /assets/images/work-experience.jpg
    alt: "work experience"
    title: "Work Experience"
    excerpt: "Discover the roles and positions I've held throughout my career, along with valuable insights into my professional journey and accomplishments."
    url: "/experiences-general/"
    btn_class: "btn--primary"
    btn_label: "Read More"

  - image_path: /assets/images/resume.jpg
    alt: "resume"
    title: "Resume"
    excerpt: "Click the button below to access my resume and learn more about my qualifications and professional background."
    url: "/about/"
    btn_class: "btn--primary"
    btn_label: "Read More"

  - image_path: /assets/images/contact-me.jpg
    alt: "contact"
    title: "Contact"
    excerpt: "Get in touch with me. I'd love to hear from you and assist with any inquiries, comments, or collaborations. Fill out the form below, and I'll get back to you promptly"
    url: "/contact/"
    btn_class: "btn--primary"
    btn_label: "Read More"

features_extras_learning:
  - image_path: /assets/images/continuous-learning.webp
    alt: "continuous learning"
    title: "Continuous Learning"
    excerpt: "Explore the courses fueling my expertise in data science, analytics, and visualization —a journey through the knowledge that shapes my skill set"
    url: "/continuous-learning/"
    btn_class: "btn--primary"
    btn_label: "Read More"

  - image_path: /assets/images/education.webp
    alt: "education background"
    title: "Educational Background"
    excerpt: "Explore My Academic Journey, including studies in physics, education, and engineering, along with insights into the subjects and courses that shaped my analytical capabilities and technical expertise."
    url: "/educational-background/"
    btn_class: "btn--primary"
    btn_label: "Read More"

  - image_path: /assets/images/blog.webp
    alt: "Blog"
    title: "Blog"
    excerpt: "Discover a wealth of guides and posts on a wide range of technical topics. Explore in-depth insights and practical advice to expand your knowledge and skills in various domains."
    url: "/posts/"
    btn_class: "btn--primary"
    btn_label: "Read More"
---
<div style="font-size: 1em; letter-spacing: 0.05em; text-align: center; font-style: italic;">
  {% for intro in site.data.intros %}
    {% if intro.id == 'welcome' %}
      {% include intro_section.html excerpt=intro.excerpt %}
    {% endif %}
  {% endfor %}
</div>
---
<div style="font-size: 1em; letter-spacing: 0.05em; text-align: right;">
  {% for intro in site.data.intros %}
    {% if intro.id == 'first_break' %}
      {% include intro_section.html excerpt=intro.excerpt %}
    {% endif %}
  {% endfor %}
</div>
---
<div class="feature__wrapper">
  <h2 style="font-size: 1.5em; letter-spacing: 0.05em; text-align: center;">Portfolio</h2>
  {% include feature_row.html id="feature_row_portfolio" type="left" %}
</div>

<h2 style="font-size: 1.5em; letter-spacing: 0.05em; text-align: center;">Professional Profile</h2>
<div class="feature__wrapper">
  {% include feature_row.html id="features_extras_about_me"%}
</div>

<div class="feature__wrapper" style="font-size: 1em; letter-spacing: 0.05em; text-align: center;">
  {% for intro in site.data.intros %}
    {% if intro.id == 'second_break' %}
      {% include intro_section.html excerpt=intro.excerpt %}
    {% endif %}
  {% endfor %}
</div>

<h2 style="font-size: 1m; letter-spacing: 0.05em; text-align: center;">Learning Profile</h2>
<div class="feature__wrapper">
  {% include feature_row.html id="features_extras_learning"%}
</div>

{% include paginator.html %}