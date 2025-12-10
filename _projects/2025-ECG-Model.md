---
layout: project
title: ECG Model
description: Just a spaceship that I designed
technologies: [MATLAB]
image: /assets/images/ECG Model/ECG_Thumbnail.png
---

In my Junior Fall, I took a class on system dynamics. The course covered major topics such as open and close-looped systems, feedback control, laplace transforms, state space models, and PID control.

The class culminaed in a final group project, where we were tasked with choosing a real-life system to study and report on. My group initially chose to study the human heart, focusing on pacemakers. We hoped to create a model of a pacemaker driving a heart suffering a range of heat conditions, but we quickly realized that we may have bit off a bit more than we could chew, especially for a project that needed to be completed in just over a week.

We quickly pivoted to modeling the heart alone, focusing on trying to emulate a standard ECG signal. To do so we performed extensive research, and eventually landed on a couple academic papers that proposed models of varying complexity. We eventually landed on two models, the established Van der Pol model, and a simpler approximation set out by Omar et al. From here we chose to divide and conquer, and I decided to base my model on the one proposed by Omar et al. as most of the work for tuning the model parameters had already been done. It was relatively simple to write the given transfer functions and parameters in MATLAB. However what was not detailed however was the input function, which was loosely described as a "cyclic impulse signal". I chose to interpret this as an impulse input that is sent periodically, and implemented it into the script. The first run on this model produced mixed results. The resulting waveform's amplitude was around 100mV, which is much larger than an ECG signal at around 0.6-0.8 mV. 

![Inverted Response]({{ "/assets/images/ECG Model/Inverted_Signal.png" | relative_url }}){: .inline-image-l}

