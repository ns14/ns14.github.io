# Lab 6:

## Prelab

I used the same methodology from Lab 5 to send and receive data over Bluetooth. Every time I send the Orientation Control command, after 5 seconds of trying Orientation Control, I report back to the computer IMU, Gyro, and PWM data to the computer for each time step that data was collected (this process is described in my Lab 5 report).

Here is an example of the Yaw Data being sent over Bluetooth to my computer:

<img width="405" alt="Screenshot 2024-03-27 at 9 44 39 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/eec00a2b-de08-4886-97b9-0e098183d438">


## PID Input Signal

I started my orientation control by integrating my gyroscope data to get an estimate of orientation for the robot. This process was pretty much derived from that done in the IMU lab. Recall that in Lab 2, the gyroscope seemed to have a lot of angular drift. Because of that, digital integration would integrate that error as well leading to more inaccurate results in angular position over time. Another issue with this integration is data not updating fast enough (due to a slower sampling rate) which would mean that the the angular position over time integrated from the angular velocity would be less accurate as well. To solve this, I minimized delays/Serial.prints() etc. as much as possible and considered adding a "drift" factor to my gyroscopic integration depending on how much drift I experimentally expected. For initial testing, I decided not to include a drift factor in my yaw integration to see how much of an error this would actually be before making the decision to use this.

<img width="342" alt="Screenshot 2024-03-27 at 9 44 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/9ae970ce-daad-4f16-95bb-b23db8b3ea27">

Lab Tasks
P/I/D discussion (Kp/Ki/Kd values chosen, why you chose a combination of controllers, etc.)
Range/Sampling time discussion
Graphs, code, videos, images, discussion of reaching task goal
Graph data should at least include theta vs time (you can also consider angular velocity, motor input, etc)
(5000) Wind-up implementation and discussion


