## Lab 10:

# Compute Control

The first part of this lab asked us to compute_control or given the odometry, poses, extract a change in rotation, a translation, and another change in rotation using the relationship in translation and orientation of the previous and current pose.

The following code was used to do this:

<img width="450" alt="Screenshot 2024-04-24 at 8 54 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/8736bd1f-c6b8-4448-bf11-d342bfdd21c1">


# Odometry Motion Model

Next, it was necessary to calculate p(x' | x, u). This indicates the probability that the robot is in a certain state given the previous state and a control input.
Essentially, a Gaussian distribution was used to define what the probability of the robot being in the new position was.

<img width="680" alt="Screenshot 2024-04-24 at 8 54 41 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/d23f4e4a-f326-446f-a8a1-0e4cc744afdf">


# Prediction Step

The next was the prediction step of the model. The prediction step in the Bayes filter takes the probability that the robot is in a certain state (bel(x_t-1), multiplying that by a transition probability (or the probability that the robot has moved to a certain new state) to find bel_bar(x_t). By calculating these for each of the grid spaces and normalized, the predict step has provided a way for us to determine a belief of being in a state given the previous control input and previous state. To speeden up the Bayes Filter, we ignore states that have a probability of less than 0.0001 (thank you to Rafi's lab for the tip!). This requires less calculations as it is already improbable that the robot is in that state and speedens up the Bayes Filter.

The code to do this is as follows:

<img width="742" alt="Screenshot 2024-04-24 at 9 02 32 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/ce32e47e-6c4d-4f1f-b41e-cea37333c40d">

# Sensor Model

The next step was to find p(z | x). i.e. if we got a certain measurement, what is the probability of receiving that measurement given we are in a certain state.

The code to do this is as follows:

<img width="753" alt="Screenshot 2024-04-24 at 8 58 53 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/81b4c585-1038-4591-aca9-1c75c201c107">



# Update Step:

The update step then updates the bel based on the bel_bar and sensor model. I.e. now that there is a sensor model, and given a prediction of what state the robot is in, calculate the new probability of where the robot is (using probabilities of the previous state as well as a measurement of the current state). Then, this is normalized. This is the final estimation of where the robot is.

The code to do this is as follows:

<img width="485" alt="Screenshot 2024-04-24 at 8 59 30 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/d3784b76-fa6f-46d0-b747-3be3a54787a7">


# Simulation Results:

After running the Bayes filter, these were the results of the simulation:

The simulator likely works well with a lot of objects in the range of the sensors (to detect and update probabilities) and poorly when there is open space where the probabilites remain largely uniform.

Sim Results:

<iframe width="560" height="315" src="https://www.youtube.com/embed/KFccahFHpV0?si=5-mA5Fhu4pRGZ4uG" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Initial:

<img width="580" alt="Screenshot 2024-04-24 at 9 21 34 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/6f816132-7e0c-4997-aaf1-d9839abf862a">


Bayes:

<img width="535" alt="Screenshot 2024-04-24 at 9 21 05 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/eaf588ed-17d8-46f5-82ef-b655384bd918">

<img width="551" alt="Screenshot 2024-04-24 at 9 21 11 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/efdec276-4076-4cbf-829b-9b4811767ae2">

<img width="509" alt="Screenshot 2024-04-24 at 9 21 15 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/4ce6fd6b-f1dd-433c-89a8-a242c2e4c6d5">

<img width="534" alt="Screenshot 2024-04-24 at 9 21 19 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/0cebf619-9670-42b1-ae41-2f04abdc45b0">

<img width="516" alt="Screenshot 2024-04-24 at 9 21 23 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/b434985a-d750-46f7-8215-3b9efbee07e0">

<img width="498" alt="Screenshot 2024-04-24 at 9 21 28 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/7a1d345d-debc-4159-81e7-70ac6378506a">


