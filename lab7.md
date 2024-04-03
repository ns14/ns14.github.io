# Lab 7:

## Estimate Drag and Momentum

The first step of this lab was to estimtae drag and momentum of the system. I was initially unsure of how to proceed with doing this given that we could not easily measure the drag of the robot. After speaking with Dr. Jaramillo, I realized that this is done because we need a way to esimate the system dynamics of the system for the state space despite not being able to measure/quantify certain values. Thus, creating "virtual" versions of these values that use "pseudo" estimations of these parameters, the state space models can be made using relevant parameters.

To accomplish this, I picked a step response of 100% of the maximum u. I then used a for loop for a long enough state space (played around with it to make sure the car didn't run into the wall). Here are graphs with the data for time steps (note the slight variations in the data are from when there was a slight obstruction that came in the way)

<img width="426" alt="Screenshot 2024-04-01 at 4 57 48 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/884d0485-8c17-46cf-814e-287654a7d1c5">


<img width="485" alt="Screenshot 2024-04-03 at 2 18 54 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/6be1388a-46cd-4d90-bf46-902566d0b91c">
