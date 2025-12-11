---
layout: project
title: Miniature Selective Laser Melting System
description: Design and simulation of a torque wrench
technologies: [Fusion 360, MATLAB, CNC Machining, 3D Printing]
image: /assets/images/SLM/Thumbnail.jpg
---

I joined Moridi Research Group at the end of my freshman spring, I then spent the following summer and the entirety of my sophomore year working on building a minizatrized selective laser melting (SLM) additive manufacturing system. The goal was to create a platform that would enable multi-layer printing during in-situ synchrotron experiemnts. On this page I have replicated my technical report that I submitted.

<p><strong>Introduction </strong></p>

Metal additive manufacturing has revolutionized the field of manufacturing, processes such as selective laser melting (SLM) have enabled production of components that were previously challenging or impossible. The field has progressed rapidly, and perhaps too quickly where the underlying processes are not fully understood. This failure can be largely attributed to an inability to conduct experiments amid the production process as specialized systems to do so either do not exist or are exceedingly hard to come by.

This project seeks to create a miniaturized SLM system that enables multi-layer printing during in-situ synchrotron experiments. The main body of the system must fit within a sealed argon chamber and the print area must be transparent to x-rays along the main axis. The system should be capable of automatically printing at least 5 consistent layers of 50-micron height with minimal downtime. Although not strictly necessary it is a personal goal to make the system easy and intuitive to set up and operate.

<p><strong>Design Overview </strong></p>

This section provides a comprehensive description of the mechanical and electrical elements of the Multi-Layer SLM System. The mechanical system consists of three main components: the substrate chassis, horizontal re-coater mechanism, and the vertical piezo mount. The electrical system consists of the piezo controller, stepper driver, and Arduino microcontroller. 

<p><strong>Substrate Design </strong></p>

The substrate (Fig 1 (1)) very closely resembles the form factor that has been used in the lab in the past with a could design changes. The overall length of the substrate has been reduced from 50mm to 40mm. This is a significant decrease in length but is necessary to ensure that the glassy carbon clamps (Fig 4 (8)) do not exert any force on the substrate. The profile of the substrate has also been altered from a blank face to having three dovetail cutouts. These are necessary to interface with the new substrate mount (Fig 1 (2)), which when inserted into the substrate chassis (Fig 1 (3)) gives the substrate enough height to pass through the glassy carbon mount and reach the beam region. There are two substrate thicknesses, 0.3mm and 1mm.

![SLM Fig 1]({{ "/assets/images/SLM/Fig 1 Marked.jpg" | relative_url }}){: .inline-image-l}

<p><strong>Substrate Chassis </strong></p>

The substrate chassis (Fig 1 (3), Fig 2 (3)) serves as the carriage for the substrate and provides an interface between the chassis and the piezo motor (Fig 3 (6)) that actuates it. There is no fixed joint between these two pieces, as the substrate chassis rests on the head of the piezo motor. 

Two constant-force strip springs (Fig 1 (4)) are attached at the ends of the substrate chassis and connect to the piezo mount (Fig 2 (6)). These springs provide a constant force on the substrate chassis in the -z direction, ensuring that it already remains in contact with the head of the piezo motor.

![SLM Fig 2]({{ "/assets/images/SLM/Fig 2 Marked.jpg" | relative_url }}){: .inline-image-r}

Four metal dowels (Fig 2 (5)) provide a guide for the vertical motion of the substrate chassis, keeping the substrate level throughout its motion. However, this is not achieved with the current version, the dowels are misaligned and do not ensure a perfectly vertical motion. Figure 3 depicts an issue that was commonly seen in the earliest stages of testing, where the substrate chassis would tilt and get caught during its motion. This has been temporarily resolved by thickening the dowels with masking tape as seen in Figure 2. This has significantly increased the friction experienced by the substrate chassis but has temporarily resolved any tilting issues. I want to emphasize that this is a temporary solution, and a more permanent solution should be investigated. Some possibilities include custom machined dowels, or dowels thickened with epoxy.

![SLM Fig 3]({{ "/assets/images/SLM/Fig 3 Marked.jpg" | relative_url }}){: .inline-image-r}

<p><strong>Glassy Carbon Mount </strong></p>

The glassy carbon mount (Fig 4 (7)) serves as the structure that secures the walls of the powder bed. It rests on top of the piezo mount (Fig 5 (6)), slotting into four dowel pins that secure it in place. These walls must be permeable by X-rays and remain fixed without imparting any force on the substrate. Two strips of glassy carbon (Fig 4 (11)) form these walls, each precisely 3mm tall and 50mm long. 

