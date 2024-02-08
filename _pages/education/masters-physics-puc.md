---
layout: single  
author_profile: true 
permalink: /education/masters-physics-puc/
years:
  first_year:
    summary: "First Year Courses"
    items:
      - title: "Classical Mechanics"
        description: "This course delves into the fundamental principles of classical mechanics, covering topics such as Newton's laws, conservation laws, systems of particles, rigid body dynamics, and oscillatory motion. It emphasizes mathematical modeling and problem-solving in physical scenarios, requiring students to apply theoretical concepts to practical situations."
      - title: "Classical Optics"
        description: "An advanced study of the behavior of light, including wave optics, ray optics, and optical instruments. The course covers phenomena such as interference, diffraction, polarization, and the propagation of light through various media. Students will explore both the theoretical frameworks and experimental techniques used in classical optics."
      - title: "Advanced Laboratory Practice"
        description: "This course is designed to enhance students' experimental skills and understanding of physical phenomena through advanced laboratory experiments. It covers a wide range of topics and techniques, requiring students to design experiments, collect and analyze data, and interpret results in the context of theoretical physics."
      - title: "Atomic and Molecular Physics"
        description: "This course covers the structure, properties, and behavior of atoms and molecules. Topics include quantum mechanics applied to atomic and molecular systems, spectroscopy, and the interaction of electromagnetic radiation with matter. Students will explore both the theoretical underpinnings and experimental evidence of atomic and molecular physics."
  second_year:
    summary: "Second Year Courses"
    items:
      - title: "Master's Thesis I and II"
        description: "These courses represent the culmination of the Master's program, where students are required to conduct original research under the supervision of a faculty advisor. The thesis project is expected to contribute new knowledge to the field of experimental physics, demonstrating the student's ability to perform independent research, analyze data, and communicate findings effectively."
---
<style>
  li, p {
    text-align: justify;
  }
</style>

<style>
  li, p {
    text-align: justify;
  }
</style>

{% include logo_and_text.html logo='/assets/images/education/universidad-catolica-de-chile-logo2.png' line1='Master’s in Physics' line2='The Pontifical Catholic University of Chile' url='https://en.wikipedia.org/wiki/Pontifical_Catholic_University_of_Chile' %}

### Program Description: 
<p>
My Master's degree in Physics, specializing in Material Sciences, represented a rigorous exploration into the forefront of experimental techniques and the nuanced interplay of technology and research. The program was a crucible for refining my expertise, particularly in the realm of Chemical Vapor Deposition (CVD), a sophisticated process at the heart of crafting groundbreaking materials with applications spanning electronics, aerospace, and beyond.

In this advanced study, my engagement with experimental physics was not merely academic; it was a deep dive into the mechanics of data acquisition, signal processing, and the mastery of advanced instrumentation. This experience was underpinned by a comprehensive application of data analysis and modeling techniques, through which I wielded statistical methods and machine learning algorithms to distill clarity from the complexity of experimental data.

A cornerstone of my work centered on the development of interactive dashboards, serving as a bridge between raw data and actionable insights. This endeavor not only honed my software development skills but also refined my ability to automate processes, thereby streamlining the analysis and presentation of data.

Central to my specialization was a focus on the research and development of material surfaces, a domain where the intersection of physical principles and technological innovation reveals potent applications. My pursuit here was not just academic curiosity but a commitment to pushing the boundaries of material science, leveraging the power of machine learning and model optimization to forecast and enhance material properties with unprecedented precision.

My path through material sciences has been marked by an unwavering dedication to discovery and the creative application of insights gained. This experience serves as a clear indicator of my profound comprehension and wide-ranging abilities, showcasing my potential to make significant contributions and drive innovation within the tech industry.

</p>
### Course Work: 
{% for year in page.years %}
    {% assign data = year[1] %}
    {% include education_course_work.html summary=data.summary items=data.items %}
{% endfor %}