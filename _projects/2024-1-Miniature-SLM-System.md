---
layout: project
title: Miniature Selective Laser Melting System
description: Design and simulation of a torque wrench
technologies: [Fusion 360, MATLAB, CNC Machining, 3D Printing]
image: /assets/images/SLM/Full Inside.jpg
---

I joined Moridi Research Group at the end of my freshman spring, I then spent the following summer and the entirety of my sophomore year working on building a minizatrized selective laser melting (SLM) additive manufacturing system. The goal was to create a platform that would enable multi-layer printing during in-situ synchrotron experiemnts. On this page I have replicated my technical report that I submitted.

<p><strong>Test Title </strong></p>
Metal additive manufacturing has revolutionized the field of manufacturing, processes such as selective laser melting (SLM) have enabled production of components that were previously challenging or impossible. The field has progressed rapidly, and perhaps too quickly where the underlying processes are not fully understood. This failure can be largely attributed to an inability to conduct experiments amid the production process as specialized systems to do so either do not exist or are exceedingly hard to come by.

This project seeks to create a miniaturized SLM system that enables multi-layer printing during in-situ synchrotron experiments. The main body of the system must fit within a sealed argon chamber and the print area must be transparent to x-rays along the main axis. The system should be capable of automatically printing at least 5 consistent layers of 50-micron height with minimal downtime. Although not strictly necessary it is a personal goal to make the system easy and intuitive to set up and operate.

The substrate (Fig 1 (1)) very closely resembles the form factor that has been used in the lab in the past with a could design changes. The overall length of the substrate has been reduced from 50mm to 40mm. This is a significant decrease in length but is necessary to ensure that the glassy carbon clamps (Fig 4 (8)) do not exert any force on the substrate. The profile of the substrate has also been altered from a blank face to having three dovetail cutouts. These are necessary to interface with the new substrate mount (Fig 1 (2)), which when inserted into the substrate chassis (Fig 1 (3)) gives the substrate enough height to pass through the glassy carbon mount and reach the beam region. There are two substrate thicknesses, 0.3mm and 1mm.

![SLM Fig 1]({{ "/assets/images/SLM/Fig 1 Marked.jpg" | relative_url }}){: .inline-image-l}

The substrate chassis (Fig 1 (3), Fig 2 (3)) serves as the carriage for the substrate and provides an interface between the chassis and the piezo motor (Fig 3 (6)) that actuates it. There is no fixed joint between these two pieces, as the substrate chassis rests on the head of the piezo motor. 

Two constant-force strip springs (Fig 1 (4)) are attached at the ends of the substrate chassis and connect to the piezo mount (Fig 2 (6)). These springs provide a constant force on the substrate chassis in the -z direction, ensuring that it already remains in contact with the head of the piezo motor.

![SLM Fig 2]({{ "/assets/images/SLM/Fig 2 Marked.jpg" | relative_url }}){: .inline-image-r}

Four metal dowels (Fig 2 (5)) provide a guide for the vertical motion of the substrate chassis, keeping the substrate level throughout its motion. However, this is not achieved with the current version, the dowels are misaligned and do not ensure a perfectly vertical motion. Figure 3 depicts an issue that was commonly seen in the earliest stages of testing, where the substrate chassis would tilt and get caught during its motion. This has been temporarily resolved by thickening the dowels with masking tape as seen in Figure 2. This has significantly increased the friction experienced by the substrate chassis but has temporarily resolved any tilting issues. I want to emphasize that this is a temporary solution, and a more permanent solution should be investigated. Some possibilities include custom machined dowels, or dowels thickened with epoxy.

![SLM Fig 3]({{ "/assets/images/SLM/Fig 3 Marked.jpg" | relative_url }}){: .inline-image-r}

The glassy carbon mount (Fig 4 (7)) serves as the structure that secures the walls of the powder bed. It rests on top of the piezo mount (Fig 5 (6)), slotting into four dowel pins that secure it in place. These walls must be permeable by X-rays and remain fixed without imparting any force on the substrate. Two strips of glassy carbon (Fig 4 (11)) form these walls, each precisely 3mm tall and 50mm long. 

![SLM Fig 4]({{ "/assets/images/SLM/Fig 4 Marked.jpg" | relative_url }}){: .inline-image-r}

