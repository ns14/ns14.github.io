# Lab 3:

## Prelab

The default I2C address of the Time of Flight (TOF) sensors is 0x52 according to the datasheet. However, because two TOF sensors are being used, it is necessary to use the XSHUT pin to temporarily shut off one of the sensors so that the I2C address cah be changed of the other sensor (that way both TOF sensors can be communicated with). I decided to choose this method as opposed to shutting down both sensors as there will be no need to have another XSHUT pin wired to the Artemis GPIO keeping wiring management a little more simpler.

I plan on placing the TOF sensors on opposite ends of the car for detection on each side (as such I used the two longer QWIIC connectors so that they could reach both sides of the car assuming the Artemis was in the middle). Reading through the datasheet, I found that the range and FoV is programmable with different distance modes and time budgets as well. I think that placing the sensors at the front and back of the car will be the most useful because it will allow me to detect obstacles in front of the car as well as rotate and detect on the opposite direction of the car as well. Technically, this means that obstacles on the side of the car will be missed unless the robot turns sideways to detect these obstacles and then turns back forward to continue that path; however, because I largely expect the robot to move forwards and backwards, I don't expect this to be too huge of an issue as the robot won't be moving side to side, so we care more about the obstacles the robot will directly run into moving forward and backwards.

Thinking ahead about the wiring, the following shows what I expect my wiring to look like in lab:

<img width="965" alt="Screenshot 2024-02-28 at 11 39 49 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5c09dbbb-8c84-4436-998d-084252dcf366">