![SLM Fig 4]({{ "/assets/images/SLM/Fig 4 Marked.jpg" | relative_url }}){: .inline-image-l}

Each strip of glassy carbon rests on the lip of a small clamp (Fig 4 (8)) machined out of stainless steel, each clamp is then manually positioned and secured by a wing nut (Not pictured) in a position that squeezes the piece of glassy carbon on a central stopper (Fig 4 (9), Fig 5 (9)). This process is very cumbersome to perform, as keeping the strips of glassy carbon pressed against the stopper without breaking is a delicate procedure. A spring-loaded system was experimented with and showed promise but requires a more thorough investigation before any mechanism is implemented.

![SLM Fig 5]({{ "/assets/images/SLM/Fig 5 Marked.jpg" | relative_url }}){: .inline-image-l}

<p><strong>Powder Bed Receptable </strong></p>

As discussed, the region for the powder bed is formed by the two strips of glassy carbon sandwiching the slightly shorter substrate. The strips of glassy carbon are pressed against two stoppers. The width of the central stoppers is slightly wider than the matching substrate, ideally ~0.4mm for the 0.3mm substrate and ~1.1mm for the 1mm substrate. This ensures that no horizontal force is put on the substrate that may inhibit its vertical movement, while still being tight enough that a powder bed can be formed on top of the substrate. However, this has not been consistent during testing.

The stoppers themselves were cut from a sheet of stock stainless steel of 0.3mm thickness with a pair of shears, with the thicker stopper formed from three layers of stock folded on top of each other. Unfortunately, during the cutting process the material developed a slight curl on the edges and corners, rendering their practical thickness to be closer to 0.6mm. This has resulted in an empty space forming between the substrate and the glassy carbon in some sections, leaving the metal powder free to fall away instead of staying atop the substrate and forming a powder bed. This problem should be relatively easy to solve, as the stoppers just need to be flattened with a hammer until the proper thickness is reached.

![SLM Fig 6]({{ "/assets/images/SLM/Fig 6 Marked.jpg" | relative_url }}){: .inline-image-r}

<p><strong>Piezo Mount & Enclosure </strong></p>

The piezo mount (Fig 3 (6)) serves as a platform for the piezo motor (Fig 3 (10)) to screw into. It is crucial that the threads on the piezo motor be protected, as any external debris may damage them and hinder the motion of the piezo. This is currently achieved by two 3d printed covers (Fig 6 (11)) that clamp down a rubber gasket on the exterior face of the piezo mount, covering the exposed faces of the motor from the chamber. Debris has still been able to accumulate on the substrate chassis by falling through the main slit in the glassy carbon mount. So far, all this debris has been caught by the substrate chassis, but a better solution needs to be found as it is still likely that metal powder may reach the piezo motor.

<p><strong>Pitfalls of The Piezo Motor </strong></p>

The system uses the 8321 Picomotor Piezo Linear Actuator from Newport Corporation to adjust the z height of the substrate. This decision was made because of the small step sizes a piezo is capable of (≤30nm), and the relatively small form factor of the piezo itself. While a small step size is important, the motor has pitfalls in many other aspects. The step size, although small, is not consistent, varying from step sizes of 18nm to 26nm. These differences compound over large distances (~1mm), and result in the head moving by a difference of 0.1mm compared to other instances, while using the same number of steps. This issue is even more grievous when moving in different directions, 2000 steps in one direction is never the same as 2000 steps in the other. As such if this process is repeated, stepping in one direction, then stepping the same number in the other, the motor inevitably drifts in one direction or the other. It is also not a consistent difference, with the increase in step number the accuracy of those steps decreases correspondingly, making long distance travel inadvisable. There is also no on-board motor encoder, although the aforementioned issues would render such a system ineffective at best. All this culminates in making the piezo a device that is cumbersome to calibrate and inconsistent to operate.

The substrate is moved vertically by the piezo motor. The head of the piezo motor is shaped like a sphere and rests in a depression on the bottom of the substrate chassis. To move the substrate in the +z direction, the piezo simply extends. To move the substrate in the -z direction, the piezo retracts, allowing the string springs on each end of the substrate chassis to pull the substrate down.

Luckily these are issues that can be mitigated as the range of motion necessary for the system is only within the 0.25-0.5mm range, making any step size inaccuracies relatively easy to compensate for. Successful runs have been conducted where the layer thickness did not vary significantly.

![SLM Fig 7]({{ "/assets/images/SLM/Fig 7 Marked.jpg" | relative_url }}){: .inline-image-l}

