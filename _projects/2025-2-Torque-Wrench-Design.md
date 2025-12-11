---
layout: project
title: Torque Wrench Design
description: Design and simulation of a torque wrench
technologies: [Fusion 360, MATLAB, Ansys Mechanical]
image: /assets/images/Redesigned_Deflection.png
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
![Torque Wrench Deflection]({{ "/assets/images/Torque Wrench/Redesigned_Strain_Normal.png" | relative_url }}){: .center-image}

Overall I am happy with my design decisions, and as someone who is not deeply familiar with FEA, this definately re-ignited my interest in the field.

```python
    %% Inputs
    % Baseline Dimensional Inputs
    M = 600;        % Max torque (in-lbf) 600 base
    L = 16;         % Length from head to drive point (in) 16 base
    h = 0.75;       % Width of handle (in) 0.75 base
    b = 0.5;        % Thickness of handle (in) 0.5 base
    c = 1;          % C-C Distance from head to strain gauge (in) 1 base

    % Material Properties
    name = 'M42 Steel';  % Material Name
    type = 'St';     % Material type (Al or St)
    E = 32e6;           % Young's Modulus (psi)
    nu = 0.29;          % Poisson's ratio
    sigma_u = 370e3;    % Ultimate yield strength (psi)
    K_IC = 15e3;        % Fracture toughness (psi*in^0.5)
    N_f = 10e6;         % Required service life
    sigma_f = 115e3;    % Fatigue strength for 10e6 cycles (psi)

    % Performance Metrics
    X_o_min = 4;        % Minimum safety factor for yielding or brittle failure
    X_K_min = 2;        % Minimum safety factor for crack growth
    X_s_min = 1.5;      % Minimum safety factor for fatigue stress
    mVV_min = 1;        % Minimum strain gauge output for 600 in-lbf torque
    a = 0.04;           % Assumed crack depth (in)

    % Tracking metrics
    yield_pass = 0;
    fracture_pass = 0;
    fatigue_pass = 0;
    sense_pass = 0;

    %% Calculate expressions
    % Moment of Intertia
    I_rect = b * h^3 / 12;

    % Applied Force
    P = M/L;

    %% Calculate Stress and Deflection Metrics
    % Calculate deflection
    delt = (P*L^3) / (3*E*I_rect);

    %Calculate normal stress
    sigma = (M*h) / (2*I_rect);

    %Print results
    fprintf('Max Deflection = %.4f in\n', delt);
    fprintf('Max Normal Stress = %.4f psi\n', sigma);

    %% Calculate Baseline Safety Factor Metrics
    % FOS for yielding
    X_o = sigma_u/sigma;
    fprintf('Strength FOS = %.4f\n', X_o);
    if X_o > X_o_min
        fprintf('Safety factor for yield passed :) \n')
    else
        fprintf('Safety factor for yield failed :( \n')
    end

    % FOS for crack growth
    K_I = 1.12 * sigma * ((pi*a)^0.5);
    X_K = K_IC/K_I;
    fprintf('Fracture FOS = %.4f\n', X_K);
    if X_K > X_K_min
        fprintf('Safety factor for fracture passed :) \n')
    else
        fprintf('Safety factor for fracture failed :( \n')
    end

    % FOS for fatigue stress
    X_s = sigma_f / sigma;
    fprintf('Fatigue FOS = %.4f\n', X_s);
    if X_s > X_s_min
        fprintf('Safety factor for fatigue passed :) \n')
    else
        fprintf('Safety factor for fatigue failed :( \n')
    end

    %% Calculate Baseline strain gauge output
    % Strain at gauge
    eps_base = ((L-c)*P*(h/2)) / (E*I_rect);
    fprintf('Strain at gauge = %.4f\n', eps_base)

    % Base strain gauge output (half bridge)
    mVV_out = eps_base*1000;
    fprintf('Base strain gauge output = %.4f\n', mVV_out)
    if mVV_out > mVV_min
        fprintf('Sensitivity spec met with half bridge :) \n')
    else
        fprintf('Sensitivity spec not met with half bridge :( \n')
    end

    % Better strain gauge output (full bridge)
    mVV_out = eps_base*2000;
    fprintf('Base strain gauge output = %.4f\n', mVV_out)
    if mVV_out > mVV_min
        fprintf('Sensitivity spec met with full bridge :) \n')
    else
        fprintf('Sensitivity spec not met with full bridge :( \n')
    end

    %% Optimized Hand Calcs
    fprintf('\n')

    % Custom Dimensional Inputs
    M_c = 600;
    L_c = 24;
    r = 0.5;        % Radius of handle (in)
    d = 2;        % C-C Distance from head to strain gauge (in)

    % Custom Material Properties
    name2 = 'Aluminium 7075';  % Material Name
    type2 = 'Al';     % Material type (Al or St)
    E_circ = 10e6;           % Young's Modulus (psi)
    nu_circ = 0.325;          % Poisson's ratio
    sigma_u_circ = 66.7e3;    % Ultimate yield strength (psi)
    K_IC_circ = 24.2e3;        % Fracture toughness (psi*in^0.5)
    sigma_f_circ = 24.9e3;    % Fatigue strength for 10e6 cycles (psi)
    N_f = 10e6;              % Required service life

    P_c = M_c/L_c;
    I_circ = pi * r^4 / 4;
    delt_circ = (P*L_c^3) / (3*E_circ*I_circ);
    sigma_circ = (32*M_c) / (pi*(2*r)^3);

    % FOS for yielding
    X_o_circ = sigma_u_circ/sigma_circ;
    fprintf('Circular Strength FOS = %.4f\n', X_o_circ);
    if X_o_circ > X_o_min
        yield_pass = 1;
        fprintf('Pass \n')
    end

    % FOS for fatigue stress
    X_s_circ = sigma_f_circ / sigma_circ;
    fprintf('Circular Fatigue FOS = %.4f\n', X_s_circ);
    if X_s_circ > X_s_min
        fatigue_pass = 1;
        fprintf('Pass \n')
    end

    % FOS for crack growth
    K_I_circ = 1.12 * sigma_circ * ((pi*a)^0.5);
    X_K_circ = K_IC_circ/K_I_circ;
    fprintf('Circular Fracture FOS = %.4f\n', X_K_circ);
    if X_K_circ > X_K_min
        fracture_pass = 1;
        fprintf('Pass \n')
    end

    % Strain
    eps_circ = ((L_c-d)*P_c*r) / (E_circ*I_circ);
    fprintf('Strain at gauge = %.4f\n', eps_circ)


    % Better strain gauge output (full bridge)
    mVV_out_circ = eps_circ*2000;
    fprintf('Full bridge output = %.4f\n', mVV_out_circ)
    if mVV_out_circ > mVV_min
        sense_pass = 1;
    end

    % Deflection
    delt_circ = (P_c*L_c^3) / (3*E_circ*I_circ);
    fprintf('Deflection = %.4f\n', delt_circ)

    %% Final Output
    if (yield_pass + fracture_pass + fatigue_pass + sense_pass == 4)
        fprintf('\n')
        fprintf('All specifications met \n')
        fprintf('Final dimensions: \n')
        fprintf('Handle length: %.2f in\n', L_c)
        fprintf('Handle radius: %.2f in\n', r)
        fprintf('Material: %s\n', name2)
    else
        fprintf('Specs not met \n')
    end
```