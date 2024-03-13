# Lab 5:

## Prelab

In the prelab, I worked on implementing processes that would make implementation of the linear PID control easiest.

To start, I created a command in my switch-case for Bluetooth that would enable the robot controller to start on an input from my computer sent over Bluetooth.

<img width="248" alt="Screenshot 2024-03-13 at 10 01 23 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/4829769c-d016-4f06-866f-e7a7b3b54c3a">


Next, I worked on implementing PID control over a fixed amount of time (I chose 5 seconds based on the pre lab) while storing debugging data in arrays. I had a hard stop implemented directly on the Artemis that would turn off all the motors after 5 seconds of the car running (in case the PID controller failed because I expected it to take a maximum of 5 seconds for it to reach 1 foot away from the wall if it started at the farthest distance of 4 feet).

# Pseudo Control

Before starting to implement my PID controller, I wanted to characterize how my ToF sensor sent data to my computer, and how my computer would control the car in response. I started with a pseudo-controller to understand how my ToF sensor worked. Specifically, I first calculated the error between the setpoint (1 foot) and the distance read by the ToF sensor and wrote a simple if statement that if the error was less than or equal to 0, the motors should stop spinning the robot's wheels. I then played around with moving obstacles in front of the ToF sensor to see how the wheels would move in response to that using simply my if statement. A video of how the ToF sensor would read  distances and how the motors would respond is shown below:

<insert-video-here>

<img width="279" alt="Screenshot 2024-03-13 at 10 00 48 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/2bb0444f-6268-4c77-ba19-db014627adc6">

Playing around with different obstacles and distances I found that there was a small lag time (as expected) between when the ToF would send distance data back from the Artemis to the computer slightly slower than how fast analogWrite would write the data to the wheels. I also realized that I had a lot of delays/Serial.println() statements that I was using for debugging/proper documentation that was slowing down my loop/effective sensor data rate. Because of this, I started removing some of my delays and removing some of my Serial.println() statements so that the effective data transfer rate from the ToF sensor to my computer would be faster. On the other hand, I also wanted to find out if the 

Fun things I was exploring:

I was looking into adaptive control: essentially, changing your controller's gains while the car is running. After talking with Dr. Jaramillo, I noticed that this wouldn't necessarily be useful for our application although what might be useful is using different sensor modalities. This is because different sensors have different modes that can be used depending on the ranging distances of the actual ToF sensor that are needed by the user. So, for example, I found that for the purposes of my ToF sensor, the closer I was getting, I might want a shorter ranging distance mode but as I was further away, I would want a longer ranging distance mode. The tradeoff here comes from having different accuracies; if I change my sensing mode, my sensor readings might become more or less accurate which can also affect how well my robot is working for the purposes of this lab.

For our purposes, having different sensor ranging modes might be helpful but adaptive control might not necessarily be because these are cheaper, less accurate sensors that we are using on our robot. After speaking with Dr. Jaramillo, where this would be useful would be for the purposes of if we were switching our robot from linoleum to carpet for example where there are different coefficients of friction which would impact how our robot was working. But, because for the most part our robots are going to be run on tile, this isn't a huge issue, and we don't really need adaptive control. It would be pretty fun though to try implementing this for some reason on a robot!!

# P Control

After characterizing the data of the ToF sensors, I then started implementing my P controller (I started with P control as it would be the easiest to implement, and I could then build on it).

To control it, I started off with a very small gain to see how it would cause my system to react. I chose a KP value of 5. I noticed that this value (as expected) was extremely small and realized I needed to increase my gain for this to work as needed for this lab.

# PI Control

# PID Control

# Extrapolation


