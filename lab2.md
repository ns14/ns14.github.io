# Lab 2:

Step 1 in Lab 2 was to install the SparkFun 9DOF IMU Breakout - ICM 20948 - Arduino Library, connect the IMU to the Artemis board using QWIIC cable, and run Example1_Basics to see if the IMU could send data to the Serial monitor.

I was able to do this successfully but found that the data was not being sent. This was because by AD0_VAL was set to 1 instead of 0. This was necessary to change because.

Exploring how the accelerometer and gyroscope data changed as I rotated, flipped, and accelerated the 

I then added a visual indication of the LED blinking three times on startup to indicate when the IMU would start collecting data for the rest of the lab.

In order to convert the accelerometer data into pitch and roll, I used the equations from class. Here, it was important that I used atan2() instead of atan() to ensure that the output would be between [-90, 90] (i.e. the correct quadrants were being used while receiving the roll and pitch data).

My data seemed fairly accurate with not too much drift as I was, on average, approximately 0.001 off from -90, 0, and 90. I used two-point calibration to detetrmine a conversion factor to ensure that the final output matched the expected output.


Next, I worked on deciding how much noise existed in the accelerometer data. I initially had left the accelerometer steady and tried to conduct the Fourier transform of that data. However, after talking with my classmate, Stephan Wagner, I noticed that this would not allow me to extract a frequency of my accelerometer signal. My Fourier transform was showing a peak amplitude at the value of 0 Hz (because my accelerometer was stationary). To fix this, I then instead recollected data by moving my accelerometer sinusoidally (i.e. rolling/pitching/tilting it back and forth at an approximately constant frequency).

After recollecting the data, I then 


Step 1 in Lab 1 was to install the Arduino IDE and follow the setup instructions to install the necessary board and libraries. To test that the board was successfully connected to my computer, I used Example: Blink it Up. This example caused the LED on the board to blink on and off which is useful in the future to denote when the Artemis is on (such as on boot up).

<iframe width="315" height="560"
src="https://youtube.com/embed/WW7X_P9v2Ms?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

Following this, I followed the Example: Serial. This allowed me to type in character inputs and then receive those same character outputs in the Serial monitor. This would be useful for later parts of the lab when I would need to use the Serial monitor to receive temperature readings from the board.

<iframe width="315" height="560" src="https://youtube.com/embed/iQT44lFqC1w" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

The first sensor was tested using Example2_analogRead which was the temperature sensor. As can be seen in the video, as I pressed onto the sensor (to warm it up), the temperature reading in the Serial monitor increased. As I waved the sensor around (i.e. causing it to cool down with the cool air), the temperature reading in the Serial monitor decreased.

<iframe width="315" height="560" src="https://youtube.com/embed/iQT44lFqC1w" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen> </iframe>

Lastly, I tested the microphone using Example1_MicrophoneOutput. I used an online note player to see how the frequency of the sound would change. I also then used a music video to see how the microphone would return frequencies that had a lot of noise. It was interesting to see that the microphone was able to discern out the highest frequency noises from the jazz music which actually consisted of a medley of frequencies.

<iframe width="315" height="560" src="https://youtube.com/embed/DXHieDjCzVo?feature=shared" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen> </iframe>

<iframe width="315" height="560" src="https://youtube.com/embed/P9mhESTJ3GM?feature=shared" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted media; gyroscope; picture-in-picture; web-share" allowfullscreen> </iframe>
