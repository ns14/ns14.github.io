# Lab 1 (Part A):

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

# Lab 1 (Part B):

To set up for this lab, it was necessary to create a virtual environment with the correct Python and pip versions. I ran into an issue due to my computer's Python version being 3.8 although we needed above 3.9 for the lab. This is because my Python version was only updated in Anaconda. Because of this, I decided to create a conda environment instead of a venv environment so that I could use more updated versions of Python and pip. To set up my virtual environment, I used the following commands:

After creating my virtual environment ("fastrobots_ble"), I installed the necessary packages (numpy, pyyaml, colorama, nest_asyncio, bleak, and jupyterlab).

The first setup task was to establish the Bluetooth connection. To do this, I ran ble_arduino.ino and was able to print the MAC address:

<img width="340" alt="Screenshot 2024-02-21 at 4 27 30 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/9df45b2d-5110-4d51-8fe2-fd801b4f9cac">

In connections.yaml, I then updated the MAC address to match the MAC address printed in the Serial monitor:

<img width="342" alt="Screenshot 2024-02-21 at 4 20 29 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/951860eb-12cb-416d-98ee-6162454e9d80">

I then used uuid4() to generate a new UUID and replaced it in both ble_arduino.ino and connections.yaml:

<img width="497" alt="Screenshot 2024-02-21 at 4 23 11 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/571b859b-81a1-45c1-9ddd-c6dc77fef250">

Updates in connection.yaml:

<img width="409" alt="Screenshot 2024-02-21 at 4 22 07 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5cfc6100-567a-4325-95a5-655b8f7b25bf">

I also was having issues initially establishing a bluetooth connection and had to change if IS_ATLEAST_MAC_OS_12 to if True in the base_ble.py file. I then confirmed that all the command types in enum and cmd_types.py were the same.

<img width="207" alt="Screenshot 2024-02-21 at 4 23 56 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/bf5cd0a2-4264-4413-9ecf-db34a45e45c5">

<img width="168" alt="Screenshot 2024-02-21 at 4 24 12 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5e719fba-2c2c-45f0-9248-28e615a74921">

To check that the Bluetooth connection was properly established, I ran through demo.py. I was successfully able to go through all the code blocks as can be seen below:

<iframe width="315" height="560" src="https://youtube.com/embed/zKxTG53Y9IA?feature=shared" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted media; gyroscope; picture-in-picture; web-share" allowfullscreen> </iframe>

Check for Understanding: The Bluetooth codebase consists of various scripts that allow a Bluetooth connection to be enabled between the Artemis and my computer. ble_arduino.ino takes care of this from the Artemis side. First, the UUIDs that were generated above help identify types of data that will be sent between the Artemis and my computer. BLEService is then used to set the local name and service, add BLE characteristics, and service. The characteristics are different types of data provided by the ArduinoBLE (in our case, we only use BLECStringCharacteristic). Then, there are functions that can write values from the Artemis to transmit to the computer.

# Lab Tasks

The first task was to send a String from the computer to the Artemis and then to send an augmented String back from the Artemis to the computer. I was able to accomplish this by using similar logic as the PING case in the code.

<img width="539" alt="Screenshot 2024-02-21 at 4 28 14 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/febed7cf-070c-415f-bcb9-3fcfea96e067">

The output of echo with String "test" can be seen here:

<img width="409" alt="Screenshot 2024-02-21 at 4 29 03 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/7aa9f2ce-9f0e-4468-bd31-2f1780ecfb20">

The next task was to return a String characteristic with the time. This task was fairly straightforward, and I used the millis() function in C to get the time and then append it to “T:”

<img width="487" alt="Screenshot 2024-02-21 at 4 30 16 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/2fe160fd-19ac-4e4c-9c5b-10a19cc6621d">

<img width="407" alt="Screenshot 2024-02-21 at 4 32 05 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/c310fed2-15ac-40d1-842e-243ac3353d1a">

