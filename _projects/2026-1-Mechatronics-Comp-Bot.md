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

![Milestone 2]({{ "/assets/images/Mechatronics Bot/Milestone2.png" | relative_url }}){: .center-image}

<p><strong>Milestone 3: Color Detection </strong></p>

For milestone three, our bot would have to drive on a narrow a blue and yellow track, immediately stop when it hits the other color, turn 180 degrees, drive to the back of the starting color, and stop again.

![Milestone 3]({{ "/assets/images/Mechatronics Bot/Milestone3.png" | relative_url }}){: .center-image}

This objective proved to be difficult, but mostly because of an arbitrary limitation we put on ourselves. We thought that we were limited to a single color sensor as our only sensor, when in reality we had the option of adding two additional QTI sensors as well. Nonetheless we were able to complete the task, and the limitation resulted in some creative problem solving on my part in the bot's algorithmn. The code is certainly not pretty, but it compleated the challenge nonetheless. The main area of improvment I could make it breaking up several portions into their own functions. For example the behaviour when the bot hits the black edge is contained entirely within the main loop, when it could be easily moved to its own function to improve readability. 

```C++
//output: pin 3
//s3:ground
//s2:ground
//s1:ground
//s0:5v
//  COLOR SENSOR 
volatile unsigned int period = 0;
volatile unsigned int timer = 0; // "timer" : stores the value of TIMER1 

ISR(PCINT2_vect)
{
    if (PIND & (1 << PD3))  // resets the timer to zero on a rising edge on D3
    {
        TCNT1 = 0;  // Reset Timer1
    }
    // and stores the timer value in a variable ("timer") on a falling edge (or vice versa).
    else   
    {
        timer = TCNT1;  
    }
}

void initColor()
{
    DDRD &= ~(1 << PD3);   // Set D3 as input from sensor
    PORTD |= (1 << PD3);   // Enable pull-up resistor on D3
    DDRD |= (1 << PD4);    // Set D4 as output for LED
    PORTD &= ~(1 << PD4);  // Start D4 LOW

    // b. Initialize my pin change interrupt
    PCICR |= (1 << PCIE2);     // Enable Pin Change Interrupt for Port D
    PCMSK2 |= (1 << PCINT19);  // Enable PCINT19 (PD3 / D3)

    // c. Initialize timer
    TCCR1A = 0x00;        // Set to normal mode
    TCCR1B = (1 << CS10); // Prescaler 1
    TCNT1 = 0;            // Reset timer
    sei();                // Enable global interrupts
}

int getColor()
{
     // a.	Enable the specific bit for your Pin Change Interrupt
    PCMSK2 |= (1 << PCINT19);   // Enable interrupt on D3

    // b.	Add a short delay (5 ~ 10 milliseconds)
    _delay_ms(10);               // Short delay 

    // c.	Disable the specific bit for your pin change interrupt
    PCMSK2 &= ~(1 << PCINT19);   // Disable interrupt on D3

    // d.	Return the period in units of time
    return timer*2*0.0625;              // Return measured time
    
}

//  MOTORS 

void stop_motors() 
{
    PORTB &= 0b11110000;
}

void drive_forward() 
{
    PORTB &= 0b11111110;
    PORTB |= 0b00000010;

    PORTB &= 0b11111011;
    PORTB |= 0b00001000;
}

void drive_backward() 
{
    PORTB |= 0b00000001;
    PORTB &= 0b11111101;

    PORTB |= 0b00000100;
    PORTB &= 0b11110111;
}

void turn_right() 
{
    PORTB &= 0b11111110;
    PORTB |= 0b00000010;

    PORTB |= 0b00000100;
    PORTB &= 0b11110111;
}

void turn_left() 
{
    PORTB |= 0b00000001;
    PORTB &= 0b11111101;

    PORTB &= 0b11111011;
    PORTB |= 0b00001000;
}

void turn_angle(int angle) 
{
    int turn_time = angle * 6;

    if (angle > 0) {
        PORTB &= 0b11111110;
        PORTB |= 0b00000010;

        PORTB |= 0b00000100;
        PORTB &= 0b11110111;

        _delay_ms(turn_time);
        stop_motors();
    }
    else if (angle < 0) {
        turn_time = turn_time * -1;
        PORTB |= 0b00000001;
        PORTB &= 0b11111101;

        PORTB &= 0b11111011;
        PORTB |= 0b00001000;

        _delay_ms(turn_time);
        stop_motors();
    }
}


int blue_threshold = 200;   // reading above this value is blue
int black_threshold = 400;  // reading above this value is black

int color_comp = 0;         // var to store color comparison
int hit_black = 0;          // Counter to see how many times we've hit the black boundary
int main(void)
{
    DDRB = 0b00001111; // motors

    // Begin serial output
    Serial.begin(9600);
    Serial.println("Began");
    
    // Initialize color detection
    initColor();

    int start_color;    // Var to store the sensor value color that we start on
    int current_color;  // Var to store current sensor value

    _delay_ms(500); 

    // Step 1: Detect and classify starting color 
    current_color = getColor();
    if (current_color >= blue_threshold) {
        start_color = 1;    // 1 for blue
    }
    else {
        start_color = 0;    // 0 for yellow
    }
    Serial.println("First color:");
    Serial.print(start_color);

    // Step 2: Drive until color changes 
    while (1) {
        current_color = getColor(); // Get current color every loop
        
        if (current_color <= blue_threshold) {
            color_comp = 0; // color_comp = 0 if yellow
        } 
        else if (current_color > blue_threshold && current_color <= black_threshold) {
            color_comp = 1; // color_comp = 1 if blue  
        } 
        else if (current_color > black_threshold) {
            color_comp = 2; // color_comp = 2 if black
        }

        if (color_comp == 0 && 0 != start_color) { // If currently yellow and start color is not yellow, break
            stop_motors();
            Serial.println("Detected Yellow, turning around");
            break;
        }
        else if (color_comp == 1 && 1 != start_color) { // If currently blue and start color is not blue, break
            stop_motors();
            Serial.println("Detected Blue, turning around");
            break;
        }
        else if (color_comp == 2) { // If currently black, turn until no longer black
            stop_motors();
            Serial.println("Detect Black, finding color on right");
            hit_black = hit_black + 1;

            // couple local vars
            bool searching = true;
            int turns = 0;
            int turn_dir = 1;

            while (searching) {
                Serial.println("searching");

                if (turn_dir == 1) {
                    Serial.println("Searching right");
                    turn_right(); // turn set degree
                    _delay_ms(100);
                    stop_motors();
                    turns = turns + 1;      // Increment turns counter
                }
                else if (turn_dir == 0) {
                    Serial.println("Searching left");
                    turn_left();
                    _delay_ms(100);
                    stop_motors();
                    turns = turns + 1;
                }

                current_color = getColor(); // Get current color again
                
                if (current_color < black_threshold) {  // If we find a nonblack color, turn a little more and break
                    Serial.println("Found color");
                    if (turn_dir == 1) {
                        turn_right(); // turn set degree
                        _delay_ms(100);
                        stop_motors();
                    }
                    else if (turn_dir == 0) {
                        turn_left();
                        _delay_ms(100);
                        stop_motors();
                    }
                    searching = false;
                }
                else if (turns >= 4) {      // If we reach 90 degrees of turn, flip turning direction
                    Serial.println("Reach 90 degrees, flipping direction");
                    turn_dir = 0;
                    turns = 0;
                }
            }
        }

        drive_forward();
        Serial.println("moving");
        Serial.println(current_color);
        Serial.println(color_comp);
    }

    _delay_ms(200);

    // Turn 180° 
    turn_angle(180);
    _delay_ms(1050);
    stop_motors();

    drive_forward();
    _delay_ms(200);
    stop_motors();

    // Step 4: Drive back to edge
    while (1) {
        current_color = getColor(); // Get current color every loop
        
        if (current_color <= blue_threshold) {
            color_comp = 0; // color_comp = 0 if yellow
        } 
        else if (current_color > blue_threshold && current_color <= black_threshold) {
            color_comp = 1; // color_comp = 1 if blue  

        } 
        else if (current_color > black_threshold) {
            color_comp = 2; // color_comp = 2 if black
        }

        if (color_comp == 0 && 0 != start_color) { // If currently yellow and start color is not yellow, break
            stop_motors();
            Serial.println("Detected Yellow, turning around");
            break;
        }
        else if (color_comp == 1 && 1 != start_color) { // If currently blue and start color is not blue, break
            stop_motors();
            Serial.println("Detected Blue, turning around");
            break;
        }
        else if (color_comp == 2 && hit_black > 0) { // If currently black, turn until no longer black
            break;
            stop_motors();
            Serial.println("Detect Black, finding color on right");
            hit_black = hit_black - 1;

            // couple local vars
            bool searching = true;
            int turns = 0;
            int turn_dir = 1;

            while (searching) {
                Serial.println("searching");

                if (turn_dir == 1) {
                    Serial.println("Searching right");
                    turn_right(); // turn set degree
                    _delay_ms(100);
                    stop_motors();
                    turns = turns + 1;      // Increment turns counter
                }
                else if (turn_dir == 0) {
                    Serial.println("Searching left");
                    turn_left();
                    _delay_ms(100);
                    stop_motors();
                    turns = turns + 1;
                }

                current_color = getColor(); // Get current color again
                
                if (current_color < black_threshold) {  // If we find a nonblack color, turn a little more and break
                    Serial.println("Found color");
                    if (turn_dir == 1) {
                        turn_right(); // turn set degree
                        _delay_ms(100);
                        stop_motors();
                    }
                    else if (turn_dir == 0) {
                        turn_left();
                        _delay_ms(100);
                        stop_motors();
                    }
                    searching = false;
                }
                else if (turns >= 4) {      // If we reach 90 degrees of turn, flip turning direction
                    Serial.println("Reach 90 degrees, flipping direction");
                    turn_dir = 0;
                    turns = 0;
                }
            }
        }

        drive_forward();
        Serial.println("moving");
        Serial.println(current_color);
        Serial.println(color_comp);
    }

    while(1) {
        stop_motors();
    }
}
```

