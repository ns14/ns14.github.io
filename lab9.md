## Lab 9:

# Compute Control

The first part of this lab asked us to compute_control or given the odometry, poses, extract a change in rotation, a translation, and another change in rotation using the relationship in translation and orientation of the previous and current pose.

The following code was used to do this:

<img width="593" alt="Screenshot 2024-04-24 at 8 16 48 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/2fd83fc6-bb4c-42ed-8fa8-9ade817349e2">

# Odometry Motion Model

Next, it was necessary to calculate p(x' | x, u). This indicates the probability that the robot is in a certain state given the previous state and a control input.
Essentially, a Gaussian distribution was used to define what the probability of the robot being in the new position was.

<img width="697" alt="Screenshot 2024-04-24 at 8 25 16 AM" src="https://github.com/ns14/ns14.github.io/assets/65001356/28677b8c-dbab-45b6-97ba-6449d754595a">

# Prediction Step

The next was the prediction step of the model. The prediction step in the Bayes filter takes the probability that the robot is in a certain state (bel(x


