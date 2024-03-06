# Lab 4:

## Prelab

First, I had to wire up the motor drivers to the Artemis and robot. To do this, the relevant connections were the two motor drivers, the Artemis, the two motors, and the motor driver battery.

In terms of the pins to use, I had to select pins on the Artemis that were ideally on the same row (such that I could tilt the Artemis to place it in my chassis and all the wires would come out the same way) and pins that allowed PWM signals. After checking with the datasheet, I found that A8 and A10 do not allow PWM, so I selected pins A1, A2, A3, and A4 (with my TOF XSHUT pin being connected to A5 from the previous lab).

As discussed in the Batteries and Actuators lecture in class, having two batteries is better than one because the motor driver(s) and the Artemis have different power needs, and, so, the motor driver needing more power at times shouldn't disrupt the Artemis' power supply. Thus, having multiple batteries helps diverge this load from one battery to two and prevents the motor drivers' power needs from affecting the Artemis.

When determining wiring, I had to consider where all the various component would go into the chassis. For the motor drivers, I parallely connected each of the IN pins and OUT pins so that they would require only one wire going either to the Artemis or the motor. I also decided to use shorter wire as I assumed the motor wires would be close together in the chassis and also close to the Artemis, so I would place both of those in the small section closest to the motor connections. The IMU could then lie flat closely to the Artemis (using a small QWIIC) on the battery pack back and the TOF (with the longer QWIIC) could lie on either side of the long side of the robot. I color coded each parallel pin joint a different color so that I could easily code each in/out pin based on which color it was and which color wire it was soldered to on the Artemis/motor wire. I also made sure to not use solid core wire so that there was better chance that the soldering/wiring would survive (for longer) possibly stunts/high accelerations.

A wiring diagram of my new connections is given here:

<img width="425" alt="Screenshot 2024-03-06 at 7 27 34 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/ee05de1e-5543-4fb6-914d-d08e8bd1a8fb">

## Connecting one Dual Motor Driver/ Oscilloscope

The first step here was connecting one of the motor drivers. To do this, I soldered the in/out pins in parallel and then soldered lead wires from the Vin pin and one GND pin. The other GND pin, I soldered to GND on the Artemis.

Because initially I was connecting the Vin and one of the GND pins to the power supply, I had to determine valid settings for the power supply. I ended up choosing 3.7 V as our battery provides so not to overpower the motor driver. I also checked the datasheet for the motor driver and saw that it has an operating voltage range from 2.7 V and 10.8 V with built in protection against reverse/under voltage and over current/temperature, so I could have picked any of those voltage values. Essentially, the power supply should fall within the specs of the motor driver as that is what it is powering. I chose 3.7 V to be consistent with the battery.

To visually inspect a PWM signal being sent through the motor driver, I used analogWrite() to send PWM signals through each OUT pin on the motor driver and hooked it up to the oscilloscope to confirm that indeed a PWM signal has been provided. When connecting to the oscilloscope, it was important for me to remember to connect my GNDs (from the Artemis and the power supply).

Below is a video of my power supply, motor driver/Artemis, and oscilloscope set up as well as visual results of my oscilloscope when I sent a PWM signal through it.

<iframe width="560" height="315" src="https://www.youtube.com/embed/K3d5H_L_WIk?si=vDmBXG5EYvV2CI7k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/E_MGlyWWbL4?si=MERbIYsR-mNB3Wug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Here are the commands I used to generate the PWM signal:

<img width="145" alt="Screenshot 2024-03-05 at 10 35 20 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/bac336f4-26fb-4350-a419-84ac3c188811">

## Spinning Wheels

The next step was for me to connect the motor driver to the actual motor on the car. To do so, I unscrewed the main chassis of the car and cut the control PCB soldered wires of one of the motors of the car wheels. I then soldered each of the wires to each of the parallely-connected OUT pins on the motor driver. I did not yet connect the battery power to the Vin and GND pins as I still wanted to test the motor driver through the power supply. I also cut the wires for the LEDs as we were not using them for future labs either. 

I then tested that the wheels worked by sending a PWM signal and checking that the wheels spun in both directions. To check both directions of the wheel spinning, I used the code below and switched which OUT pin had a non-zero duty cycle to switch the direction of the wheels spinning.

Here is a screenshot of the code I used:

<img width="173" alt="Screenshot 2024-03-05 at 10 35 11 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5fe9f874-7cf0-4d3d-bae3-c283335db966">

Here is a video of the wheels spinning:

<iframe width="315" height="560"
src="https://youtube.com/embed/tdYq7KYseXE?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

<iframe width="315" height="560"
src="https://youtube.com/embed/wkq_iLD99u0?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

I then tested the motor driver with the 850mAh battery externally connected and saw that the wheels ran the same. Initially, they seemed to be running extremely slow and inconsistently, but I found this was becasue I was using stranded wire and simply needed to twist it more so that the battery connection was strong enough for current to go through (thank you Stephan Wagner for helping debug this!)

I repeated this for the other motor driver/motor/set of wheels, and the results are shown below as well (this time with the battery to show that I tested with the power supply and battery):

<iframe width="315" height="560"
src="https://youtube.com/embed/BZxPAVF9x4k?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

<iframe width="315" height="560"
src="https://youtube.com/embed/DQzzPMtw5QE?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

I then cut the battery connection in the chassis and soldered both Vin pins to the positive terminal and both GND pins to the negative terminal of both motor drivers. I then ran the code again to see if all four wheels would spin.

Here is a video of all four wheels spinning when connected to the battery:

