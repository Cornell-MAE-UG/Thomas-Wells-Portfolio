---
layout: project
title: ECG Model
description: Just a spaceship that I designed
technologies: [MATLAB]
image: /assets/images/ECG Model/ECG_Thumbnail.png
---

In my Junior Fall, I took a class on system dynamics. The course covered major topics such as open and close-looped systems, feedback control, laplace transforms, state space models, and PID control.

The class culminaed in a final group project, where we were tasked with choosing a real-life system to study and report on. My group initially chose to study the human heart, focusing on pacemakers. We hoped to create a model of a pacemaker driving a heart suffering a range of heat conditions, but we quickly realized that we may have bit off a bit more than we could chew, especially for a project that needed to be completed in just over a week.

We quickly pivoted to modeling the heart alone, focusing on trying to emulate a standard ECG signal. To do so we performed extensive research, and eventually landed on a couple academic papers that proposed models of varying complexity. We eventually landed on two models, the established Van der Pol model, and a simpler approximation set out by Omar et al. From here we chose to divide and conquer, and I decided to base my model on the one proposed by Omar et al. as it was much simpler compared to other proposed models. Omar et al. break down the ECG wave into three primary elements, a P-wave, a QRS complex, and a T-wave. In real life, each wave is produced by a different portion of the heart moving, but in the model each wave is described by the same transfer function, just with different parameters. To help me visualize the system, I created a block diagram of the system in google sheets. 

![ECG Block Diagram]({{ "/assets/images/ECG Model/ECG_Block_Diagram.png" | relative_url }}){: .center-image}

After doing so, it was relatively simple to write the given transfer functions and parameters in MATLAB. While some sample parameters were given, details on the input function were lacking, instead loosely described as a "cyclic impulse signal". I chose to interpret this as an impulse input that is sent periodically, and implemented it into the script. The first run on this model produced mixed results. The resulting waveform's amplitude was around 100mV, which is much larger than an ECG signal at around 0.6-0.8 mV. 

![Inverted Response]({{ "/assets/images/ECG Model/Inverted_Signal.png" | relative_url }}){: .center-image}

This problem was solved in two different ways. First by varying the strength of the impulse given to the system, and also by tuning the values for parameter "k" in the transfer functions, which alters the response gain. It was easier to set a new impulse strength as I only had to change on variable, but it may have been better to individually tune the gains for each transfer function for more control. Nonetheless the results were favorable, and I added another graph that showed the individual contributions of each transfer function.

![Single Heartbeat]({{ "/assets/images/ECG Model/Single_Heartbeat.png" | relative_url }}){: .center-image}

With this success, I expanded the single response to a rhymic heartbeat by implementing a repeating impulse signal. With this addition, I decided to model afib, a heart condition that I was diagnosed with as a child, but have since recovered from. The defining characteristics of afib are an irregular heartbeat, and the absence of a P-wave. To simulate the irregular heartbeat, a random delay was added to the cyclic impulse signal. Removing the P-wave was done by simply removing the transfer function that described the P-wave from the overall system. I was happy with the results.

![Single Heartbeat]({{ "/assets/images/ECG Model/Repeat_afib.png" | relative_url }}){: .center-image}

As another exercise and additional deliverable for the class project, I made bode plots of the normal system.

![Single Heartbeat]({{ "/assets/images/ECG Model/Frequency_Response.png" | relative_url }}){: .inline-image-l}

```MATLAB
    some code = 10;
    plot();
```