# Lab 12:

As stated in previous labs, unfortunately, because my one set of my robot's wheels no longer turn, localization doesn't work correctly unless I mechanically jam one set of wheels. Unfortunately, this means that for Lab 12, my original plan of using localization to get from waypoint to waypoint does not work. Also note, that I left Ithaca for my internship prior to resolving this issue, so I created a set up of the lab space and tested out my robot there.

Instead, I opted to hard code in when the ToF sensor would detect obstacles to reach certain waypoints with PID control of the robot moving forward. Additionally this was difficult because my robot was no longer able to do turns, so I had to help the robot turn while it was navigating. If I had implemented a local path planning algorithm, I could have used Bug 0 for example. To do this, I would have mounted a sensor to the side and front of my robot. I would then enter a waypoint value to the robot (and also use orientation control to determine the orientation the robot would need to get to that waypoint). Then, the robot would traverse along obstacles found using the ToF sensor. Likely an issue here would be how fast the ToF data would update (where the obstacles were) as well as how well my orientation control actually worked (correct orientation to the waypoint), but these issues could be largely resolved using different ranging/update modes of the ToF and rigorously testing my orientation control with the waypoints.

# Running my robot

Because I was not in lab, I decided to tech demo this at home with the use of three walls. I measured ToF values that the robot would need to reach before me having to turn it and then it would continue moving forward to meet all three waypoints.

Open Loop Implementation:

<img width="220" alt="Screenshot 2024-05-16 at 10 30 57 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/c04b0e3d-f7fc-42c8-8d6b-d633b80fbdb9">

Closed Loop Implementation:
<img width="261" alt="Screenshot 2024-05-16 at 10 30 28 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/c6e27a82-246b-4e7d-bc73-67f42fffb7a4">

However, when I went to test this on my robot, I could hear one motor turning, but it was oversaturated because the friction of the other wheel being stationary on the floor of the room was too high for the robot to move forward. So unfortunately, I was unable to actually test my system on my robot :(.

I tried again with a PWM value of 255, but again, there was too much friction for one set of wheels, so the robot ended up just turning instead of moving forward in a straight line as can be seen in the video here:

<iframe width="315" height="560"
src="https://youtube.com/embed/6LASNBySUm8?feature=share"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Although it is dissapointing that I wasn't able to fully implement Lab 12 on my robot, it goes to show that hardware functionality is just as important as the theory that goes behind most robotics. Overall, I had a great time taking this class and have learned a lot!! Huge thank you to Jonathan + the TAs for making this class what it was!!