Each strip of glassy carbon rests on the lip of a small clamp (Fig 4 (8)) machined out of stainless steel, each clamp is then manually positioned and secured by a wing nut (Not pictured) in a position that squeezes the piece of glassy carbon on a central stopper (Fig 4 (9), Fig 5 (9)). This process is very cumbersome to perform, as keeping the strips of glassy carbon pressed against the stopper without breaking is a delicate procedure. A spring-loaded system was experimented with and showed promise but requires a more thorough investigation before any mechanism is implemented.

![SLM Fig 5]({{ "/assets/images/SLM/Fig 5 Marked.jpg" | relative_url }}){: .inline-image-r}

As discussed, the region for the powder bed is formed by the two strips of glassy carbon sandwiching the slightly shorter substrate. The strips of glassy carbon are pressed against two stoppers. The width of the central stoppers is slightly wider than the matching substrate, ideally ~0.4mm for the 0.3mm substrate and ~1.1mm for the 1mm substrate. This ensures that no horizontal force is put on the substrate that may inhibit its vertical movement, while still being tight enough that a powder bed can be formed on top of the substrate. However, this has not been consistent during testing.

The stoppers themselves were cut from a sheet of stock stainless steel of 0.3mm thickness with a pair of shears, with the thicker stopper formed from three layers of stock folded on top of each other. Unfortunately, during the cutting process the material developed a slight curl on the edges and corners, rendering their practical thickness to be closer to 0.6mm. This has resulted in an empty space forming between the substrate and the glassy carbon in some sections, leaving the metal powder free to fall away instead of staying atop the substrate and forming a powder bed. This problem should be relatively easy to solve, as the stoppers just need to be flattened with a hammer until the proper thickness is reached.

![SLM Fig 6]({{ "/assets/images/SLM/Fig 5 Marked.jpg" | relative_url }}){: .inline-image-l}

The piezo mount (Fig 3 (6)) serves as a platform for the piezo motor (Fig 3 (10)) to screw into. It is crucial that the threads on the piezo motor be protected, as any external debris may damage them and hinder the motion of the piezo. This is currently achieved by two 3d printed covers (Fig 6 (11)) that clamp down a rubber gasket on the exterior face of the piezo mount, covering the exposed faces of the motor from the chamber. Debris has still been able to accumulate on the substrate chassis by falling through the main slit in the glassy carbon mount. So far, all this debris has been caught by the substrate chassis, but a better solution needs to be found as it is still likely that metal powder may reach the piezo motor.

The system uses the 8321 Picomotor Piezo Linear Actuator from Newport Corporation to adjust the z height of the substrate. This decision was made because of the small step sizes a piezo is capable of (≤30nm), and the relatively small form factor of the piezo itself. While a small step size is important, the motor has pitfalls in many other aspects. The step size, although small, is not consistent, varying from step sizes of 18nm to 26nm. These differences compound over large distances (~1mm), and result in the head moving by a difference of 0.1mm compared to other instances, while using the same number of steps. This issue is even more grievous when moving in different directions, 2000 steps in one direction is never the same as 2000 steps in the other. As such if this process is repeated, stepping in one direction, then stepping the same number in the other, the motor inevitably drifts in one direction or the other. It is also not a consistent difference, with the increase in step number the accuracy of those steps decreases correspondingly, making long distance travel inadvisable. There is also no on-board motor encoder, although the aforementioned issues would render such a system ineffective at best. All this culminates in making the piezo a device that is cumbersome to calibrate and inconsistent to operate.

The substrate is moved vertically by the piezo motor. The head of the piezo motor is shaped like a sphere and rests in a depression on the bottom of the substrate chassis. To move the substrate in the +z direction, the piezo simply extends. To move the substrate in the -z direction, the piezo retracts, allowing the string springs on each end of the substrate chassis to pull the substrate down.

Luckily these are issues that can be mitigated as the range of motion necessary for the system is only within the 0.25-0.5mm range, making any step size inaccuracies relatively easy to compensate for. Successful runs have been conducted where the layer thickness did not vary significantly.

![SLM Fig 7]({{ "/assets/images/SLM/Fig 7 Marked.jpg" | relative_url }}){: .inline-image-l}

