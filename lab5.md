# Lab 5:

## Prelab

In the prelab, I worked on implementing processes that would make implementation of the linear PID control easiest.

To start, I created a command in my switch-case for Bluetooth that would enable the robot controller to start on an input from my computer sent over Bluetooth.

<insert-image-here>

Next, I worked on implementing PID control over a fixed amount of time (I chose 5 seconds based on the pre lab) while storing debugging data in arrays. I had a hard stop implemented directly on the Artemis that would turn off all the motors after 5 seconds of the car running (in case the PID controller failed because I expected it to take a maximum of 5 seconds for it to reach 1 foot away from the wall if it started at the farthest distance of 4 feet).

# Pseudo Control

Before starting to implement my PID controller, I wanted to characterize how my ToF sensor sent data to my computer, and how my computer would control the car in response. I started with a pseudo-controller to understand how my ToF sensor worked. Specifically, I first calculated the error between the setpoint (1 foot) and the distance read by the ToF sensor and wrote a simple if statement that if the error was less than or equal to 0, the motors should stop spinning the robot's wheels. I then played around with moving obstacles in front of the ToF sensor to see how the wheels would move in response to that using simply my if statement. A video of how the ToF sensor would read  distances and how the motors would respond is shown below:

<insert-video-here>

<insert-images-here>

Playing around with different obstacles, I found that 

# P Control

After characterizing the data of the ToF sensors, I then started implementing my P controller (I started with P control as it would be the easiest to implement, and I could then build on it).

