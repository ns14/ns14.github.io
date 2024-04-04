# Lab 7:

## Estimate Drag and Momentum

The first step of this lab was to estimtae drag and momentum of the system. I was initially unsure of how to proceed with doing this given that we could not easily measure the drag of the robot. After speaking with Dr. Jaramillo, I realized that this is done because we need a way to esimate the system dynamics of the system for the state space despite not being able to measure/quantify certain values. Thus, creating "virtual" versions of these values that use "pseudo" estimations of these parameters, the state space models can be made using relevant parameters.

To accomplish this, I picked a step response of 100% of the maximum u. I then used a for loop for a long enough state space (played around with it to make sure the car didn't run into the wall). Here are graphs with the data for time steps (note the slight variations in the data are from when there was a slight obstruction that came in the way)

<img width="426" alt="Screenshot 2024-04-01 at 4 57 48 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/884d0485-8c17-46cf-814e-287654a7d1c5">

I took the part of the data that had more accurate steady state values and got the following results (the data is negative because I placed the TOF sensor on the opposite side, but the results should still be the same just inverted; i.e. I did it so that in between two obstructions, I measured TOF data going away from one wall instead of toward the other).

<img width="385" alt="Screenshot 2024-04-03 at 2 22 50 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/6dd4ae7a-e357-413f-ad66-f1c86ad4ecb2">

Graphs shifted to first time being 0 seconds and units being corrected:

<img width="224" alt="Screenshot 2024-04-03 at 2 37 24 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/9f149ab6-f2fa-4f63-80cf-a4da5af71733">

To compute the velocity, I simply computed the slope between corresponding TOF sensor readings (v = d/t) and plotted those values. The motor input stays 100% the entire time (because there was no PID control here). Note: I referenced other students outputs to check if my values seemed reasonable when I was doing runs!

The steady state speed is around 1500 mm/s, and the rise time is around 0.4 seconds with a rise time velocity of about 2000 m/s.

So d is: uss/v = 1 / 2000 = 0.005 

And m is: -d*t/ln(0.1) = -0.005*0.4/ln(0.1) = 0.00086858896

I saved the relevant data to a csv file for future use!

## Initialize KF

I next computed the A and B matrices using the d and m values calculated above:

<img width="381" alt="Screenshot 2024-04-03 at 3 16 45 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/a65531a2-f59c-4a1b-99f8-d368dfcdfc9a">

I then discretized the state space with dimension of the state space being 2 (second-order system) and the sampling time being 0.1 seconds.

I next initialized the covariances. sig_z defines noise in the measurement (i.e. we estimate this to be around 20 mm). sig_u defines noise in the process (depending on the sampling rate). Because we assume uncorrelated noise for the Kalman filter, the off diagonal terms are 0 sig_u.

<img width="662" alt="Screenshot 2024-04-03 at 4 07 42 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/0569b1d7-c811-48e8-9a87-cae5bf4857c0">


For the position this is: sqrt(100 / 0.1) = 31 mm.
For the velocity this is: sqrt(100 / 0.1) = 31 mm/s.