After being able to receive the time, the next step was to set up a notification handler to receive the string value, and then, in the callback, receive the time from the string that was sent. This is useful to speed up the main control loop (rather than having to wait for data, it takes care of receiving and processing the data without slowing down other processes). I was able to implement a notification handler by using the code below (when I initially tested this code, I kept receiving an error that the notification handler was already enabled; this was because I had not used "stop_notify" when testing. I looked at Julian Prieto and Jueun Kwon's pages from last year to help me debug this issue).

<img width="419" alt="Screenshot 2024-02-21 at 4 35 28 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/0c561109-1eed-417e-8462-12feab8c67dd">

After setting up a notification handler, I then tested it by instead collecting time in a loop for a couple seconds and then sending that from the Artemis to the computer. I used the datetime package in Python to determine the timestamps of when my computer received the data. From this, I was able to calculate a data transfer rate of 5e7 bytes per second.

Now, I tested another method of sending data where the data would be added to a global array in the Arduino IDE and then another function ("SEND_TIME_DATA") would send this data to the computer. I was initially running into a lot of issues with being able to create an array of times but not being able to send them via the notification handler. After debugging via Serial.print() statements, I realized that this was because I needed a delay in my loop (the data was being sent too fast), and this allowed me to then see the data saved into a list in my Python file as well.

<img width="514" alt="Screenshot 2024-02-21 at 5 32 35 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/b6436d14-341f-4e71-bb9a-17aaba499d8b">

<img width="424" alt="Screenshot 2024-02-21 at 5 32 58 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/afe12097-67e3-40e7-8881-716798a587cf">

<img width="715" alt="Screenshot 2024-02-21 at 5 32 46 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/94193b39-e085-485a-9291-14f31c511bba">

Now that I could receive time stamps for data, the next step was to create an identically-sized array that would collect temperature readings (as done in Lab 1 (Part A)). It was important that the timestamp in the first array would correspond to the temperature reading taken at that time.

<img width="500" alt="Screenshot 2024-02-21 at 5 43 07 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/3567bb13-923d-414e-9aca-5d6be7590aa5">

I then added a command GET_TEMP_READINGS that looped through both arrays and sent the temperature readings with a timestamp (which the notification handler then took and added to two lists).

<img width="518" alt="Screenshot 2024-02-21 at 5 43 21 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/1968f747-9c18-4f6c-9896-e5b7377a56fc">

<img width="792" alt="Screenshot 2024-02-21 at 5 42 34 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/165e9c09-0b87-47da-914f-545d2ff41635">

Although one method requires us to create an array and then later loop through that array to send timestamps/sensor data, the other method creates a loop that sends that data automatically. The advantages to the second listed method are that if one needs real time data sent over, that would be helpful to continuously receive data in real time from a sensor on the robot. However, in the case that there is some connectivity issue between the computer and Artemis, this would lead to loss of data with no backup. Thus, if there is real time data that needs to be transferred from the robot to the computer continuously, this method will be useful. On the other hand, if there are not as many opportunities to downlink data and data needs to be sent via packets, the first listed method would be more useful (i.e. in space robots, for example). This would allow for the data to be stored locally (preventing loss of data in case of connectivity issues) and then the data could be sent to the computer. However, this requires memory to be taken up as the values have to be stored before being sent over. The second method records data just as fast as the first one (however, data transfer is slower because now all the data is being sent somewhat delayed and all at a time). With 384000 bytes, that would be the equivalent of 96000 float data points (so 48000 time/temperature data points) and additionally a string to store this data (still close to 48000 data points) before running out of memory.

This lab gave me a strong understanding of how Bluetooth works between the Artemis board and my computer as well as how data is transmitted between the two. I did have some issues with my notification handler (in terms of being able to save the data to a list on my computer), but after understanding the theory behind how the notification handler worked, this process became a lot easier to understand and change based on the requirements of what was needed (i.e. are we storing one timestamp versus an array of timestamps). I feel more confident in my ability to use Bluetooth for future labs now.