<p><strong>Milestone 4: Cube Clearing </strong></p>

With the completion of milestone 3, the team was able to quickly complete milestone four on the same day. The objective of the task was to prove that we could collect a minimum of five block. To accomplish this, we made a pair of prototype arms out of cardboard and glued them to the robot frame. The milestone two code was modified so that the bot would simply move forwards and turn right, a behaviour that we kept for the final bot behvaiour with how simple and reliable it proved to be. We were able to quickly demonstrate our cpabilities and move on to designing the competition features themselves.

<p><strong>Gear Train Design </strong></p>

For the final design, I was in charge of creating the gear train. This involved making the gears and and method of mounting them to the provided robot frame. My main limitation in this process was the budget, I figured I only had around $15-$20 for the entire prototyping process and final product. A single gear off McMaster is at minimum $5, so if I bought all four gears I needed, I would be over budget already. I decided to then pivot to making my own gears by laser cutting them out of acrylic. This saved a lot of money, as for all four gears I only spent around $9. To make the gears, I used Fusion 360's gear generator add-in, and settled on a 3:1 ratio to maximize speed while minimizing the space the gear train would take up. 

After designing the gears, I moved onto the mounting system. Again, my main limitation was the budget as each 3D printed part cost us $1 as a base rate, and an additional $0.4 for each gram the part weighed. Because of that limitation, I decided to make the mounting piece as small as possible to save on cost, but the final cost per part still reached over $2. Furthermore, my design for the mount was flawed, and was not stiff enough. The main portion of the part is essentially a single canteliver beam, with the gear and wheel mounted on the very edge. This meant that when turning, the mounts tended to bend and cause meshing issues between the gears. To alleviate this issue, I thicked the walls in the model and changed the print settings to prioritize stiffness in the horizontal direction. These changes were successful, and the bot moved reliably.

![Milestone 3]({{ "/assets/images/Mechatronics Bot/Gear Mount.png" | relative_url }}){: .center-image}
![Milestone 3]({{ "/assets/images/Mechatronics Bot/Gear Train.png" | relative_url }}){: .center-image}

<p><strong>Competiton Day </strong></p>

On the day of competition, our bot proved to be formidable. The extra speed provided by the gear train allowed us to consistently reach the blocks faster than our oppoents. We managed to top our group with a 4-1-1 record, dropping only the final match due to an electrical failure at the start. This qualified us into the single elimination bracket, where we were knocked out in the sweet sixteen. Overall I am happy with how the gear train design turned out, especially with the limited budget. I would love to revisit this challenge with more resources sometime in the future.