<p><strong>Re-coater Gantry </strong></p>

The re-coater mechanism can be described as a miniature version of the horizontal gantry on the Prusa i3 MK3s, a consumer FDM printer. The main structure of the gantry is composed of four 5mm stainless steel shafts cut to lengths of 120mm and 90mm for the horizontal and vertical spans respectively (Fig 7 (15)). 

The shafts are joined by two custom 3D printed mounts (Fig 7 (13) (14)) that fit two linear bearings that guide the vertical motion of the gantry along the z axis. The vertical stage is not actuated by a motor as there is no need for the z height of the re-coater to change during printing. As such the only means of adjusting the z height is to manually loosen the set screw on each mount (Fig 7 (16)). 

![SLM Fig 8]({{ "/assets/images/SLM/Fig 8 Marked.png" | relative_url }}){: .inline-image-l}

The y-axis is actuated by a NEMA 14 stepper motor (Fig 7 (17)) that drives a belt system to move the central bearing hub (Fig 7 (12)) and end piece (Fig 7 (18)), a system derived from the Prusa i3 MK3s. The timing belt is tensioned and secured within the bearing hub (Fig 8). It is important that the x-axis remains clear of any obstacles during printing, and any obstructions must only pass by temporarily.

<p><strong>Powder Delivery </strong></p>

The powder delivery is performed by a custom 3d printed end piece that serves as the powder hopper, delivery nozzle, and levelling blade. The process is initiated with the stepper motor moving the bearing hub and attached end piece across the length of the substrate. This first pass deposits a rough powder bed, which is then levelled out with the return pass as the levelling blade removes any excess. This process is then repeated once more to refine the resulting powder bed. 

![SLM Fig 9]({{ "/assets/images/SLM/Fig 9 Marked.jpg" | relative_url }}){: .inline-image-r}

The powder is stored in a chamber shaped as an inverted cone (Fig 10 (18)) with an interior angle of just over 18.5 degrees, this slope improves the flow of the powder into the nozzle while allowing for increased storage volume. The friction of the powder itself restricts the flowability while in the storage chamber and does not flow without significant agitation. As such the chamber is equipped with two mini vibrating motors (Fig 9 (19)) that are activated during the recoating process to facilitate powder deposition. The total volume of the storage chamber exceeds 550 cubic millimeters, sufficient for well over 15 layers with a 0.3mm print width and 5 layers with a 1mm print width without needing to refill. 

![SLM Fig 10]({{ "/assets/images/SLM/Fig 10 Marked.png" | relative_url }}){: .inline-image-r}

The nozzle itself is a traditional 3d printing brass nozzle (Fig 9 (20)) that is screwed into a heat set insert. This allows for the nozzle to be easily switched, allowing for easy maintenance and little downtime when assembling the system. A 0.4mm nozzle diameter was found to deposit the correct amount of powder for a 0.3mm width substrate, while a 0.6mm diameter was more suitable for a 1mm width substrate. 

The levelling blade (Fig 9 (21)) is fashioned from a plastic razor blade taped to a slice of flexible rubber. It is crucial that a non-magnetic blade be used otherwise the powder bed may be destroyed during the leveling process. The plastic razor slides along the top of the two strips of glassy carbon, clearing any excess powder while leaving the powder bed between the strips of glassy carbon intact. It is obviously a prototype, future iterations will eliminate the screws and tape to fully integrate the blade into the rest of the end piece.

<p><strong>Baseplate </strong></p>

