# Lab 6:

## Prelab

I used the same methodology from Lab 5 to send and receive data over Bluetooth. Every time I send the Orientation Control command, after 5 seconds of trying Orientation Control, I report back to the computer IMU, Gyro, and PWM data to the computer for each time step that data was collected (this process is described in my Lab 5 report).

Here is an example of the Yaw Data being sent over Bluetooth to my computer:

<img width="405" alt="Screenshot 2024-03-27 at 9 44 39 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/eec00a2b-de08-4886-97b9-0e098183d438">


## PID Input Signal

I started my orientation control by integrating my gyroscope data to get an estimate of orientation for the robot. This process was pretty much derived from that done in the IMU lab. Recall that in Lab 2, the gyroscope seemed to have a lot of angular drift. Because of that, digital integration would integrate that error as well leading to more inaccurate results in angular position over time. Another issue with this integration is data not updating fast enough (due to a slower sampling rate) which would mean that the the angular position over time integrated from the angular velocity would be less accurate as well. To solve this, I minimized delays/Serial.prints() etc. as much as possible and considered adding a "drift" factor to my gyroscopic integration depending on how much drift I experimentally expected. For initial testing, I decided not to include a drift factor in my yaw integration to see how much of an error this would actually be before making the decision to use this.

<img width="342" alt="Screenshot 2024-03-27 at 9 44 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/9ae970ce-daad-4f16-95bb-b23db8b3ea27">

I had a framework set up for my PID controller in the prelab. As can be seen, there are three gains (proportional, integral, and derivative), and after calculating a delta t and error, the proportional gain is applied to the error, the integral gain is applied to a discretized time step times the error (added to the previous error), and the derivative gain is applied to the change in error every time step.

The proportional control essentially affects the output of the system given some proportional constant to the error (i.e. if error is larger, the output will proportionally be affected). The integral gain however seeks to reduce the error by integrating over the error over time. Thus when the error is zero, the integral control will also remain constant indicating that the error has been removed between the set point and actual output (and proportional control). The derivative control "predicts" the future error by calculating what the change in error is over time and controls how much the error is changing over time (the faster the error is changing, the more derivative control there is). It was interesting reading more about these in the lecture slides as well as online to understand more fundamentally what each part of the controller does.

I had a busier schedule during this lab, so I decided to play around with just a P controller (I'm in the 4000-level class). I found the error between the set point and the yaw and then multiplied that by some constant KP. That was then inputted to run the motors. The code is shown below:

<img width="315" alt="Screenshot 2024-03-27 at 2 19 22 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/a871b5f4-4a4c-42c2-821b-7fa127f25c99">

I had a hard stop set after 1000 data points of yaw were collected in case there was an issue with Bluetooth.

I ended up having a huge issue with my motors. When I ran the control loop, I found that one of my motors would start turning and the other one wouldn't. I found that this was an issue in my calibration factor in that I needed to add some kind of calibration factor for both of them to move at the same actual speed (as we had done in the motor driver lab). To fix this, I played around with 
different PWM signals to make sure that both motors would move.

I also ran into the issue of my motors not overcoming the friction of the carpet in my apartment. I had initially set a really low KP value which meant that the PWM signal that I was inputting into the system was too low for the wheels to overcome the static friction from the carpet. To fix this, I played around with different KP values that would have the correct proportion of error to speed of the wheels. I ended up on a KP value of 5 that led to promising results.

However, one issue I noticed was that the robot seemed to move non-rotationally as well in between trying to reach setpoints. i.e. if I were to not just place it at a random yaw angle but rather move it's yaw angle, I noticed that this wouldn't necessarily lead to it trying to return to the setpoint directly but rather moving linearly and then returning to the setpoint eventually. To expedite this process up, I decided to play around with my controller. After trying a couple different set points, I realized that the issue in my controller was that the gyro data wasn't being integrated correctly. That's what was causing the speed of my motors to simply keep increasing and starting to move linearly. To fix this, I reassigned which pins would need the positive vs. negative 

<img width="68" alt="Screenshot 2024-03-27 at 5 59 27 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/fe117b47-f57a-465d-9266-fb90529063ed">

I started playing around with my gyro values (basically changing my ICs and playing around with the equation) and realized that my ICs were causing my gyro data to be off (I was accidentally setting the initial angular position to be the angular velocity from the gyro). After fixing this, my gyro data seemed to be more accurate:

<img width="96" alt="Screenshot 2024-03-29 at 3 49 01 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/d9d2a405-e36f-4c93-a775-eb901657b656">

I was having an issue with my motors at this point which I wasn't able to fix in time for this lab but when logging what PWM values were being returned, I could clearly see that as the angle from the setpoint increased, the motor values would continue to increase and as I moved the car closer to the setpoint, they would decrease. I also graphed these values to see how they responded. Here's an example:

Here is a video of my robot's response to a random setpoint value. As can be seen it seems to reach the setpoint, stops, and then when it loops again, returns back to the set point. 

<iframe width="315" height="560"
src="https://www.youtube.com/embed/2D20kxEkXE4"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>


