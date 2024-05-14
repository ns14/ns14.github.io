## Lab 11:

# Localization

While working on this lab, I was having issues with my robot's motors (they were turning at very inconsistent speeds, and sometimes one set of wheels would stop turning entirely). Although I was able to temporarily fix this, when I went to collect my data again, I realized that the wire connecting one of the motor drivers to the motor had completely come out (not on the motor driver side but on the motor side). This essentially meant that I had no control over one set of wheels, so in order to perform orientation control, I would need to somehow restrict those set of wheels from turning and only let the other set of wheels turn (thus the robot would rotate about the axis of the non-spinning set of wheels). Although this could have been solved mechanically (my jamming the other wheels), due to time constraints as I was leaving Ithaca, I decided, for the purpose of this lab, to use my Lab 9 data as my mapping data was not working as planned. Overall, the issues with my motor meant that I didn't necessarily get the results that I wanted, but I'm super thankful for Larry for hosting OH super last minute so that I could spend some time to at least try getting a map + Jonathan for letting me use the room!!

# Confirming that the Localization Worked in Sim

First step for this lab was to confirm that localization would work for in the simulation. I ran the localization scripts provided to us and saw a fairly desirable map of the Bayes Filter running on the simulated robot. Results are pictured below:

<img width="263" alt="Screenshot 2024-05-09 at 2 31 06 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/6aca91ac-ab19-4fd2-9175-64e6f8e32d2d">

# Localization on Robot
As mentioned above, I was having a lot of trouble getting my robot to map correctly because one of the motors was not working, so I instead defaulted to using my Lab 9 data to see if the robot worked instead.

Here are the results for each run:

Point 1:

<img width="283" alt="Screenshot 2024-05-14 at 5 14 32 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/5d850112-c37c-40e2-820a-eae333582c75">


Point 2:

<img width="274" alt="Screenshot 2024-05-14 at 5 14 44 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/07f37f92-0f1b-4335-a701-420dd7d29cb9">



Point 3:

<img width="261" alt="Screenshot 2024-05-14 at 5 14 56 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/1cb9416b-f3b9-4110-8c21-29d0eb79b7cd">



Point 4:

<img width="310" alt="Screenshot 2024-05-14 at 5 17 02 PM" src="https://github.com/ns14/ns14.github.io/assets/65001356/117cc6f5-505f-4dcb-9735-5a54ab185b28">


# Conclusions

This was a cool lab that let me tie in my work for mapping and implementing the Bayes Filter in the simulation and confirming that it would work on my actual robot. Although I wasn't actually able to test this out in the lab testing set up, it was still cool to see how this worked in the simulation using the lab data!
