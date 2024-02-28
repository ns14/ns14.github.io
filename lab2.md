# Lab 2:

## Setup IMU
Step 1 in Lab 2 was to install the SparkFun 9DOF IMU Breakout - ICM 20948 - Arduino Library, connect the IMU to the Artemis board using QWIIC cable, and run Example1_Basics to see if the IMU could send data to the Serial monitor. Here is an image of my IMU being connected to the Artemis board:

<img width="500" alt="Screenshot 2024-02-22 at 2 26 15 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/4f780437-6a62-4b6c-b142-7bf52eafe1d1">

I was able to do connect the IMU successfully but found that the data was not being sent. This was because by AD0_VAL was set to 1 instead of 0. This was necessary to change to 0 because the ADR jumper was soldered together (after reading up on this online, I found that AD0_VAL is the last bit of the I2C address and that when the ADR jumper is closed, this value is 1).

<img width="400" alt="Screenshot 2024-02-22 at 11 46 21 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5324a0a3-96b1-4295-90ee-8fda79a4c00a">

## Analyzing Accelerometer and Gyroscope Data

Next, I explored how accelerating the IMU would affect the accelerometer and gyroscope readings:

<iframe width="315" height="560"
src="https://www.youtube.com/embed/0JXrrpiR4Oc"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

After playing around with moving the IMU, I found that the gyroscope (measuring angular velocity) would read a value anytime the IMU was rotating whereas the accelerometer would read values where the IMU was translationally moving. The gyroscope would pick up on some angular velocity even when the IMU was "translationally" moving because I realized that I had held it up in the air causing some angular velocity as well in the sensor. The magnetometer values remained largely constant (and very small) as I was moving my board around.

I was initially confused as to why there was a separate "scaled accelerometer" reading and how that value differed from the raw accelerometer reading. After looking through the example code, I found that this was because this data was taken relative to g which would be important when I was calculating my pitch and roll later in the lab.

## Adding visual indication of LED blinking

I then added a visual indication of the LED blinking three times on startup to indicate when the IMU would start collecting data for the rest of the lab.

<iframe width="315" height="560"
src="https://youtube.com/embed/QKjdkMUVRYQ"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

## Accelerometer Data

In order to convert the accelerometer data into pitch and roll, I used the equations from class. Here, it was important that I used atan2() instead of atan() to ensure that the output would be between [-90, 90] (i.e. the correct quadrants were being used while receiving the roll and pitch data).

<img width="438" alt="Screenshot 2024-02-22 at 2 24 43 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5815c4c8-c6aa-4173-a48c-cbf9e3dcf501">

When testing {-90, 0, 90} degrees for the roll and pitch by setting my accelerometer against a flat edge, I got these values calculated for the roll and pitch. As can be seen, the values were largely accurate except as they got closer to -90 degrees and 90 degrees. I think this is because the table I was using has a slight curvature to it, so the accelerometer wasn't entirely flush with the table. Also, because of the sensors and connections on the accelerometer, it was hard to lay it exactly flat onto a surface (also because the connectors were twisted), so there may have been error in simply experimentation there as well that decreased the accuracy in the measurements). The values below show the measurements I got for roll and pitch at -90, 0, and 90 degrees where I paid attention to making the IMU as flush with a flat surface as possible.

Roll = -90 degrees:

<img width="303" alt="Screenshot 2024-02-22 at 2 29 04 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/33695187-60ca-42de-ae4e-61babacab655">


Roll = 0 degrees:

<img width="339" alt="Screenshot 2024-02-22 at 2 29 17 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/0f25f773-03b3-410f-a4d1-1bf532a60521">


Roll = 90 degrees:

<img width="327" alt="Screenshot 2024-02-22 at 2 29 34 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/3f325fc0-6d3b-4571-a1eb-1879ba309576">


Pitch = -90 degrees:

<img width="292" alt="Screenshot 2024-02-22 at 2 29 49 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/a4e97230-26c8-47ca-a505-005e6d842df4">


Pitch = 0 degrees:

<img width="293" alt="Screenshot 2024-02-22 at 2 30 07 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/73141f0e-aed2-4d9b-a773-925b9885a282">


Pitch = 90 degrees:

<img width="133" alt="Screenshot 2024-02-22 at 2 30 19 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/a06f38a8-b3de-44aa-980d-1dd6b8553cc5">


## Accelerometer Calibration

My data seemed fairly accurate with not too much drift as I was, on average, approximately an order of magnitude of about 10 thou off from -90, 0, and 90. I used two-point calibration to detetrmine a conversion factor to ensure that the final output matched the expected output. I used the following methodology regarding calibrating sensors from Adafruit to determine the conversion factor:

<img width="664" alt="Screenshot 2024-02-22 at 12 05 11 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/7c7e20dc-63de-4bc8-aa21-d2436f24bd96">

Roll:


Reference Range = 90 - -90 = 180

Raw Range = 89.8 - -89.5 = 179.3


CorrectedValue = (((RawValue – RawLow) * ReferenceRange) / RawRange) + ReferenceLow =   

CorrectedValue = (((RawValue + 89.5) * 180) / 179.3) + (-90)  

Pitch:  

Reference Range = 90 - -90 = 180  
Raw Range = 90.2 - -85.4 = 175.6  

CorrectedValue = (((RawValue – RawLow) * ReferenceRange) / RawRange) + ReferenceLow =   

**CorrectedValue = (((RawValue + 85.4) * 180) / 175.6) + (-90)**  

As can be seen, the roll seems to have more accuracy than the pitch in terms of the range being closer to [-90, 90].

## Accelerometer Noise

Next, I worked on determining how much noise existed in the accelerometer data. I initially had left the accelerometer steady and tried to conduct the Fourier transform of that data. However, after talking with my classmate, Stephan Wagner, I noticed that this would not allow me to extract a frequency of my accelerometer signal. My Fourier transform was showing a peak amplitude at the value of 0 Hz (because my accelerometer was stationary). To fix this, I then recollected data by moving my accelerometer sinusoidally (i.e. rolling/pitching/tilting it back and forth at an approximately constant frequency) and with vibrational noise.

The original data plotted looks as such (not entirely sinusoidal but moving):

<img width="458" alt="Screenshot 2024-02-22 at 1 22 53 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/7ed3fb71-29ec-40db-bb3f-8b9eaf2993f4">

I then conducted a Fourier Transform on the data.

Roll:

<img width="438" alt="Screenshot 2024-02-22 at 1 24 50 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/da015cc7-5ef9-4c5e-b5c5-aa4f08e2aa9c">

Pitch:

<img width="446" alt="Screenshot 2024-02-22 at 1 25 07 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5d567ff2-5d47-4fee-ac5f-1fc592eee1ec">

## Accelerometer Lowpass Filter

As can be seen, largely, the amplitude is found at lower frequencies as opposed to higher ones. I had a sampling frequency of about 414 data points per second (based on time stamps I had while collecting the accelerometer data). I chose a cut off frequency of around 2 Hz because I didn't seem to have too much noise past 2 Hz. I used an alpha for my low pass filter to be around 0.61 (first, I tried with a lower frequency of 0.10).

The data I used for this analysis included noise coming from me introducing vibrational noise as well by inducing light shaking when I was collecting the data).

After trying the filter, I found that pretty much all the data was being filtered as can be seen in the screenshot below.

<img width="437" alt="Screenshot 2024-02-22 at 1 48 01 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/16873396-6dbc-465c-892f-ef812880362a">

I realized this was because in my filter, I was replacing the previous data meaning the filter ended up calculating zero for each consecutive data point. After changing this, I was able to generate data that seemed to make more sense in terms of being filtered:

<img width="435" alt="Screenshot 2024-02-22 at 1 54 33 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/b0b1e2aa-028b-4bcd-aac8-a5ccb6d01302">

With an alpha of 0.10, these were the results of the data (as can be seen, the noise has decreased).
<img width="437" alt="Screenshot 2024-02-22 at 2 09 56 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/dfa32050-345c-47c0-afa6-bf4ef01bc2c6">

<img width="435" alt="Screenshot 2024-02-22 at 2 10 08 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/f5203cc7-e415-49fb-8c58-90ef7f03c15e">

As the cutoff frequency increases, the alpha increases in the low pass filter, so I then tried increasing my cutoff frequency to the original 0.61 that I had calculated (i.e. this would increase how much past measurements affected future measurements through the smoothing factor alpha). Because alpha increased, we can see that it largely mimics the original data without reducing noise too much, so a lower alpha value would be desirable for the filter.

<img width="446" alt="Screenshot 2024-02-22 at 2 19 48 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/ffd9038f-e920-4e9b-86d4-3db5f5d7c65f">

<img width="459" alt="Screenshot 2024-02-22 at 2 19 59 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/29f8942b-0e35-4700-a895-0f722eeb8aa7">

It seems that the accelerometer to begin with did not have too much noise (after looking into it, I found that this was because there was already a low pass filter implemented into the accelerometer).

## Gyroscope Data