<iframe width="315" height="560"
src="https://youtube.com/embed/bFf9sRoTS3w?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

## Car Chassis

After having soldered everything, I began arranging everything in my chassis. This proved to be a slightly messy process as my wire lengths were not entirely consistent with each other, and I had some wires tangling for the motor drivers. After playing around with a couple different placements, I decided that the motor drivers, Artemis battery, and Artemis would go in one half of the car and the IMU/TOF would extend from there. I temporarily used tape to secure my components (for easily removal) although would like to resolder some components in the future and more permanently secure them. I put the TOF sensors on the outside of the car on the longest edges because I had used the longer QWIIC wires that enabled this. I also faced the power port on the Artemis towards the outside of the car so that I could easily access the battery for the Artemis and unplug and replug it in. Here is a labelled picture of my final chassis setup:

<img width="593" alt="Screenshot 2024-03-06 at 7 39 50 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/2fd0cf38-e5b8-4f3c-8be3-dc3f2984f3ee">

Initially, when testing the car, I noticed that the batteries would fall out and (being one of the heavier components) push the casing out as well (I had initially screwed it very loosely so that I could easily access the inside of the car chassis). This was leading to the power for the Artemis being inconsistent causing my car to shut down in the middle of running as well as the power to the motor drivers being inconsistent due to the motor drivers losing power halfway. At first, I was concerned that my motors weren't able to turn the wheels given my PWM signal inputs but later realized this was just because my power connection was not strong enough. To fortify it, I added a lot of tape to make sure the batteries and their connectors stayed in place. In the future, I'd like to maybe find another set of wires to solder onto the batteries and then connect those to the Artemis/motor drivers because the current connectors I have seem to be loose and falling out. I'd also like to better secure my motor drivers in the chassis. 

## PWM Lower Limit

After setting up my car chassis, I explored the lower PWM for the car. To do this for the forward case, I started the car at rest on my table and then incrementally (in steps of 10), increased the duty cycle input to the motor driver until the car would move forward consistently. While doing this, I found that for one set of wheels, the minimum PWM signal would be 45 while for the other it would be 50. I then tried this with turning the wheels on-axis starting at 45 and incrementing upwards. I noticed that 45 was not enough to turn because there was now rotational friction as well from the treads not being parallel that had to be overcome too. As I incremented, the lowest PWM signal at which the car turned counterclockwise was 120 for both sides.

Videos of the low PWM signal range are shown below:

Forward:

<iframe width="315" height="560"
src="https://youtube.com/embed/h1_rGor8GJU?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

<iframe width="315" height="560"
src="https://youtube.com/embed/-cQldKW5-so?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Turn:

<iframe width="315" height="560"
src="https://youtube.com/embed/WUqMgcLLqS0?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

<iframe width="315" height="560"
src="https://youtube.com/embed/O5HglZGGGe4?si=Xdg8ujI9ruq_b1QH"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Code I used to test the low PWM signal was the same as the oscilloscope code but for forward I had the first OUT pin set to a high duty cycle and the second set to 0 for each motor driver, and for turning, I set one OUT pin to a high duty cycle on one motor driver and the rest to 0.

## Motor Calibration

I had found this in the previous labs trying to record stunts and noticed this time as well that my car did not drive straight. To fix this, I attempted to find a calibration factor that would set PWM singals that would essentially drive both my wheels at the same speed such that my car would not veer to one side. When I initially tested, I noticed that my car would veer to the left as can be seen here:

<iframe width="315" height="560"
src="https://youtube.com/embed/TQpiybAg968?si=rdVVk0ki01LskjAH"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

So, I tested decreasing the PWM duty cycle input to the right wheels until it straightened out. Initially, I had tried turning one of the motors completely off (commented out in my code below) slightly earlier than the other one because it seemed to spin for longer even after a PWM of 0 was sent to both. However, this simply caused it to do an axis-turn and veer more, so I decided to remove this and only focus on calibration.

For a PWM signal of 85 to the first motor driver, the second would need to be at 95. Thus, the calibration factor is 95/85 from one motor driver to the next or about 1.12. To test this, I used the same code as the one above of driving the car forward and simply played around with keeping the PWM constant for the first motor driver and changing in increments of 20 at first and then decreasing to 5 the second motor driver's PWM (code of the calibration shown below):

<img width="167" alt="Screenshot 2024-03-06 at 2 37 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/ee1277cb-b8b1-4e10-8af1-e6b3b762c1cc">

A video of it going straight on 6 ft of measured tape is shown below:

<iframe width="315" height="560"
src="https://youtube.com/embed/IEHeZ-yyOZw?si=i5injQ4QK-IeckwU"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

## Open Loop Control

The last part of this lab was to demonstrate open loop, untethered control of the robot. To do this, I wrote a couple commands for the robot to move forward, backwards, and make wide turns. I then randomly called these commands and saw the results in the robot moving.

Screenshots of the code for my functions is shown here:

<img width="619" alt="Screenshot 2024-03-06 at 7 49 33 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/4cdd2ffa-aa08-4ac6-8b4b-7d360f590a09">

<img width="624" alt="Screenshot 2024-03-06 at 7 49 46 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/41fcf8ac-eba2-475e-af9d-d1219392e16a">

<img width="625" alt="Screenshot 2024-03-06 at 7 50 01 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/df3dcc9c-4d12-4de6-9de9-fbfd9b4fa42b">

A video of my robot moving is shown here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Af_mPwpI5Cg?si=YV-WqdrjjRfEEmFh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>




