---
layout: project
title: Cube Craze Competition Bot
description: Design and Construction of a Competiton Robot
technologies: [Fusion 360, Arduino UNO]
image: /assets/images/Mechatronics Bot/Thumbnail.png
---

As the culmination of my mechatronics class in junior spring, I formed a team with two other students to design and built a robot to compete in the class's annual "Cube Craze" competition. In each compeition round, two robots face off in an arena with the goal of collecting the most blocks after a one minute period. The bots themselves had several restrictions on the design, they had to use the provided frame and motors, had to fit within an eight inch square at the start of each round, and could not cost more than $40. There were 62 teams that competed this year, and our group made it to the sweet sixteen before being knocked out. Leading up to the day of competition, our group had to complete four milestones to prove our readiness, these milestones sequentially built up to the final robot. 

<p><strong>Milestone 1: Design strategy presentation </strong></p>

The first step in and project is to define the objectives, and this short presentation laid out our goals clearly. Our overarching strategy focused on speed, as the first robot to reach the cubes in the center would be able to take control of the cubes early. It was my responsibility to lay out how we aimed to achieve that goal. The competition rules stipulated that no alterations could be made to the provided motors, which informed the following two design choices we made. First, we would use our limited budget to purchase two larger wheels. Second, we would create a gear train to increase the speed of the wheels as well. With this strategy, we were able to move on to the second milstone.

<p><strong>Milestone 2: Mobility </strong></p>

The second miltstone had us drive along a simple course to demonstrate our manuverability. My group achieved this by hard coding durations for each movement command in the bot. It was a simple task that was a good warm-up for the following task.

![Milestone 2]({{ "/assets/images/Mechatronics Bot/Milestone2.png" | relative_url }}){: .inline-image-l}

<p><strong>Milestone 3: Color Detection </strong></p>

For milestone three, our bot would have to drive on a narrow a blue and yellow track, immediately stop when it hits the other color, turn 180 degrees, drive to the back of the starting color, and stop again.

![Milestone 3]({{ "/assets/images/Mechatronics Bot/Milestone3.png" | relative_url }}){: .inline-image-l}

This objective proved to be difficult, but mostly because of an arbitrary limitation we put on ourselves. We thought that we were limited to a single color sensor as our only sensor, when in reality we had the option of adding two additional QTI sensors as well. Nonetheless we were able to complete the task, and the limitation resulted in some creative problem solving on my part in the bot's algorithmn. 

The workflow of the bot went as follows: 

1. 

Here's the code we used to complete the task, I've broken it up so I can 


```C++
```

<p><strong>Milestone 4: Cube Clearing </strong></p>