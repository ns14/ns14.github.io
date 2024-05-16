# Lab 12:

As stated in previous labs, unfortunately, because my one set of my robot's wheels no longer turn, localization doesn't work correctly unless I mechanically jam one set of wheels. Unfortunately, this means that for Lab 12, my original plan of using localization to get from waypoint to waypoint does not work. Also note, that I left Ithaca for my internship prior to resolving this issue, so I created a set up of the lab space and tested out my robot there.

Instead, I opted to hard code in when the ToF sensor would detect obstacles to reach certain waypoints. I initially wanted to do this with PID control, but because one set of wheels wasn't working, I realized that open loop control would work. I'm a little disappointed that I wasn't able to fully implement the robot in Lab 12 the way I had initially planned, but here's a little snippet of what it can do:

Additionally this was difficult because my robot was no longer able to do turns. So instead here, I have attached the theory and code behind what I wanted to my robot to do although I don't have great videos of the robot actually implementing it.
