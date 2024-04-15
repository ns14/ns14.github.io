## Lab 8:

# Task A: Position Control 

I decided to perform a flip (position control) stunt. Just a note: I did this lab in my apartment which has carpet and so despite setting the PWM value to a maximum of 255; because there was a lot of friction with the carpet, my robot did not actually "flip" but rather abruptly stopped and reversed.

I disengaged my PID control and started playing around with various "setpoints" for when the car would need to abruptly stop and turn around. With an initial underestimate of 300 mm, I found that the car would run into the wall or start accelerating at a different angle.

I then changed the value in the if statement to closer to 500 mm and was successful in the car not hitting into the wall/accelerating in different directions.

The next issue I faced was having problems was my car turning back at an angle. To fix this, I played around with different starting points and how the speeds of my motors related to each other. I eventually realized this was being caused by a piece of tape stuck to one of my wheels giving it a frictional advantage and causing my robot to spin/turn at an angle instead of in a straight line. Removing this piece of tape helped ensure that the robot would move in a straight line.

Another issue I had was my ToF data updating too slowly. Initially, I was waiting for a new ToF reading before running the motors, but this was too slow for my robot to run into the wall at full speed. To fix this, I, instead of waiting for the ToF to have new data, kept ranging (which would lead to some duplicate values), but this ensured that my motor wouldn't stop too late and rather stopped when the distance was below 0.5 m.

As can be seen, I also had to increase the speed so that the robot would actually return past the initial starting point as well!

Here are three of my most successful runs!

Best One!

<iframe width="315" height="560"
src="https://youtube.com/embed/YjiH-vMhOXs?feature=share"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Another One!

<iframe width="560" height="315" src="https://www.youtube.com/embed/8MhfNe9wVGY?si=l8o8TlK7bx_ffiUm" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Last One!

<iframe width="560" height="315" src="https://www.youtube.com/embed/CJ4IAuA-Vwk?si=RrmtirdHCQTmhwSC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Following that are graphs of ToF data and motor speed for two consecutive runs (the last run I did not save the data for):

<img width="451" alt="Screenshot 2024-04-15 at 1 46 23 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/4f92953b-c5c5-4c3f-9bdf-234f0fa94cd3">

Here's some bloopers!

The robot crashing on beat to MJ:

<iframe width="315" height="560"
src="https://www.youtube.com/embed/avx0OGhAYb4"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Another Blooper:

<iframe width="315" height="560"
src="https://youtube.com/embed/DhSSLVrbx4o?si=HgfE4FDVUt0PhV2-"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

No Idea What Happened Here:

<iframe width="315" height="560"
src="https://youtube.com/embed/5ISr6gWtmbQ?si=hRT4AmNUr9Zrdkvg"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>


This was a fun lab to complete, and I learned more about little nuances in the way my robot works (such as motor calibration, ToF Data Accuracy, etc.!)
