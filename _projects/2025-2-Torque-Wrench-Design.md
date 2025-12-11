---
layout: project
title: Torque Wrench Design
description: Design and simulation of a torque wrench
technologies: [Fusion 360, MATLAB, Ansys Mechanical]
image: /assets/images/spaceship-design.jpg
---

As the culmination of my mechanicals of material class in my junior year, we were asked to take an existing design for a torque wrench and redesign it. The requirements were that under a lod of 600 in-lbf:
•	achieve a safety factor of Xo = 4 for yield or brittle failure 
•	achieve a safety factor of XK = 2 for crack growth from an assumed crack of depth 0.04 inches (1 mm).
•	achieve a safety factor of XS = 1.5 for fatigue stress
•	attain at least 1.0 mV/V strain gauge output at the rated torque of 600 in-lbf
•	material must be a steel, aluminum or titanium alloy.

To begin, I wrote a MATLAB script to quickly iterate over designs to achieve the design specifications. I wrote it to have two outputs, one section to check the math against a given baseline, and another to determine wheter my redesign met the design specifications. The key change I made from the baseline was defining new geometry for the handle. The baseline had a very simple rectangular handle that would not be very ergonomic to use, so I quickly decided to go with a circular handle.

![Baseline Design]({{ "/assets/images/Torque Wrench/Baseline_Design.jpg" | relative_url }}){: .inline-image-l}

With that key design decision made, I moved forward with selecting a material for the wrench. Throughout the process I found that the safety factors specifications were easily met with any sane materials and dimensions, but the strain gauge sensetivity was more challenging. The key variable that determined the strain gauge sensetivity was the Young's Modulus of the material, a higher young's modulus would increase the strain on the handle, but it generally came with poor strength. Initially the materials I tried were either too stiff for the strain gauge or too brittle to meet the safety factor against crack growth. In the end I landed on Aluminium 7075 for its high strength and elasticity. I also upgrade from a half wheatsone bridge strain gauge to a full bridge sensor. I chose the SGT-2/350-FB11 from DwyerOmega which has dimensions of 10mm length by 9mm width, small enough to fit on the handle. It would be attached near the wrench head to maximize the strain it would measure. With these choices I was able to attain ~1.2mV/V in the script, and felt confident in moving forward with the modelling. 

```python
    Put script here = 10;
```

Here is the final CAD model of the redesigned torque wrench, along with a drawing showing all the key dimensions.

![Torque Wrench CAD]({{ "/assets/images/Torque Wrench/Torque_Wrench_CAD.png" | relative_url }}){: .center-image}
![Torque Wrench Drawing]({{ "/assets/images/Torque Wrench/Torque_Wrench_Drawing.png" | relative_url }}){: .center-image}

With the design done, I moved forward with verification using Ansys Mechanical. To simulate the wrench under load, I set a zero displacement boundary condition on all faces of the torque wrench. This would ensure that the head would remain stationary like it would in a real life scenario, but would create issues later on. I applied a 25 lbf load on the end of the handle to comply with the 600 in-lbf loading requirement. 

![Torque Wrench Loading]({{ "/assets/images/Torque Wrench/Redesigned_Loading.png" | relative_url }}){: .center-image}

Here are the results of the simulation. 

The deflection at the load point was 0.34 inches, which is larger than the deflection predicted by my MATLAB script of 0.23 inches. This can likely be attributed to the approximations made for the MATLAB script, where the geometry may have been oversimplified.

![Torque Wrench Deflection]({{ "/assets/images/Torque Wrench/Redesigned_Deflection.png" | relative_url }}){: .center-image}

I faced significant problems with the maximum principle stress. The loading and boundary conditions resulted in a significant stress concentration at the edges of the head. This somewhat makes sense as this is where the area is the smallest. For a future redesign, I would resolve this issue by chaning the material of the head to one with higher strength. 

![Torque Wrench Deflection]({{ "/assets/images/Torque Wrench/Redesigned_Stress.png" | relative_url }}){: .center-image}
![Torque Wrench Deflection]({{ "/assets/images/Torque Wrench/Redesigned_Stress_Concentration.png" | relative_url }}){: .center-image}

The results for strain likewise faced the same issue, where it became concentrated around the edge of the wrench head. However the simulated and predicted values for the normal strain in the direction of the strain gauge were close, 600 vs 521 microstrain. This gives a final strain gauge of sensitivity of 1.04mV/V, just barely in spec.

![Torque Wrench Deflection]({{ "/assets/images/Torque Wrench/Redesigned_Strain.png" | relative_url }}){: .center-image}
![Torque Wrench Deflection]({{ "/assets/images/Torque Wrench/Redesigned_Strain_normal.png" | relative_url }}){: .center-image}

Overall I am happy with my design decisions, and as someone who is not deeply familiar with FEA, this definately re-ignited my interest in the field.


