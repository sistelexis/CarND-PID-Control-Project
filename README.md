# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

# Overview
This repository contains my implementation PID controller project in Udacity's Self-Driving Car Nanodegree.

---

## Project Instructions and Rubric

More information is only accessible by people who are already enrolled in Term 2 of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562) for instructions and the [project rubric](https://review.udacity.com/#!/rubrics/824/view).

## Reflection

### Describe the effect each of the P, I, D components had in your implementation.

As soon as I started playing around with the components paramenters, it was quite clear that they affected the trajectory the way it has been described during the course:

P - proportional control - that is the main parameter to bring the car to the right position, unfortunately, it is not enough by itseld to do the work alone since it gets very unstable.
I - integral control - this one, as said in the course, in meant to eliminate any existing bias. Since it is not the case here, the recommended value is so low the it can be simplified as being zero. Values of the same dimension used on the other modules makes thye car extremely unstable.
D - diferential control - this parameter acts like a stabilizator to remove the "over-reaction" from the P module

### Describe how the final hyperparameters were chosen.

1 1 1 -> Jumps directly out to the right off the road
0 0 0 -> Goes perfectly straight and gets out on the first kerb
-1 -1 -1 -> Goes Extreme left then fully right and gets off the road (Negative values look better than positives)
-1 0 0 -> Starts ok then increases occilation until it gets off th road
0 -1 0 -> Same as [-1 -1 -1]
0 0 -1 -> A bit better than [0 0 0]
-1 0 -1 -> Quite close to [-1 0 0] (same weight has a bigger effect on parameter 1 than on 3)
-0.1 -0.1 -0.1 -> Goes Extreme left then fully right and gets off the road
-0.1 0 0 -> reaches the first kerb, but undershoots it, and gets off the road
0 -0.1 0 -> Same overall behaviour than [-1 -1 -1] 
0 0 -0.1 -> Same as (0 0 0)

Based on those first results, it seems that the system is:
- much more sensible to changes on parameter I than on parameter P
- more sensible on changes on parameter P than on parameter D.

Based on the description, Parameter I is made to correct some bias, that doesn't appear to be present on the demo, so setting it to a very low value close to 0 (or exactly zero) seems a good approach.

Based on that approach, from now on, we will only concentrate on Parameters P and D. We have also learnt that D needs to have a bigger asolute value.

-1 0 -10 -> Quite nervous but completes the lap
-0.1 0 -1 -> Smoother but less reactive than [-1 0 -10]
-0.1 0 -10 -> A bit like [-1 0 -10]

Fine tuning approach:

-0.1 0 -5 -> Between [-1 0 -10] and [-0.1 0 -1]
-0.5 0 -5 -> Slightly more precise than [-0.1 0 -5] but still too nervous
-0.5 0 -3 -> Ok
-0.1 0 -3 -> Ok

It is hard to evaluate all the working sets of parameters. The main idea is to find the sweet spot between reactivity and stability.
Using twiddle should provide us a set near those last two. Unfortunately, due to lack of time, I have not been able to follow that path.



