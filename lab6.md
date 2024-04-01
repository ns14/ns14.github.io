# Lab 6:

## Prelab

I used the same methodology from Lab 5 to send and receive data over Bluetooth. Every time I send the Orientation Control command, after 5 seconds of trying Orientation Control, I report back to the computer IMU, Gyro, and PWM data to the computer for each time step that data was collected (this process is described in my Lab 5 report).

Here is an example of the Yaw Data being sent over Bluetooth to my computer (I added in timestamps after; can be seen in my IMU lab!):

<img width="405" alt="Screenshot 2024-03-27 at 9 44 39 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/eec00a2b-de08-4886-97b9-0e098183d438">

## PID Input Signal

I started my orientation control by integrating my gyroscope data to get an estimate of orientation for the robot. This process was pretty much derived from that done in the IMU lab. Recall that in Lab 2, the gyroscope seemed to have a lot of angular drift. Because of that, digital integration would integrate that error as well leading to more inaccurate results in angular position over time. Another issue with this integration is data not updating fast enough (due to a slower sampling rate) which would mean that the the angular position over time integrated from the angular velocity would be less accurate as well. To solve this, I minimized delays/Serial.prints() etc. as much as possible and considered adding a "drift" factor to my gyroscopic integration depending on how much drift I experimentally expected. For initial testing, I decided not to include a drift factor in my yaw integration to see how much of an error this would actually be before making the decision to use this.

Orientation Determination and Bluetooth to Computer:

<img width="342" alt="Screenshot 2024-03-27 at 9 44 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/9ae970ce-daad-4f16-95bb-b23db8b3ea27">

Reading the data sheet for the gyroscope, I found that the max rotational velocity (degrees per second) the gyroscope can range is +/- 2000 dps which seems largely sufficient for our applications. Other possible limitations include the precision of our sensor: When leaving the sensor stationary, I saw that the values of the yaw was changing +/-2 degrees despite no rotational velocity. This was probably caused by gyro drift that I accounted for above. In terms of sampling rate, after removing unnecessary delays/Serial print statements, I was able to sample data every 3 milliseconds consistent with my findings from the IMU lab.

<img width="771" alt="Screenshot 2024-04-01 at 3 31 17 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/fb62c250-376b-4e88-9093-41cc002132ff">

I had a framework set up for my PID controller in the prelab. There are three gains (proportional, integral, and derivative), and after calculating a delta t and error, the proportional gain is applied to the error, the integral gain is applied to a discretized time step times the error (added to the previous error), and the derivative gain is applied to the change in error every time step.

The proportional control essentially affects the output of the system given some proportional constant to the error (i.e. if error is larger, the output will proportionally be affected). The integral gain however seeks to reduce the error by integrating over the error over time. Thus when the error is zero, the integral control will also remain constant indicating that the error has been removed between the set point and actual output (and proportional control). The derivative control "predicts" the future error by calculating what the change in error is over time and controls how much the error is changing over time (the faster the error is changing, the more derivative control there is). It was interesting reading more about these in the lecture slides as well as online to understand more fundamentally what each part of the controller does.

