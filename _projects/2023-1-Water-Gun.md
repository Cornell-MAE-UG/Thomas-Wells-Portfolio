---
layout: project
title: Custom Water Gun
description: Design and development of a custom water gun for CUAB
technologies: [Fusion 360, Ansys Mechanical, 3D Printing]
image: /assets/images/SLM/Thumbnail.jpg
---



<p><strong>Background </strong></p>

In the fall of my freshman year, I joined the Cornell AutoBoat Project Team, whose goal is to design and manufacture a completely autonomous boat to compete in the annual RoboBoat Competition held by RoboNation. In the competition the vessle has to navigate a challenge course and complete various tasks along the way. I was tasked with redesigned our competition water gun, focusing on implementing a remotely adjustable nozzle. A system already existed when I joined, but the pitch of the nozzle was fixed and had to be manually set on land before the competition run began. This obviously was not ideal and it was my job to improve it.

![Old WG]({{ "/assets/images/Water Gun/Old Water Gun.png" | relative_url }}){: .inline-image-l}

<p><strong>Design Requirements & Key Challenges </strong></p>

The only requirement given to me was that the nozzle needed to change pitch remotely. What angles does it have to reach? No clue. How quick should it adjust? Not an inkling. What range does it need? Not sure yet. So I was given an open problem, one that I could tackle however I wanted to, and I jumped right in.

The first lesson came with setting my initial constraints, I had not been given much to work with and was initially very lax with formally establishing any design constraints, something that would quickly come back to bite me. One thing I did decide was that the nozzle pitch needed to go from 0 to 45 degrees, as any other angle would be redundant.

The main challenge I was aware of what that the motor needed to be at least splash resistant, and ideally able to be submerged in the worst case scenario, so with this lax decision making I made the first model of the new water gun.

<p><strong>First Iterations </strong></p>

That lack of direction really shows in the first two iterations, the designs are obtuse, difficult to manufacture, and overall confused. There's a gear train in there for no reason as I hadn't selected a motor at that point, there wasn't really an idea of what the nozzle would look like, and the waterproofing was there but fundamentally flawed.

It was after a design review where I was asked what everything did and couldn't really answer that I realized my mistakes, and started to redesign.

![WG v1]({{ "/assets/images/Water Gun/Redesign v1.png" | relative_url }}){: .inline-image-r}

![WG v2]({{ "/assets/images/Water Gun/Redesign v2.png" | relative_url }}){: .inline-image-r}

<p><strong>Second Iteration </strong></p>

Before I began my redesign I took some time to evaluate my constraints. With some actual numbers I was able to determine that a common hobby servo was both strong and fast enough to do everything I wanted to. This allowed me to massively simplify my design by completely removing the gear train and as a bonus, rotate the nozzle inline with the axle. The waterproofing greatly benefitted from the reduction in complexity, with the main ring becoming one continuous loop instead of several discontinuous segments. The nozzle was now fully realized, and the first version of the deck mount appears.

This is obviously a massive improvement over the prior designs, but there are still quite a few issues. Primarily, the waterproofing had improved but was still flawed, I also hadn't considered that the nozzle rotating would intersect with the boat deck.

![WG v3]({{ "/assets/images/Water Gun/Watergun Gearbox V3.png" | relative_url }}){: .inline-image-l}

<p><strong>Final Iteration </strong></p>

With the final version, I flipped the orientation of the enclosure so that it would be positioned vertically, this gives the nozzle enough clearance to rotate freely. The completed deck mount uses four ball nosed spring plungers to secure the motor enclosure, aided by a set of dovetails. The biggest change was the interior, the servo is not longer mounted by screws, but instead pressed into four cylindrical extrusions and held in place by a wedge. This eliminates the four exterior screw holes, simplifying waterproofing. 

![WG Final]({{ "/assets/images/Water Gun/New Watergun Front.png" | relative_url }}){: .inline-image-l}


<p><strong>Waterproofing </strong></p>

The biggest challenge in this entire project was protecting the servo from water damage, and although the waterproofing proved to be tricky the final solutions turned out pretty elegant. 

The lid compresses a cut to length rod of buna n rubber inside a channel set into the main body of the motor enclosure. I consulted the Parker O-Ring handbook to determine the proper fillet radii. 

The axle originally used a press-fit rotary seal, but it leaked as it did not interface well with the rough FDM printed surface. Instead it is now a custom rotary seal using a 14mm O-ring.

![Waterproofing Top]({{ "/assets/images/Water Gun/Waterproofing Top View.png" | relative_url }}){: .inline-image-l}
![Waterproofing Side]({{ "/assets/images/Water Gun/Waterproofing Side View.png" | relative_url }}){: .inline-image-r}

<p><strong>Nozzle Mount FEA </strong></p>

The nozzle has two main components: the hose fitting/nozzle, the only part that carried over from the old water gun, and the axle mount. The mount was designed to mimic a shaft connector, by tightening two screws the mount would compress the axle, securing the nozzle in place. 

I performed finite element analysis using Ansys Mechanical to determine if my part could support this load, and the results showed that it would pass with flying colors. This was my first exposure to both FEM and FEA, and I've since strived to improve my skills with Ansys and other simulation software.

![Nozzle FEA]({{ "/assets/images/Water Gun/FEA.png" | relative_url }}){: .center-image}

<p><strong>Reflection</strong></p>

This project was my first real taste of the whole engineering process, the cycle of conceptualization, design, iteration, prototype, and repeat was a great learning experience for me. I now have a more disciplined approach to projects, ensuring that everything I do has a reason behind it, and I'm not wasting time and effort on something that didn't need to be done anyways. They say that slow is smooth, and smooth is fast, a mentality that I will be taking with me into all future projects, now that I've gotten a taste of it firsthand. 

If I were to do it all over again, I would put more emphasis on flexibility, the current design only supports one specific size of servo, and although that size is a common standard, it isn't universally applicable. Waterproofing the wire outlet is another area of improvement. After some deliberation with my team lead, we decided that sealing putty would be used, making it that the joint would have to be destroyed if we ever wanted to swap out the servo. I would've liked to house the wire within 10mm rubber tubing and use the same O-ring seal that the axle uses, but that solution wouldn't interface with the boat's electrical system without compromising waterproofing on their end.