The entire system rests on a Thorlabs MSB1010/M optical breadboard (Fig 7 (23). Two custom 3D printed stands (Fig 7 (22)) are screwed into the breadboard. Each stand has a slot for one leg of the piezo mount and a slot for a gantry shaft to slide into. Both the piezo mount and gantry shafts merely rest inside these stands, allowing for easy setup and disassembly. Once fully assembled on the breadboard, the entire system can be easily transported and placed inside the argon chamber.

![SLM Fig 11]({{ "/assets/images/SLM/Full Outside Chamber.jpg" | relative_url }}){: .center-image}

![SLM Fig 12]({{ "/assets/images/SLM/Full Inside.jpg" | relative_url }}){: .center-image}

<p><strong>Motor Controllers </strong></p>

The piezo and stepper motors each have an individual motor controller. The piezo motor uses the accompanying model 8742 Picomotor Controller from Newport Corporation. The system is largely plug and play once the corresponding control software is downloaded. The NEMA 14 stepper motor uses a StepperOnline DM320T Digital Stepper Driver paired with an Arduino MEGA 2560 microcontroller. Wiring the driver to the microcontroller was a trivial task. 

<p><strong>Custom Control Software </strong></p>

Software is by far my weakest skill, a lot of the code was copied from online tutorials or from sample code, I will try to give a general overview of how things work without going into the nitty gritty as I don’t have an excellent conceptual understanding of what’s happening under the hood. There may be issues that I outline that stem purely from my lack of experience with the C# language and my lack of high-level coding abilities. To operate the system, two separate pieces of software were written, one for the Arduino microcontroller and one to run on the laptop. 

<p><strong>Arduino Code </strong></p>

The Arduino code is responsible for controlling the NEMA 14 stepper motor and the two vibrating motors. It is rather simple with only a couple of main methods. The program calculates how many steps are required to travel across the gantry once based off some input values. The rest of the program is dedicated to waiting for a signal to activate the stepper and vibrating motor. While a serial port is open, the Arduino will wait until it receives a specific character from the computer, a ‘#’. Once it has received said character, it will run the re-coater assembly across the substrate a specific number of times, through experimentation it was determined that two passes were the sweet spot, then send a signal to the computer indicating that it is finished. All in all the program works well, there was one instance where it did not receive the signal properly. This was likely because of the low baud rate I was using at the time, and since switching to a higher baud rate it has not occurred again. It is important to note that the Arduino does not need additional input signals while it is executing its instructions, this will be important for future optimizations.

<p><strong>C# Code </strong></p>

The main portion of the control code was written in C# with the Microsoft .NET Windows forms app template using the Visual Studio IDE. This allowed me to make a custom windows program with a GUI to control the various elements of the SLM system. I prioritized making as many things as possible toggleable so I could test systems separately. An in-built status monitor lets me keep track of what is happening with the system when it is not able to be viewed. I wanted to make a kill switch of some sort but found it just beyond my abilities.

![SLM Fig 13]({{ "/assets/images/SLM/GUI Screenshot.png" | relative_url }}){: .center-image}

All the key components of the SLM system can be controlled from this GUI. The important sections are the COM PORT text box that has you choose the correct COM port that the Arduino microcontroller is connected to. The piezo enabled and stepper enabled check boxes enable their respective motors, so that they could be tested independently or simultaneously if desired. The number of layers text box allows you to choose how many layers the system should print consecutively. The start movement button begins the program once all the prior settings are defined.

If only the piezo is selected, the program will read the number of layers the user has selected (defaulted to 1), and the layer height. It is important to note that adjusting the layer height, although possible, is not recommended, this will be explained shortly. Once this information is read, it is used in a method I got from the piezo sample code that came with the motor to move the piezo. This involves connecting to the piezo, interpreting the distance to move, moving the piezo, then disconnecting. If this is the final time the piezo will move for the operation, interpolate the number of steps required to return to its original position and execute that instead. 

If only the stepper is selected, the program will read the number of layers the user has selected (defaulted to 1) and will incrementally send signals to the Arduino until the number of layers has been achieved. This process currently relies on the Arduino sending a return signal back to the computer to know that one layer has finished. This step can be eliminated as the stepper will always take a certain amount of time of finish a layer, so a simple delay could be used within the C# code instead of relying on the Arduino to send a signal that could theoretically be lost.

If both are selected, the program will do everything previously mentioned one after the other with some extra steps. First the piezo will be activated, lowering the substrate, then the stepper activates creating a powder bed, a delay for the laser firing occurs, then the process is repeated until the number of desired layers is reached, and the piezo goes home. This is the idealized version of what the system should do, but there are a lot of issues along the way.

<p><strong>Problems With My Bad Code </strong></p>

This process is inefficient and fundamentally flawed. 1: As previously mentioned, the step size of the piezo is not consistent, so there is no practical way to interpolate a distance to travel to the number of steps that are necessary to move that distance. 2: Furthermore, as mentioned, the number of steps required to move in one direction is not the same as the number of steps required to move the same distance in the other direction, rendering the return home step inaccurate at best. 3: The program must connect and disconnect to the piezo every time it wants to move it. This takes a significant amount of time (around 10-15 seconds), making any tests done less indicative of real SLM printing situations. 4: There is no way to actively monitor the position of the piezo, as the entire program freezes once the signals to start moving have been sent. This renders any status updates backlogged, and they are only sent after the piezo has finished its movement completely. 5: There is no communication with the laser currently, the program is completely detached from it. This requires the operator to wait for a status update on one computer, and then must go to another computer to activate the laser. 

The first two problems can be alleviated by the fact that we won’t be experimenting with different layer heights soon. So, the number of steps can be set to a fixed value, as that will keep the layer heights the same amount. I’ve found that the number of steps downwards for a 0.05mm layer height is around 2700 steps, and the corresponding number of steps upwards is around 2200 steps. 

The third and fourth problems are trickier and lie with the fundamental way the program works. The Arduino and the piezo controller both require a serial connection to operate. On modern computers there are no proper serial ports, instead often the USB ports are used with an adaptor. This means that the number of serial ports available to a computer is the number of USB ports that computer has. The way I have the program written currently only utilizes one of those serial ports, as using multiple would require me to multithread my program which I am not currently capable of doing. This stops me from doing any active monitoring of the piezo during its operation, but more importantly, means that every time I want to switch from the stepper to the piezo motor, I must first disconnect from one then reconnect to the other. When this is happening literally every layer, the time it takes to disconnect and reconnect really starts to add up, to the point where most of the printing time is spent connecting and disconnecting instead of actually printing. Hopefully these issues can be resolved once I learn how to multithread, but it should be noted that a computer with at least 2 USB ports should be used to run the program as otherwise any multithreading will be nullified by the number of serial channels available to the computer in the first place.

The fifth problem is one that I haven’t tackled at all due to the lack of time and knowledge about the laser system. I do not think it should be too dissimilar to what I have created thus far and likewise not too challenging

<p><strong>Testing and Results </strong></p>

The system was tested on two separate occasions and produced excellent results. The first trial used the 0.3mm substrate and the second trial used the 1mm substrate. On both occasions a powder bed was formed, and multiple layers were able to be printed. The layers were each 4mm long and 5 layers tall, showing that the system is capable of its primary objective of enabling multi-layer printing.

To conduct these tests the height of the beam aligner had to be altered as we were now printing at a significantly higher point compared to previous prints. This also necessitated a small attachment added to the top of the argon chamber to avoid any potential damage to the window as the power density of the beam had changed as shown in figure 12. 

![SLM Fig 14]({{ "/assets/images/SLM/Empty Chamber.jpg" | relative_url }}){: .inline-image-r}

Nonetheless two successful trials were conducted, the results of which are shown in figure 13. The 0.3mm substrate trial is on the top and the 1mm substrate trial is on the bottom. Both trials produced multiple instances of successful multi-layer printing. The 1mm thickness trials were more successful as it was easier to align the laser to the center of the substrate. Each segment is 4mm long, the laser was operated at 25% power with a 40mm/s laser travel speed.

![SLM Fig 15]({{ "/assets/images/SLM/Printed Samples.jpg" | relative_url }}){: .inline-image-r}

Both trials, however, showed that there is a lot of room for improvement. The melt pools tended to bead together and did not consistently form a continuous line. This was improved by lowering the laser travel speed, although the difference was minimal. What proved to be more important was properly aligning the laser to the substrate. It was exceptionally difficult to do this as the chamber was very loose and rocked with even a little agitation. Along with this is proved to be difficult to see where the trace laser was on the substrate, as most of the system inhibited the view. Using a phone camera to look through the top window proved to be the most effective method of checking the laser alignment, as shown in Figure 14. This should be easier once the chamber is properly secured and level. 

![SLM Fig 16]({{ "/assets/images/SLM/Top View.jpg" | relative_url }}){: .inline-image-r}

Another issue that came up was the glassy carbons bending along their span, with their centers further from the substrate than the edges. This can be alleviated by using shorter lengths of glassy carbon than the 50mm lengths currently in use. Even a reduction to 49.5mm should be able to alleviate these issues as then the glassy carbon strips won’t get any pressure from their ends and try to buckle.

A problem that has persisted from prior experiments was the substrate itself deforming under the thermal load of the laser. There is no easy way to solve this problem, but it appears that the dovetail design on the substrate and substrate mount has somewhat alleviated this issue. I believe this is something that should be investigated further.

<p><strong>Conclusion </strong></p>
The prototype SLM system, despite its shortcomings, has clearly shown its ability to execute its given objective. There are still many things that can and should be improved, so I’ve included a list of the most pressing concerns to conclude this report.
    1-	Speeding up the connection process between the computer and piezo controller, ideally this should be eliminated entirely
    2-	The laser control needs to be integrated with the rest of the system
    3-	The piezo motor needs to be better protected from powders
    4-	Homing the piezo motor is largely done by sight, having a measured method of achieving this necessary
    5-	The glassy carbon clamps are clunky to deal with and have caused the untimely demise of two glassy carbon strips. A more streamlined process to mount them should be found
    6-	The powder bed leveler works, but needs a finalized version