I had a busier schedule during this lab (and my battery connectors broke while I was working on this lab), so I decided to play around with just a P controller (I'm in the 4000-level class). I found the error between the set point and the yaw and then multiplied that by some constant KP. That was then inputted to run the motors. The code is shown below:

<img width="315" alt="Screenshot 2024-03-27 at 2 19 22 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/a871b5f4-4a4c-42c2-821b-7fa127f25c99">

I had a hard stop set after 1000 data points of yaw were collected in case there was an issue with Bluetooth.

I ended up having an issue with my motors not turning simultaneously. When I ran the control loop, I found that one of my motors would start turning and the other one wouldn't. I found that this was an issue in my calibration factor in that I needed to add some kind of calibration factor for both of them to move at the same actual speed (as we had done in the motor driver lab). To fix this, I played around with different PWM signals to make sure that both motors would move. Here's a video of their being a slight delay in the motors spinning (without control loop):

<iframe width="315" height="560"
src="https://www.youtube.com/embed/Gu5FuG36Wik"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

I also ran into the issue of my motors not overcoming the friction of the carpet in my apartment. I had initially set a really low KP value which meant that the PWM signal that I was inputting into the system was too low for the wheels to overcome the static friction from the carpet. To fix this, I tuned by KP value until it would have the correct proportion of error to speed of the wheels. I ended up on a KP value of 5 that led to promising results (in this video, you can hear the motor drivers turning the motors but them saturating due to the KP value!):

<iframe width="315" height="560"
src="https://www.youtube.com/embed/Iila5h8sR_s"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

However, one issue I noticed was that the robot seemed to move non-rotationally as well in between trying to reach setpoints; i.e., if I were to not just place it at a random yaw angle but rather move it's yaw angle, I noticed that this wouldn't necessarily lead to it trying to return to the setpoint directly but rather moving linearly and then returning to the setpoint eventually. To expedite this process up, I decided to play around with my controller. After trying a couple different set points, I realized that the issue in my controller was that the gyro data wasn't being integrated correctly due to an incorrect initial condition of the yaw. That's what was causing the speed of my motors to simply keep increasing and starting to move linearly. To fix this, I reassigned which pins would need the positive vs. negative speed (I had them both at the positive speed). I started playing around with my gyro values (basically changing my ICs and playing around with the equation) and realized that my ICs were causing my gyro data to be off (I was accidentally setting the initial angular position to be the angular velocity from the gyro). After fixing this, my gyro data seemed to be more accurate:

<img width="96" alt="Screenshot 2024-03-29 at 3 49 01 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/d9d2a405-e36f-4c93-a775-eb901657b656">

<img width="68" alt="Screenshot 2024-03-27 at 5 59 27 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/fe117b47-f57a-465d-9266-fb90529063ed">

I was having an issue with my motors (again!), but when logging what PWM values that were being returned, I could clearly see that as the angle from the setpoint increased, the motor values would continue to increase and as I moved the car closer to the setpoint, they would decrease (i.e. what I expect from a proportional controller). Because I was having motor issues, I tested this by simply sending PWM data instead of seeing the robot move and graphing these values to see what the results would be. I also graphed these values to see how they responded. Here's an example:

To tune the KP controller, because it is a proportional constant, I essentially figured what the maximum angular displacement should be for fast differential drive vs. slow differential drive. I.e. to prevent overshoot (and possible saturation of the motors), if I was 1 degree off, I would want a much slower change in angular velocity than if I were 20 degrees off. 

As discussed above, initially I set this value TOO low (0.05) because I was worried the motors would oversaturate. However, this would essentially mean that the robot's wheels would not turn at all (which I later realized)! In the motor driver lab, we had found the lowest possible PWM signal the motors would need to be sent in order for the wheels to overcome static friction. I then changed my gain to 5. I noticed that there wasn't too much overshoot, but to minimize overshoot, I would have implemented a derivative controller.

I then implemented the controller. Lastly, here is a video of my robot reacting to a given setpoint. The controller loops for time steps. So, the robot moves, stops and enters the control mode, fixes orientation and keeps doing that a couple times as seen in this video: 

<iframe width="315" height="560"
src="https://www.youtube.com/embed/2D20kxEkXE4"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

## Programming Implementation
In terms of programming implementation, the controller can take in new values of the setpoint via Bluetooth commands while the controller is running. Reading any strings that are sent via Bluetooth allows the controller to update its setpoint each step (or the PID gains). For navigation/stunts, it would be helpful to do this as the setpoint or needed orientation of the robot rapidly changes as it is performing stunts. Although it wasn't required to control the orientation driving forwards/backwards, my robot is able to move and reset its orientation if need be as shown in the video above! I can make this more robust with a full PID controller (explained in the Future Work section below) and running this loop simultaneously with motion commands.

## Future Work

If I had more time, I would like to implement a derivative and integral controller as well. The derivative controller would be helpful in reducing overshoot because it would adjust the controller according to how the error was changing over time. Integral control would also be helpful in terms of removing any steady state error.

Analytically, it initially doesn't make sense to me to take the derivative of the signal that's already an integral of another signal (I could then just use the angular velocity readings to calculate KD). Maybe, however, usingn Kd on the derivative would make sense and account for the integration of any error coming over time from the yaw readings as well as ICs.

Changing the set point while the derivative controller is running would cause issues in that if there is a sudden change in the setpoint, the change in error for the derivative control would be really large. I.e. if I change the setpoint from 0 degrees to 100 degrees, I would suddenly have a really fast change in error. This would cause a disproportionately large value to be sent to the controller which could possibly saturate the wheels. To fix this, the error could be incrementally changed (for example, if I'm going from 0 degrees to 359 degrees, the change in error could be made 2 degree (clockwise) vs. 358 degrees (counterclockwise). Therefore, for large errors, the error could be made incrementally smaller which would help resolve this issue. A lowpass filter would also be extremely helpful before the derivative term to makes sure that a very large value isn't being sent in as a change in error.

In the case that I implemented integral and derivative control as well, I was looking up various procedures to tune the controller and found that one possible solution would be the Ziegler-Nichols method which sets an ultimate gain (Ku) and then tunes Kp, Ki, and Kd relative/proportional to Ku. This method would be interesting to implement. As explained in Lab 5, I was also exploring implementing adaptive controller where the values of the gains would change throughout the implementation of the controller based on either shifts in system dynamics or system parameters/changing environments (i.e. switching the coefficient of friction of the floor suddenly to a higher value would result in an increased P). I didn't have time to implement the adaptive control, but that might be a fun task to do in the future for the robot!