Next, I started exploring working with the gyroscope. I was able to calculate roll, pitch, and yaw from the gyroscoope readings by using the equations (in code below) that we had learned in class. Because the gyroscope measures angular velocity, I would have to calculate some dt that I could use to discretely integrate the gyroscope velocity and add to the previous roll, pitch, and yaw. The gyroscope values for roll and pitch (and yaw) seem to drift more than the accelerometer although they are less noisy, so they start off pretty accurate in terms of having a similar reading, and then for large movements in roll and pitch, they drift off and don't match the values anymore. I noticed this happening more in pitch than roll (I think this is because I accelerated pitch very quickly which wasn't detected quickly by the angular velocity from the gyroscope as compared to the accelerometer).

<img width="321" alt="Screenshot 2024-02-23 at 7 18 21 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/8fbf3203-b49c-4712-bdcd-cbf90f6eefc5">

Data:

<img width="264" alt="Screenshot 2024-02-28 at 12 05 39 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/58472691-af40-4ef6-8e09-301d78b5a277">

When I increase the sampling frequency, the values seem to match better (the gyroscope data seems to be more accurate) although the accelerometer data continues to be very noisy. Also, here there was not too much angular acceleration given to the data which also lead to the gyroscopic data being more accurate.

<img width="378" alt="Screenshot 2024-02-28 at 12 37 31 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/fbbc02ca-04ee-4ac1-bbfe-5dbc7b7a7714">

## Gyroscope Complementary Filter

Given that there is an issue with drift in the gyroscope and an issue with noise in the accelerometer, a complementary filter was implemented to try and receive more accurate and less noisy results from the IMU for roll and pitch.

<img width="341" alt="Screenshot 2024-02-28 at 12 43 49 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/f5778e66-337c-46e2-b473-14f0b641428d">

I used an alpha2 value of 0.8 to start because I wanted to exploit the higher accuracy in the accelerometer while also having more stability in terms of reducing noise from the gyroscope. These results seemed to work fairly well as can be seen in the data below. For the roll, the noise is minimized with the weighting of the gyro; however, it seems to maintain accuracy with the overall shape of the accelerometer. This data was collected adding vibrations. The filter seems to work for a variety of angles but seems to work best in the working range of lower angles.

<img width="368" alt="Screenshot 2024-02-28 at 12 58 52 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/9ace4b9b-0312-456b-b362-5a0d899010ae">

The same pattern can be seen for the pitch as well:

<img width="362" alt="Screenshot 2024-02-28 at 12 59 46 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/90f1ff32-1462-47bd-a7c5-34c5f10dd39f">

## Speed Up Data / Store Data

To speed up the data collection, I removed all Serial.print statements and delays in my code that I was using for debugging. I also added logic to check if data is eady in every iteration of the main loop to store it (I had already previously done this). In terms of storing data, I stored it as a float in separate arrays (that way the data could be manipulated/processed separately without having to spend time separating the data into accelerometer, TOF, and gyroscope data). I also initially decided to use doubles instead of floats that way no decimals are lost in calculations (although floats could work as well to save on space and collect more data points). After trading these off and realizing that too many decimal points are probably not critical for the purposes of our robot, I decided to use floats to store the data. I was able to sample data every about 3 milliseconds. The main loop does run faster than how often the IMU is ready to send data (so to prevent this, the main loop only stores data when it's ready so that the loop doesn't take longer than needed). The image below shows my time-stamped IMU data being stored in different arrays (roll, pitch for accelerometer and roll, pitch, yaw for gyroscope).

<img width="842" alt="Screenshot 2024-02-28 at 1 35 23 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/cc10dd27-3f11-44cf-996f-aa506e627bad">

Each of the arrays can take up 1/3 of the data available on the Artemis (given that the data is stored by accelerometer, gyroscope, and TOF separately and not how I collected it with roll, pitch, yaw). With 384000 bytes on the Artemis and each float per array taking up 4 bytes of data, this is about 32000 data points; with a sampling rate of 3 milliseconds, this is the equivalent time of 96 seconds of data or about 1.6 minutes of data (in theory).

## Data Sent Via BlueTooth

Lastly, I worked to send data from the IMU via Bluetooth to my computer as can be seen here:



## Record A Stunt

After playing around with the robot, I found that it didn't maintain a very straight line going forward but did backwards; also getting it to turn was pretty straightforwad, and it would maintain its motion in the face of an obstacle until it would turn and then continue. I might need to figure out some way to correct for it not travelling linearly when I work on the motor drivers in future labs.

<iframe width="315" height="560"
src="https://youtube.com/embed/4fhogKkCdQo"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

