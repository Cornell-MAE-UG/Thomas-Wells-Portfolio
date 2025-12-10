---
layout: project
title: Model of afib ECG Waveform Using Matlab
description: Modeling an ECG Using Matlab
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


![Single Heartbeat]({{ "/assets/images/ECG Model/Frequency_Response.png" | relative_url }}){: .center-image}

```python
    % Model for ECG Generation based on Transfer Functions
    % Based on Equations (12) and (13) from Omar et al. doi:10.3390/biomimetics9050300

    clear; close all; clc;


    %% Parameters from Table 2 (Normal heartbeat)
    % P-wave (i=1)
    r1 = 0.0794;
    a1 = 0;
    b1 = 40.92;
    c1 = 120.22;
    d1 = 0.85e3;
    k1 = 50.11;

    % QRS complex (i=2)
    r2 = 0.192;
    a2 = -0.465;
    b2 = 349.96;
    c2 = 223.35;
    d2 = 48.3e3;
    k2 = 94.81;

    % T-wave (i=3)
    r3 = 0.28;
    a3 = 0;
    b3 = 35.12;
    c3 = 38.90;
    d3 = 2.28e3;
    k3 = 57.57;

    %% Transfer functions (Equation 12 in Omar et al.)
    s = tf('s');

    % Transfer function for P-wave
    TF1 = k1 * exp(-r1*s) * (a1*s - b1) / (s^2 + c1*s + d1);

    % Transfer function for QRS complex
    TF2 = k2 * exp(-r2*s) * (a2*s - b2) / (s^2 + c2*s + d2);

    % Transfer function for T-wave
    TF3 = k3 * exp(-r3*s) * (a3*s - b3) / (s^2 + c3*s + d3);

    % Combined heartbeat model
    HB = TF1 + TF2 + TF3;   % For normal heartbearts
    HB_afib = TF2 + TF3;    % For afib 


    %% Create periodic ECG model (Equation 13)
    % For periodic signal, we'll simulate with impulse train input
    % The periodic impulse is represented by: 1/(1-exp(-s/f))

    % Time parameters
    t_end = 0.6;        % Time for single heartbeat (purely for formatting)
    t_end_ECG = 5;      % Time for periodic ECG (Full duration of graph)
    t_single = 0:0.001:t_end;  
    t_periodic = 0:0.001:t_end_ECG;   


    %% Simulate single heartbeat with single impulse
    impulse_strength = -8;      % Vary this value to adjust response amplitude
    impulse_input = zeros(size(t_single));
    impulse_input(1) = impulse_strength;

    % Graph single heartbeat
    figure('Position', [50 50 1200 800]);
    subplot(4,1,1);
    y_single = lsim(HB, impulse_input, t_single);
    plot(t_single, y_single, 'b', 'LineWidth', 1.5);
    xlabel('Time (s)');
    ylabel('Amplitude (mV)');
    title('Single Heartbeat');
    grid on;
    xlim([0 t_end]);


    %% Plot individual components
    subplot(4,1,2);
    hold on;

    % Individual components for first heartbeat
    y1 = lsim(TF1, impulse_input, t_single);
    y2 = lsim(TF2, impulse_input, t_single);
    y3 = lsim(TF3, impulse_input, t_single);

    plot(t_single, y1, 'g--', 'LineWidth', 1.2, 'DisplayName', 'P-wave (TF1)');
    plot(t_single, y2, 'm--', 'LineWidth', 1.2, 'DisplayName', 'QRS complex (TF2)');
    plot(t_single, y3, 'c--', 'LineWidth', 1.2, 'DisplayName', 'T-wave (TF3)');
    plot(t_single, y_single, 'k', 'LineWidth', 2, 'DisplayName', 'Combined ECG');

    xlabel('Time (s)');
    ylabel('Amplitude (mV)');
    title('Individual Transfer Function Components');
    legend('Location', 'best');
    grid on;
    xlim([0 t_end]);
    hold off;


    %% Simulate periodic ECG with cyclic impulse signal
    f = 1.5;        % Heart rate frequency (Hz)
    period = 1/f;   % Base period (s)

    impulse_train = zeros(size(t_periodic));
    impulse_times = [];
    current_time = 0;

    while current_time < max(t_periodic)
        impulse_times = [impulse_times, current_time];
        current_time = current_time + period;
    end

    for i = 1:length(impulse_times)
        [~, idx] = min(abs(t_periodic - impulse_times(i)));
        if idx <= length(impulse_train)
            impulse_train(idx) = impulse_strength;
        end
    end

    % Graph periodic ECG
    subplot(4,1,3);
    y_periodic = lsim(HB, impulse_train, t_periodic);
    plot(t_periodic, y_periodic, 'r', 'LineWidth', 1.5);
    xlabel('Time (s)');
    ylabel('Amplitude (mV)');
    title(sprintf('Periodic ECG - %d bpm', round(f*60)));
    grid on;


    %% Simulate afib
    % Create periodic impulse train with random offsets
    f = 1.5;        % Heart rate frequency (Hz)
    period = 1/f;   % Base period (s)
    variability = 0.5;  % Variability in heart rate

    impulse_train = zeros(size(t_periodic));
    impulse_times = [];
    current_time = 0;

    while current_time < max(t_periodic)
        impulse_times = [impulse_times, current_time];
        random_offset = period * variability * randn();
        current_time = current_time + period + random_offset;
    end

    for i = 1:length(impulse_times)
        [~, idx] = min(abs(t_periodic - impulse_times(i)));
        if idx <= length(impulse_train)
            impulse_train(idx) = impulse_strength;
        end
    end

    % Graph afib ECG
    subplot(4,1,4);
    y_periodic = lsim(HB_afib, impulse_train, t_periodic);
    plot(t_periodic, y_periodic, 'r', 'LineWidth', 1.5);
    xlabel('Time (s)');
    ylabel('Amplitude (mV)');
    title(sprintf('Afib - %d bpm', round(f*60)));
    grid on;

    %% Additional analysis - Frequency response
    figure('Position', [150 150 1200 800]);
    bode(HB);
    title('ECG Frequency Response')
    grid on
```