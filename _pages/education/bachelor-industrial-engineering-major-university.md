---
layout: single  
author_profile: true 
permalink: /education/bachelor-industrial-engineering-major-university/
years:
  semester_0:
    summary: "Semester 0 Courses"
    items:
      - title: "Computational Thinking"
        description: "Introduces fundamentals of computational thinking, emphasizing problem-solving with algorithmic thinking, pattern recognition, and abstraction for computational solutions."
      - title: "Statistics"
        description: "Focuses on collecting, analyzing, interpreting, and presenting data, with emphasis on statistical measures and hypothesis testing."
      - title: "Linear Algebra"
        description: "Explores vector spaces, linear mappings, and systems of linear equations, including matrix operations and eigenvalues."
      - title: "Mechanics"
        description: "Studies motion under force influence, covering kinematics, dynamics, statics, and engineering applications."
      - title: "Integral Calculus"
        description: "Covers function integration techniques, applications of the definite integral, and improper integrals."
  semester_1:
    summary: "Semester 1 Courses"
    items:
      - title: "Bootcamp Workshop in Industrial Engineering"
        description: "An intensive introduction to industrial engineering's fundamentals, focusing on systems thinking and problem-solving."
      - title: "Inference"
        description: "Examines techniques for drawing conclusions from sample data, including confidence intervals and hypothesis testing."
      - title: "Differential Equations"
        description: "Studies ordinary and partial differential equations solving methods and their applications."
      - title: "Electromagnetism"
        description: "Explores electric and magnetic field interactions, covering electrostatics, magnetostatics, electrodynamics, and electromagnetic waves."
      - title: "Multivariable Calculus"
        description: "Studies calculus in multiple dimensions, focusing on partial derivatives, multiple integrals, and vector calculus."
      - title: "Applied Programming in Data Analysis"
        description: "Teaches practical coding for data analysis, using common data science tools and languages."
  semester_2:
    summary: "Semester 2 Courses"
    items:
      - title: "Operations Research"
        description: "Studies decision-making analytical methods, including optimization and simulation."
      - title: "Fluid Mechanics"
        description: "Covers fluid behavior fundamentals and engineering applications, including fluid dynamics and statics."
      - title: "Operations Management"
        description: "Focuses on managing organizational operations for productivity and efficiency."
      - title: "Numerical Analysis"
        description: "Studies mathematical problem-solving algorithms, including error analysis and numerical integration."
      - title: "Microeconomics"
        description: "Analyzes individual and firm behavior in resource allocation and decision-making."
  semester_3:
    summary: "Semester 3 Courses"
    items:
      - title: "Optimization Methods"
        description: "Examines processes and systems optimization techniques, including linear and nonlinear programming."
      - title: "Applied Thermal Systems"
        description: "Studies heating, ventilating, and air conditioning (HVAC) systems design and analysis."
      - title: "Process Simulation"
        description: "Uses software to simulate process operation for optimization."
      - title: "Econometrics"
        description: "Applies statistical methods to economic data for hypothesis testing and forecasting."
      - title: "Macroeconomics"
        description: "Examines the overall economy, including inflation, unemployment, and economic policies."
  semester_4:
    summary: "Semester 4 Courses"
    items:
      - title: "Technological Innovation Project"
        description: "Focuses on developing innovative technological solutions."
      - title: "Heat Transfer and Procurement"
        description: "Explores heat transfer in procurement and supply chain management."
      - title: "Database Processing and Analysis"
        description: "Teaches managing, processing, and analyzing large data sets."
      - title: "Accounting and Management Control"
        description: "Covers accounting principles and financial resource management."
      - title: "Human Resource Management"
        description: "Focuses on managing employees, including recruitment, training, and performance management."
  semester_5:
    summary: "Semester 5 Courses"
    items:
      - title: "Entrepreneurship and Technology Transfer"
        description: "Explores transforming innovations into marketable products and managing new ventures."
      - title: "Agile Methodologies for Process Improvement and Optimization"
        description: "Applies agile practices to improve and optimize business processes."
      - title: "Strategic Marketing"
        description: "Teaches planning and executing marketing strategies for competitive advantage."
      - title: "Financial Engineering"
        description: "Applies mathematical methods to finance problems, including risk management and investment strategies."
      - title: "Machine Learning"
        description: "Introduces machine learning concepts and algorithms for data-driven decision-making."
      - title: "Supply Chain Management and Procurement"
        description: "Focuses on efficient resource acquisition and supply chain optimization,

 including supplier selection and inventory control."
  semester_6:
    summary: "Semester 6 Courses"
    items:
      - title: "Capstone Project: Final"
        description: "Integrates knowledge and skills acquired through a comprehensive project."
      - title: "Business Simulation Games"
        description: "Uses interactive games to simulate business environments and scenarios."
      - title: "Economic and Financial Project Evaluation"
        description: "Assesses project viability using cost-benefit analysis and investment appraisal."
      - title: "Renewable Energies and Energy Efficiency"
        description: "Delves into renewable energy technologies and practices for optimizing energy use."
      - title: "Project Management"
        description: "Introduces project management principles, including planning, executing, and closing projects successfully."
---
{% include logo_and_text.html logo='/assets/images/education/um-logo.jpg' line1='Bacherlor’s Degree in Industrial Engineering' line2='Major University of Chile' url='https://www.umayor.cl/um/carreras/ingenieria-civil-industrial-santiago' %}

---

<div class="degree-profile"> 

  <p>Industrial Engineering is a dynamic field of engineering deeply rooted in Chilean higher education. This multidisciplinary program blends engineering principles, business optimization, and advanced analytical techniques. Graduates emerge equipped to drive efficiency, maximize resource utilization, and implement data-driven strategies across diverse industries.</p>

  <h4>Key Skills:</h4>
  <ul>
    <li>Optimization Modeling: Design and implementation of mathematical models (linear/non-linear programming, queuing theory, simulation) for resource allocation, system design, and decision-making.</li>
    <li>Supply Chain Analytics: Demand forecasting, inventory optimization, network design, and risk assessment within complex supply chains.</li>
    <li>Data-Driven Insights: Statistical modeling, predictive analytics, and machine learning techniques for transforming data into actionable business solutions.</li>
    <li>Strategic Problem-solving: Applying critical thinking and optimization methods to identify and improve inefficiencies within organizations.</li>
    <li>Project Management: Incorporating risk analysis and optimization concepts into project management.</li> 
  </ul>

  <h4>Tools & Technologies:</h4>
  <ul>
    <li>Python</li>
    <li>R</li>
    <li>Excel</li> 
  </ul> 

  <h4>Industry Applications:</h4> 
  <ul>
    <li>Management Consulting</li>
    <li>Technology and Data Science</li> 
    <li>Manufacturing and Logistics</li> 
    <li>Finance and Risk Management</li> 
    <li>Research and Academia</li>  
  </ul>

  <p>My experience demonstrates a strong ability to translate operations research methodologies into tangible business improvements.</p> 

</div> 

## Course Work
Here's the complete engineering degree program with course descriptions, distributed across all semesters:

{% for year in page.years %}
    {% assign data = year[1] %}
    {% include education_course_work.html summary=data.summary items=data.items %}
{% endfor %}