The re-coater mechanism can be described as a miniature version of the horizontal gantry on the Prusa i3 MK3s, a consumer FDM printer. The main structure of the gantry is composed of four 5mm stainless steel shafts cut to lengths of 120mm and 90mm for the horizontal and vertical spans respectively (Fig 7 (15)). 

The shafts are joined by two custom 3D printed mounts (Fig 7 (13) (14)) that fit two linear bearings that guide the vertical motion of the gantry along the z axis. The vertical stage is not actuated by a motor as there is no need for the z height of the re-coater to change during printing. As such the only means of adjusting the z height is to manually loosen the set screw on each mount (Fig 7 (16)). 

![SLM Fig 8]({{ "/assets/images/SLM/Fig 8 Marked.png" | relative_url }}){: .inline-image-l}

The y-axis is actuated by a NEMA 14 stepper motor (Fig 7 (17)) that drives a belt system to move the central bearing hub (Fig 7 (12)) and end piece (Fig 7 (18)), a system derived from the Prusa i3 MK3s. The timing belt is tensioned and secured within the bearing hub (Fig 8). It is important that the x-axis remains clear of any obstacles during printing, and any obstructions must only pass by temporarily.

The powder delivery is performed by a custom 3d printed end piece that serves as the powder hopper, delivery nozzle, and levelling blade. The process is initiated with the stepper motor moving the bearing hub and attached end piece across the length of the substrate. This first pass deposits a rough powder bed, which is then levelled out with the return pass as the levelling blade removes any excess. This process is then repeated once more to refine the resulting powder bed. 

![SLM Fig 9]({{ "/assets/images/SLM/Fig 9 Marked.jpg" | relative_url }}){: .inline-image-l}

The powder is stored in a chamber shaped as an inverted cone (Fig 10 (18)) with an interior angle of just over 18.5 degrees, this slope improves the flow of the powder into the nozzle while allowing for increased storage volume. The friction of the powder itself restricts the flowability while in the storage chamber and does not flow without significant agitation. As such the chamber is equipped with two mini vibrating motors (Fig 9 (19)) that are activated during the recoating process to facilitate powder deposition. The total volume of the storage chamber exceeds 550 cubic millimeters, sufficient for well over 15 layers with a 0.3mm print width and 5 layers with a 1mm print width without needing to refill. 

![SLM Fig 10]({{ "/assets/images/SLM/Fig 10 Marked.png" | relative_url }}){: .inline-image-r}

The nozzle itself is a traditional 3d printing brass nozzle (Fig 9 (20)) that is screwed into a heat set insert. This allows for the nozzle to be easily switched, allowing for easy maintenance and little downtime when assembling the system. A 0.4mm nozzle diameter was found to deposit the correct amount of powder for a 0.3mm width substrate, while a 0.6mm diameter was more suitable for a 1mm width substrate. 

The levelling blade (Fig 9 (21)) is fashioned from a plastic razor blade taped to a slice of flexible rubber. It is crucial that a non-magnetic blade be used otherwise the powder bed may be destroyed during the leveling process. The plastic razor slides along the top of the two strips of glassy carbon, clearing any excess powder while leaving the powder bed between the strips of glassy carbon intact. It is obviously a prototype, future iterations will eliminate the screws and tape to fully integrate the blade into the rest of the end piece.

The entire system rests on a Thorlabs MSB1010/M optical breadboard (Fig 7 (23). Two custom 3D printed stands (Fig 7 (22)) are screwed into the breadboard. Each stand has a slot for one leg of the piezo mount and a slot for a gantry shaft to slide into. Both the piezo mount and gantry shafts merely rest inside these stands, allowing for easy setup and disassembly. Once fully assembled on the breadboard, the entire system can be easily transported and placed inside the argon chamber.

![SLM Fig 11]({{ "/assets/images/SLM/Full Outside.jpg" | relative_url }}){: .center-image}

![SLM Fig 12]({{ "/assets/images/SLM/Full Inside.jpg" | relative_url }}){: .center-image}

The piezo and stepper motors each have an individual motor controller. The piezo motor uses the accompanying model 8742 Picomotor Controller from Newport Corporation. The system is largely plug and play once the corresponding control software is downloaded. The NEMA 14 stepper motor uses a StepperOnline DM320T Digital Stepper Driver paired with an Arduino MEGA 2560 microcontroller. Wiring the driver to the microcontroller was a trivial task. 