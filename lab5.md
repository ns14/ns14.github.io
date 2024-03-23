# Lab 5:

## Prelab

In the prelab, I worked on implementing processes that would make implementation of the linear PID control easiest.

To start, I created a command in my switch-case for Bluetooth that would enable the robot controller to start on an input from my computer sent over Bluetooth.

<img width="248" alt="Screenshot 2024-03-13 at 10 01 23 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/4829769c-d016-4f06-866f-e7a7b3b54c3a">

Next, I worked on implementing PID control over a fixed amount of time (I chose 5 seconds based on the pre lab) while storing debugging data in arrays.

<img width="253" alt="Screenshot 2024-03-23 at 10 59 25 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/12d727e6-1da3-49e4-9364-b0b9b8b39960">

For debugging after the PID controller ended, I decided to store TOF data with timestamps (i.e. how far the car was from the wall). I also sent data on what the PID controller was inputting to the motor drivers.

<img width="320" alt="Screenshot 2024-03-23 at 11 02 46 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/7a51511d-592f-4997-99a5-2d7552ba22a4">

I had a hard stop implemented directly on the Artemis that would turn off all the motors after 5 seconds of the car running (in case the PID controller failed because I expected it to take a maximum of 5 seconds for it to reach 1 foot away from the wall if it started at the farthest distance of 4 feet).

<img width="505" alt="Screenshot 2024-03-23 at 11 02 07 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/3adcf6bb-5527-4542-9b16-3345a3d445f7">

I didn't initially add a case to tune my gains, but after having to reprogram them a lot, I realized it would be faster to send a Bluetooth message to retune the gains to make the tuning process a lot faster than I originally had it.

# Pseudo Control

Before starting to implement my PID controller, I wanted to characterize how my ToF sensor sent data to my computer, and how my computer would control the car in response. I started with a pseudo-controller to understand how my ToF sensor worked. Specifically, I first calculated the error between the setpoint (1 foot) and the distance read by the ToF sensor and wrote a simple if statement that if the error was less than or equal to 0, the motors should stop spinning the robot's wheels. I then played around with moving obstacles in front of the ToF sensor to see how the wheels would move in response to that using simply my if statement.

<img width="279" alt="Screenshot 2024-03-13 at 10 00 48 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/2bb0444f-6268-4c77-ba19-db014627adc6">

Playing around with different obstacles and distances I found that there was a small lag time (as expected) between when the ToF would send distance data back from the Artemis to the computer slightly slower than how fast analogWrite would write the data to the wheels. I also realized that I had a lot of delays/Serial.println() statements that I was using for debugging/proper documentation that was slowing down my loop/effective sensor data rate. Because of this, I started removing some of my delays and removing some of my Serial.println() statements so that the effective data transfer rate from the ToF sensor to my computer would be faster.

# PID Control

After characterizing the data of the ToF sensors, I then started implementing my PID controller. I had a framework set up for my PID controller in the prelab. As can be seen, there are three gains (proportional, integral, and derivative), and after calculating a delta t and error, the proportional gain is applied to the error, the integral gain is applied to a discretized time step times the error (added to the previous error), and the derivative gain is applied to the change in error every time step.

The proportional control essentially affects the output of the system given some proportional constant to the error (i.e. if error is larger, the output will proportionally be affected). The integral gain however seeks to reduce the error by integrating over the error over time. Thus when the error is zero, the integral control will also remain constant indicating that the error has been removed between the set point and actual output (and proportional control). The derivative control "predicts" the future error by calculating what the change in error is over time and controls how much the error is changing over time (the faster the error is changing, the more derivative control there is). It was interesting reading more about these in the lecture slides as well as online to understand more fundamentally what each part of the controller does. For example, if the robot is moving fast towards the wall, the change in error is fast meaning the derivative control will influence the controller more.

<img width="249" alt="Screenshot 2024-03-22 at 1 44 52 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/36d98c11-a2fb-48ce-aee6-1bbd96acfdfc">

The motor input values can go from 0 to 255. I decided to use a KP of 0.12. That way if the error is 2000 mm, then that would equate to a full speed, and when the error reaches 304 mm, the motors would essentially stop (too low of a value for the wheels to actually turn).

So a good range of proportional gains would be from 0.06 to 0.12 because that is the ratio of the full speed to farthest ranging distance of the ToF sensor to the closest one that I plan on testing.

A long ranging mode would be better for the purposes of this because despite the lower accuracy, we want our robot to move at fast speeds, so we want a faster sampling time (20 ms/50 Hz). I also reduced the sensor integration time (to ensure independent measurements) to 4 (a value between 1 and 8).

Now that I had the PID framework set up, I sent time stamped sensor values and motor outputs so that I could graph the outputs of various PID control runs.

Here is an example of my first run with KP as 0.12:




LOG DATA: If you don’t want to repeat work during Lab 7, be sure to log all data (time stamped sensor values and motor outputs), as well as setup variables from at least one successful run. Even if you are doing orientation control, be sure to log ToF data as you speed towards the wall as well.
Documentation: Please clearly document the maximum linear or angular speed you are able to achieve (you can use your sensors to compute this). To demonstrate reliability, please upload videos of at least three repeated (and hopefully successfull) experiments.
Frequency: Fast loop times means everything to a good controller. Be sure to include a discussion of sensor sampling rate and how this affects the timing of your control loop. Avoid using blocking statements when you can (e.g. delay() or while(sensor not ready) ){wait} ). Also, remember that any serial.print/BLE sending that occurs during execution may slow down your loop time considerably.
Deadband: From Lab 5, you should have found the deadband of your motor (the region below which the power to the motors does not overcome the static friction in your system). Consider writing a scaling function that converts the output from your PID controller to an output for which the motors can actually react.
Wind up: If you include an integrator, consider whether you need to worry about integrator wind-up.
Derivative LPF: If you include a derivative, consider whether it is necessary to include a low pass filter in the derivative branch.
Derivative kick: Consider whether the derivative kick can cause any issues, given the task you choose. Here is a great overview on how to eliminate derivative kick: http://brettbeauregard.com/blog/2011/04/improving-the-beginner’s-pid-derivative-kick/
Motor drivers: Recall Lecture 6 on Actuators and that the motor drivers have both coasting and active breaking modes. These might come in handy.

# P Control
I first implemented

# PI Control

# PID Control

# Extrapolation

Fun things I was exploring:

I was looking into adaptive control: essentially, changing your controller's gains while the car is running. After talking with Dr. Jaramillo, I noticed that this wouldn't necessarily be useful for our application although what might be useful is using different sensor modalities. This is because different sensors have different modes that can be used depending on the ranging distances of the actual ToF sensor that are needed by the user. So, for example, I found that for the purposes of my ToF sensor, the closer I was getting, I might want a shorter ranging distance mode but as I was further away, I would want a longer ranging distance mode. The tradeoff here comes from having different accuracies; if I change my sensing mode, my sensor readings might become more or less accurate which can also affect how well my robot is working for the purposes of this lab.

For our purposes, having different sensor ranging modes might be helpful but adaptive control might not necessarily be because these are cheaper, less accurate sensors that we are using on our robot. After speaking with Dr. Jaramillo, where this would be useful would be for the purposes of if we were switching our robot from linoleum to carpet for example where there are different coefficients of friction which would impact how our robot was working. But, because for the most part our robots are going to be run on tile, this isn't a huge issue, and we don't really need adaptive control. It would be pretty fun though to try implementing this for some reason on a robot!!


