# Lab 9:

Lab 9 took me a lot longer than expected because one set of wheels on my robot was not working as intended. Although I was able to temporarily fix this, when I went to collect my data again, I realized that the wire connecting one of the motor drivers to the motor had completely come out (not on the motor driver side but on the motor side). This essentially meant that I had no control over one set of wheels, so in order to perform orientation control, I would need to somehow restrict those set of wheels from turning and only let the other set of wheels turn (thus the robot would rotate about the axis of the non-spinning set of wheels). Although this could have been solved mechanically (my jamming the other wheels), due to time constraints, I decided, for the purpose of this lab, to just hold those wheels in place while the other wheels rotated the robot around. Although this wasn't the best solution because it wasn't turning very well, I decided to continue with this temporary solution for the sake of understanding how mapping works even if it wouldn't be the most accurate map. I had also originally tried velocity control when both sets of wheels were working, but then, I was having issues with my controller itself (which I later fixed but my wheels were then not working). Overall, the issues with my motors meant that I didn't necessarily get the best results that I wanted, but I'm super thankful for Larry for hosting OH super last minute so that I could spend some time to at least try getting a map + Jonathan for letting me use the room!!

# PID Orientation Control

I first started with PID Orientation Control of the robot's one set of wheels to enable on axis turns. The code for it is listed below (because one set of wheels wasn't working, I changed it to only use P control for simplicity although this rendered likely a lot of steady state error that an integrator would've taken care of).

<img width="390" alt="Screenshot 2024-05-13 at 11 08 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/9a07b7f1-1c4e-46cd-9d23-aa3b96d4542a">


I had originally tried working with Velocity Control, but I realized that because the velocity wouldn't be super accurate, this was messing up my map. With Orientation Control, I created a for loop of each of the angles that I wanted the robot to be at (around 0.436 radians incrementally), and used a P controller to multiply the error of the setpoint by the KP value. I had a threshold for how big the error could be before it exited the inner while loop for error and incremented to the next angle that it would need to reach. At each setpoint, I took 10 ToF readings which I averaged such that I would have a sensor range reading for each sensor bearing. Code for this is shown below:

My sensor does have a lot of drift but not too much based on my IMU testing in earlier labs. Based on how much drift I found in the IMU lab, the average error is probably around 0.05 - 0.1 radians, and the maximum I found while testing in this lab was around 1 radian (which I corrected for by reducing the threshold of how much error was okay before changing the setpoint to the next angle in the full 360 degree rotation).

Here is a video of my robot's PID control (again I had to jam one set of wheels by turning it):

<iframe width="315" height="560"
src="https://youtube.com/embed/h0l8JLjHY2U"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Because of me having to jam one set of my wheels, I wasn't able to graph good TOF data showing that PID control worked, but I do have descriptions quantifying my data collection below.

After trusting that the data was collected at the correct radian values, I plotted the data using pyplot.polar:

Point 1 (-3, -2):

<img width="277" alt="Screenshot 2024-05-13 at 10 29 07 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/ec538555-6e7a-48f1-a943-20ed873af1a4">



Point 2 (0, 3):

<img width="299" alt="Screenshot 2024-05-13 at 10 29 13 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/dc04d99b-e885-4334-828e-f032e5eef6b9">


Point 3 (5, 3):

<img width="255" alt="Screenshot 2024-05-13 at 10 29 21 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/ae4a7e29-822c-465d-a938-906f38634bed">

Point 4 (5, -3):

<img width="308" alt="Screenshot 2024-05-13 at 10 29 27 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/15038ebc-5e74-4d90-98ed-77bec1dc8bfa">

The measurements are slightly off from what I expect. Such as in point 3, I don't see a full loop of data, but I attribute this to my robot observation data not being the best because it was spun off one axis.

Next, I computed the transformation matrices (x = rcos(theta), y = rsin(theta)) and converted the measurements to the inertial reference frame. To make sure that I had a common origin, I offset each measurement by the distance the point was from the origin.

<img width="539" alt="Screenshot 2024-05-13 at 10 37 33 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/8eba875f-799c-4d76-89d3-6e9ae6bafc95">

As can be seen, my graph isn't a great map, and I assume this is because my PID control wasn't as accurate due to me jamming the wheels. I especially notice very little precision for point (5, 3) which I assume is due to it being very close to a corner and then suddenly having a lot of open space which causes this high shift in data (I think that if I had better integration of my gyroscopic value, this would have been resolved).

Drawing a general trace around the map, I can get this general view of what the lab setup looks like:

<img width="434" alt="Screenshot 2024-05-13 at 11 05 03 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5673e565-0b62-4363-9fd4-c5898c9d1d2a">


My map tends to not fare well with the boxes placed in the middle of the ground but has a general overall trace of the arena. Because I was spinning off axis and so there was a little bit of a larger turn in my robot (i.e. it would not spin about its center), I think this caused some of my TOF sensor readings to be off because they were not taken via a consistent spin. I would have liked some more time to take apart my robot and access the bottom motor so that I could have taken more precise data to generate a better map, but I have a general understanding of how the mapping process works!

Another thing I noticed about my map was that it did pretty poorly in terms of mapping corners. Pretty much all of my corners were not right angles, and I think this is because the TOF sensor bounces a lot when it hits a corner. I also think that I should have changed my ranging which might have helped with some of the closer/farther measurements' precision. It would have been cool to also implement a controller on the type of sensor ranging to use depending on what types of TOF values are being received so that there could be more precision when there were obstacles closer versus farther away.

Here is a list of my points for the simulator:

<img width="376" alt="Screenshot 2024-05-13 at 11 07 57 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/09fc6582-502f-43dd-8cb9-32330dfeadce">