I will try to make all the sensors (IMU/TOF) detachable so that I can move them around as needed (the soldered connection for one TOF's XSHUT pin will be permanently soldered). I use the longer QWIIC connectors for the TOF sensors because I am trying to put them on opposite sides of the robot whereas the IMU can pretty much go "anywhere" in terms of not needing to detect obstacles on the outside of the robot. The XSHUT pin of one TOF also needs to connect to the Artemis, so I need to solder on a pretty long wire onto one TOF sensor's XSHUT pins to the Artemis' GPIO pin. Because the TOF sensors face outwards, I will solder the wires from the back so that they are not sticking out the side that has the sensor and possibly interfere with it.

## Battery

First, I soldered the battery wires to the JST jumper wires. To test that I soldered these correctly, I powered the Artemis without the USB C port and tried sending BLE messages (using demo.py from Lab 1) to ensure that it was working. Results can be seen below:

<img width="358" alt="Screenshot 2024-02-28 at 8 29 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/b3733579-dc70-4f51-b202-407c648fe1e0">


<img width="819" alt="Screenshot 2024-02-28 at 8 28 17 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/88b64b2f-601d-44b8-bb0e-1251661a6cc2">

<iframe width="315" height="560"
src="https://youtu.be/embed/EXYS9i2Z2SM?feature=shared"
title="YouTube video player"
frameborder="0"
allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
allowfullscreen></iframe>

## Connecting one TOF Sensor

Using the QWIIC breakout board, I connected the first sensor to the Artemis (using the long cable because it needed to span half the length of the car). I connected SCL to yellow and SDA to blue. The wiring of the TOF sensor is shown below:

<img width="117" alt="Screenshot 2024-02-28 at 8 36 25 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/18a95719-e0da-4c23-aa5c-aaba57937211">


<img width="413" alt="Screenshot 2024-02-28 at 8 36 15 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/96f7b96b-c470-4381-a3fc-2f33e31bc1c1">

I then ran Apollo3 > Example05_Wire_I2C and found that the address is actually 0x29 instead of 0x52. This is because the address seems to be bit shifted to the right (the rightmost bit is used to determine whether data is being written or read).

<img width="382" alt="Screenshot 2024-02-28 at 8 55 12 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/66229051-bb36-4101-a910-e04a16922a1d">

## Three Modes

I then looked into the three modes for the final robot. After reading through the datasheet, these were the conclusions I came to:

1. .setDistanceModeShort(): The short distance mode has a maximum distance detection of 1.3 m (the shortest) but has better ambient immunity (i.e. it is less susceptible to noise).
2. .setDistanceModeMedium(): This mode has a distance in between short and long of 3 m and has slightly worse ambient immunity than short.
3. .setDistanceModeLong(): This mode has the maximum distance of 4 m but has the worse ambient immunity (i.e. it is susceptible to noise with a lot of ambient light).

I don't foresee our robot having to see farther than about 1 meter (for obstacle collision at least because we care about the obstacles closer to the robot), and because we will be using it in a lot of different ambient lighting conditions, I think the short distance mode will work best for this application.

## Testing Chosen Mode

I used the Example1_ReadDistance to test if how my chosen mode worked. Because the default sensor mode is long, I used the code below to change it to short. These were the distances I then got from the sensor (in a slightly dim room).

<img width="1073" alt="Screenshot 2024-02-28 at 9 06 39 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/55178905-3762-4e9c-ae7a-d7d1f282db18">

Next, I worked on documenting the TOF sensor's range, accuracy, repeatability, and ranging time:

1. Range: to measure the range, I set the TOF sensor as close to a box as possible and continued to move it until the error between the actual distance and measured distance became too large. The results are shown below:

<img width="449" alt="Screenshot 2024-02-28 at 9 57 48 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/23b52550-de2d-4552-a0b5-6b1f7ccec05a">

Visually inspecting the data, it seems that the sensor has a range of between 400 mm and 850 mm (which is pretty consistent with the data sheet) before the distance readings are not accurate. I also plotted the errors for each distance I took measurements for, and it seems that the lowest error is between 400 mm and 850 mm which is consistent with my visual inspection: 

<img width="465" alt="Screenshot 2024-02-28 at 10 04 21 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/8fc2956a-6456-4961-b08d-0008cf065089">

2. Accuracy: to measure accuracy, I took 10 measurements at different data points and averaged the error between those measurements. The results are shown below:
   
<img width="460" alt="Screenshot 2024-02-28 at 10 08 29 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/dff60797-ed3d-441d-8a54-9dc23b490b21">

Calculating the accuracy in the above range, I got an accuracy (error) of 13.7 mm (I think this is slightly higher due to inaccuracy in me taking measurements as well (i.e. the sensor slightly moving in between repeated measurements, for example).
   
3. Repeatability: to measure repeatability, I took 10 measurements at the same distance and compared how often the error repeated (this is basically the standard variation of the measurements). The results are shown below for the measurement of 101 mm. Given that the measurements are in mm, it can be seen that the measurements are largely repeated.

<img width="507" alt="Screenshot 2024-02-28 at 10 15 18 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/41185bf7-706a-46ab-b63d-0ab3c52dedaf">

4. Ranging Time: to measure ranging time, I used the timestamps in the Serial monitor to find the ranging time to be about 67 ms on average for a series of data points that were collected by the TOF sensor.

## Hooking Up Both Sensors

Now that I had data about one TOF sensor, I used connected both sensors to the Artemis as shown below:

<img width="456" alt="Screenshot 2024-02-28 at 10 22 02 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/179f34b4-96de-45ad-a8bb-fd5b6843f344">

I connected the XSHUT pin of one sensor to a GPIO pin on the Artemis to ensure that different I2C addresses would be used for both TOF sensors (I used .setI2CAddress() to change the I2C address of one of the sensors). I then was able to get distance readings from both TOF sensors:

<img width="479" alt="Screenshot 2024-02-28 at 10 51 50 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/97a13fb1-fe60-4fa3-b777-2a2315e93ece">

## Execution Time

Because in future labs, the code needs to execute quickly, I added continuous timestamps to my code and printed new data only when available. The code snippet is shown below:

<img width="261" alt="Screenshot 2024-02-28 at 11 07 52 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/fe87494b-017f-431e-a635-fee62cba4ccd">

From the timestamps, it seems that the loop executes about every 7 milliseconds, but the TOF sensors' data is sent every 103 milliseconds. Possible limiting factors are that there is some time in between for the data to be read and then written or that the data is simply not available everytime through the loop (due to the checkForDataReady() if statement).

<img width="487" alt="Screenshot 2024-02-28 at 11 08 19 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/e5bc22b5-9505-46e7-ac5c-d348fbde02eb">

## Record Time and Send Data To Computer

Lastly, I merged my previous labs' code to this lab's code and was able to send the data of both TOF sensors to my computer. The graph of this data is shown below:

<img width="493" alt="Screenshot 2024-02-28 at 11 27 11 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/922436e1-7f7f-4c50-a72c-4454ede5d680">















