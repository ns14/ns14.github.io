---
layout: default
---

# Lab 1 (Part A):

Step 1 in Lab 1 was to install the Arduino IDE and follow the setup instructions to install the necessary board and libraries. To test that the board was successfully connected to my computer, I used Example: Blink it Up. This example caused the LED on the board to blink.

Following this, I followed the Example: Serial. This allowed me to type in character inputs and then receive those same character outputs in the Serial monitor. This would be useful for later parts of the lab when I would need to use the Serial monitor.

Then, I tested the sensors on the Artemis. The first sensor was tested using Example2_analogRead which was the temperature sensor. As can be seen in the video, as I pressed onto the sensor (to warm it up), the temperature reading in the Serial monitor increased. As I waved the sensor around (i.e. causing it to cool down with the cool air), the temperature reading in the Serial monitor decreased.

Lastly, I tested the microphone using Example1_MicrophoneOutput. I used an online note player to see how the frequency of the sound would change. I also then used a music video to see how the microphone would return frequencies that had a lot of noise. It was interesting to see that the microphone was able to discern out the highest frequency noises from the sound.

# Lab 1 (Part B):

To set up for this lab, it was necessary to create a virtual environment with the correct Python and pip versions. I ran into an issue that my computer's Python version was 3.8 although we needed above 3.9 for the lab. This is because my Python version was only updated in Anaconda. Because of this, I decided to create a conda environment instead of a venv environment so that I could use more updated versions of Python and pip.

After creating my virtual environment ("fastrobots_ble"), I installed the necessary packages (numpy, pyyaml, colorama, nest_asyncio, bleak, and jupyterlab).

The first setup task was to establish the Bluetooth connection. To do this, I ran ble_arduino.ino and was able to print the MAC address.

In connections.yaml, I then updated the MAC address to match the MAC address printed in the Serial monitor.

I then used uuid4() to generate a new UUID and replaced it in both ble_arduino.ino and connections.yaml.

I also was having issues initially establishing a bluetooth connection and had to change if IS_ATLEAST_MAC_OS_12 to if True. I then checked that all the command types in enum and cmd_types.py were the same.

To check that the Bluetooth connection was properly established, I ran through demo.py. I was successfully able to go through all the code blocks.

Check for Understanding: The Bluetooth codebase consists of various scripts that allow a Bluetooth connection to be enabled between the Artemis and my computer. ble_arduino.ino takes care of this from the Artemis side. First, the UUIDs that were generated above help identify types of data that will be sent between the Artemis and my computer. BLEService is then used to set the local name and service, add BLE characteristics and service. The characteristics are different types of data provided by the ArduinoBLE (in our case, we only use BLECStringCharacteristic). Then, there are functions that can write values from the Artemis to transmit to the computer.

# Lab Tasks

The first task was to send a string value from the computer to the Artemis and then to send that string back from the Artemis to the computer with an added string. I was able to accomplish this by using similar logic as the PING case in the code.

The next task was to return a string characteristic with the time. This task was fairly straightforward, and I used the millis() function in C to get the time and then append it to “T:”

After being able to receive the time, the next step was to set up a notification handler to receive the string value, and then, in the callback, receive the time from the string that was sent. I was able to do this using the code below.

After setting up a notification handler, I wrote a loop to get the current time from the Artemis and sent it to the notification handler. After collecting some values, I was able to calculate how fast the messages could be sent by:

Thus, the effective data transfer rate was:

Now that I could receive time stamps for data, the next step was to create an identically-sized array that would collect temperature readings (as done in Lab 1 (Part A)). It was important that the timestamp in the first array would correspond to the temperature reading taken at that time.

I then added a command GET_TEMP_READINGS that looped through both arrays and sent the temperature readings with a timestamp (which the notification handler then took and added to two lists).

In terms of the 

5. Discussion

This lab gave me a strong understanding of how bluetooth works between the Artemis board and my computer as well as how data is transmitted between the two

This lab gave me a strong understanding of how Bluetooth works between the Artemis board and my computer as well as how data is transmitted between the two. I did have some issues setting up my virtual environment due to the way my Python was updated through the 
