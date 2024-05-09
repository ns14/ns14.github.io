## Lab 11:

# Localization

While working on this lab, I was having issues with my robot's motors (they were turning at very inconsistent speeds, and sometimes one set of wheels would stop turning entirely). Although I was able to solve this issue, because I left Ithaca Friday, May 11, I was unable to actually run the localization scripts on my robot. Instead, I borrowed Daria Kot's data for localization to see how well it would be estimated and also attached a video of my robot localizing in my room instead. I've also attached code I would have implemented on my robot if I had time to actually localize at each of the points in lab.

# Confirming that the Localization Worked in Sim

First step for this lab was to confirm that localization would work for in the simulation. I ran the localization scripts provided to us and saw a fairly desirable map of the Bayes Filter running on the simulated robot. Results are pictured below:

<img width="263" alt="Screenshot 2024-05-09 at 2 31 06 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/6aca91ac-ab19-4fd2-9175-64e6f8e32d2d">

# Localization on Robot
Given the note above, I used my friend, Daria Kot's data to test only the update step of the Bayes Filter on the robot. This was necessary because...

I also tried out the <> function on my robot in my room to see if it would successfully return the sensor range and bearing data, and as can be seen in the video below, it was successful! Like I mentioned above, I didn't have time to do this in lab before I left Ithaca due to other debugging issues, so I borrowed Daria's data for the sake of showing that it worked in the simulation:

# Conclusions

This was a cool lab that let me tie in my work for mapping and implementing the Bayes Filter in the simulation and confirming that it would work on my actual robot. Although I wasn't actually able to test this out in the lab testing set up, it was still cool to see how this worked in the simulation using the lab data!
