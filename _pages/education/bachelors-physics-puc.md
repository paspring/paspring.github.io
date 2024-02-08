---
layout: single  
permalink: /education/bachelors-physics-puc/
author_profile: true 
years:
  first_year:
    summary: "First Semester Courses"
    items:
      - title: "Introduction to Physics"
        description: "An exploration of fundamental physical concepts, including motion, energy, and matter, setting the stage for a deeper understanding of the subject."
      - title: "Physics Lab I"
        description: "Hands-on experiments complementing theoretical knowledge, focusing on measurement techniques and data analysis."
      - title: "Calculus I"
        description: "Introduction to differential and integral calculus, emphasizing its application in physical sciences."
      - title: "Algebra and Geometry"
        description: "Study of algebraic structures, vector spaces, and geometric transformations, laying the mathematical groundwork for physics."
  second_year:
    summary: "Second Semester Courses"
    items:
      - title: "Classical Mechanics I"
        description: "Examination of Newtonian mechanics, covering laws of motion and the principles of force, mass, and acceleration."
      - title: "Physics Lab II"
        description: "Further experimentation in mechanics, fostering practical understanding and proficiency in laboratory practices."
      - title: "Calculus II"
        description: "Continuation of Calculus I, introducing multivariable calculus and its use in physics."
      - title: "Linear Algebra"
        description: "Concepts of linear equations, matrices, and determinants, essential for quantum mechanics and other areas of physics."
  third_year:
    summary: "Third Semester Courses"
    items:
      - title: "Thermodynamics and Kinetic Theory"
        description: "Study of energy transfer, thermodynamic processes, and the behavior of gases."
      - title: "Calculus III"
        description: "Advanced calculus topics, focusing on vector calculus and its applications in physics."
      - title: "Introduction to Programming"
        description: "Basics of computer programming, equipping students with computational tools for problem-solving in physics."
      - title: "Differential Equations"
        description: "A crucial study of the mathematical equations that describe the behavior of dynamic systems. This course covers methods for solving first and higher-order differential equations, applications to physical systems, and introduces the Laplace transform."
  fourth_year:
    summary: "Fourth Semester Courses"
    items:
      - title: "Classical Mechanics II"
        description: "Advanced topics in mechanics, including rotational dynamics and conservation laws."
      - title: "Electromagnetism"
        description: "In-depth study of electric and magnetic fields and their interactions with matter."
      - title: "Physics Lab III"
        description: "Experiments in optics and electromagnetism, reinforcing theoretical knowledge with practical application."
      - title: "Methods of Physics I"
        description: "Introduction to the analytical and computational methods used in physics research."
      - title: "Advanced Programming"
        description: "A course focused on developing high-level programming skills used in simulations and analysis of physical systems, covering algorithm optimization and object-oriented programming."
  fifth_year:
    summary: "Fifth Semester Courses"
    items:
      - title: "Waves and Optics"
        description: "Exploration of wave phenomena, optical instruments, and the nature of light."
      - title: "Modern Physics"
        description: "Covers the groundbreaking developments of the 20th century, including special relativity and the early concepts of quantum theory."
      - title: "Physics Lab IV"
        description: "Advanced laboratory experiments, often related to modern physics topics."
      - title: "Methods of Physics II"
        description: "Further study of research methodologies, including statistical analysis and advanced computational techniques."
      - title: "Statistics and Probability"
        description: "An exploration of statistical methods and probability theory essential for analyzing data and modeling uncertainty in physical phenomena."
  sixth_year:
    summary: "Sixth Semester Courses"
    items:
      - title: "Quantum Physics I"
        description: "Introduction to the principles of quantum mechanics, wave functions, and quantum states."
      - title: "Physics Lab V"
        description: "Experiments related to quantum and modern physics, enhancing theoretical understanding with hands-on experience."
      - title: "Electromagnetic Theory"
        description: "A fundamental study of electric and magnetic fields and their interactions, covering Maxwell's equations, wave propagation, and applications in various technologies."
      - title: "Machine Learning for Physicists"
        description: "Introduces machine learning algorithms and their application in data analysis and modeling physical systems, with an emphasis on mathematical understanding and practical utilization."
  seventh_year:
    summary: "Seventh Semester Courses"
    items:
      - title: "Quantum Physics II"
        description: "Continued exploration of quantum mechanics, including more advanced topics and applications."
      - title: "Physics Lab VI"
        description: "Advanced quantum experiments, often involving sophisticated techniques and equipment."
      - title: "Electronics and Microcontrollers I"
        description: "Introduction to electronics principles with a focus on microcontrollers, covering electronic components, circuits, and programming for various devices."
      - title: "Laser and Non-Linear Optics"
        description: "Detailed study of laser physics and non-linear optical systems, including laser operation, applications, and fundamental principles."
  eighth_year:
    summary: "Eighth Semester Courses"
    items:
      - title: "Statistical Mechanics"
        description: "Understanding the statistical nature of physical systems and the laws governing them."
      - title: "Advanced Physics Lab"
        description: "A capstone laboratory course, involving complex experiments and independent projects."
      - title: "Material Science"
        description: "An exploration of material properties and applications, covering material structure, electronic properties, and mechanical behavior."
      - title: "Electronics and Microcontrollers II"
        description: "Continuation of Electronics and Microcontrollers I, covering advanced circuit design and microcontroller functionality, often involving interactive device projects."
  ninth_year:
    summary: "Ninth Semester Courses"
    items:
      - title: "Research Methods"
        description: "A course designed to equip students with the tools necessary for conducting scientific research, covering hypothesis formulation, data collection, analysis, and communication."
      - title: "Research Thesis"
        description: "An intensive research project requiring students to investigate a novel question or problem in physics, resulting in a formal thesis."
---
<style>
  li, p {
    text-align: justify;
  }
</style>

{% include logo_and_text.html logo='/assets/images/education/universidad-catolica-de-chile-logo2.png' line1='Bachelor’s Degree in Physics' line2='The Pontifical Catholic University of Chile' url='https://en.wikipedia.org/wiki/Pontifical_Catholic_University_of_Chile' %}


### Program Description:

<p>
My Bachelor's degree in Physics provided a comprehensive education grounded in the principles of physics and its applications, laying a solid foundation for a career in a wide range of industries, especially technology. Through this program, I developed a robust analytical mindset, critical for solving complex problems and innovating solutions. My studies covered key physics areas, including mechanics, electromagnetism, quantum mechanics, and thermal physics, which are essential for understanding the underlying principles of technological advancements.

I honed practical skills highly valued in the industry, such as data analysis, precision measurement, and computational modeling, through rigorous coursework and hands-on laboratory experiences. These skills are directly applicable to roles in research and development, quality assurance, and product design, enabling me to contribute effectively to interdisciplinary teams and projects.

In addition to a broad physics education, I specialized in condensed matter physics with an emphasis on material science. This focus allowed me to explore the cutting-edge of materials technology, including the development and characterization of novel materials with applications in electronics, energy, and nanotechnology. This specialty complements my general physics background, equipping me with unique insights and capabilities to drive innovation in the technological sector.
</p>

---

### Course Work
{% for year in page.years %}
    {% assign data = year[1] %}
    {% include education_course_work.html summary=data.summary items=data.items %}
{% endfor %}