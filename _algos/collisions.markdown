---
layout: other
title:  Collision Systems Simulation [Almost Complete]
description: Attempt to simulate, with physical accuracy, a system of 2D colliding particles.
date:   2017-3-7 7:41:12 -0800
---

The following page documents how to simulate a collision system. The
implementation below assumes perfectly elastic collisions, but the fine
details don't really matter.


## Videos

Coming Soon.



## Problem Statement

Attempt to simulate, with physical accuracy, a system of 2D colliding particles.

## Methods

There are two ways to potentially implement this system: discrete-time driven or by event driven.
Let us explore discrete-time driven first

### Discrete Time Driven Simulation

The idea behind discrete time driven is to first define a small increment of time, lets call that dt.
Then we loop the following:

1. Move the system forward by dt seconds, check for collisions between all particles.
2. If two particles collide or are colliding, update their velocities
accordingly.  
3. Repeat.

There are several obvious flaws with such a system, let us explore the problems.

1. Checking for collisions between all particles takes n^2 time (where n is the number of particles).
2. If dt is too small, there are usually few collisions per check, and we waste a lot of time running the expensive collision check.
3. If dt is too large, 2 particles could move past each other, resulting in an inaccurate simulation.

These issues are difficult to patch, so let us examine another way to do this simulation.

### Event Driven Simulation

Enter event driven simulation, a solution to all the problems defined above.

First lets define the terms.

* **Unit Box**: The box in which the simulation takes place. For our purposes this will take place in a 1 x 1 box (ie. all on-screen co-ordinates are between [0,1])
* **Event**: There are 4 kinds of events. Event 2-4 will keep track of the number of collisions its particles have collided with prior to the prediction of this event, as well as the time this event occurs.
  1. A redraw event. This refreshes the screen. Since we want a consistent frame rate, we should be redrawing every dt seconds.
  2. A side collision event. A particle collision with left or right side of the unit box.
  3. A updown collision event. A particle collision with the top or bottom of the unit box.
  4. A particle-particle collision event. A collision between 2 particles.  
* **Consume**: To process an event
* **Obsolete**: Events can go obsolete, meaning the predicted event will no longer happen. more on this later.
* **Particle**: A circle with mass, velocity, and radius. Particles collide with each other perfectly inelastically. Each particle also stores the number of times it has collided with something.  

Next lets design the system.

1. Create a PQ that stores events and lets us retrieve the most impending event (the event with the smallest time)
2. Predict all collisions that will happen between all particles and something.
3. Insert a redraw event that happens at time 0.
4. Pop off the most impending event. Determine if it is Obsolete.
5. If not obsolete, consume the event.
6. Put newly created events from the consumption back into the PQ.
7. Repeat from step 2.

Now let us detail how events are consumed.

#### Obsoletion Check

Check to see if the particle's collision counter matches the number of collisions stored in the event.
Since the collision number in the event indicates the number of collisions this particle has been in
since the event was predicted, if the 2 numbers do not match, that means the particle has
participated in another collision since this event was predicted, this means the collision predicted
will not happen, since this particle has a different velocity than when the prediction was made.
This makes the event obsolete.


#### Redraw Event

A redraw event has no particles in question. It can never be obsolete.

First we move all particles to their positions that they will be at the redraw event time.
Draw all particles to the screen.
Insert a new redraw event at time + dt.

#### Side Collision Event

A side collision event has 1 particle in question. Check for obsoletion.

First, we move all particles to their positions that they will be at the time of this collision event.
Reverse this particle's x-velocity. Determine whether or not this particle will collide with
any other particle. Create events from the predictions and shove them into the PQ.

#### UpDown Collision Event

A side collision event has 1 particle in question. Check for obsoletion.

First, we move all particles to their positions that they will be at the time of this collision event.
Reverse this particle's y-velocity. Determine whether or not this particle will collide with
any other particle. Create events from the predictions and shove them into the PQ.


#### particle-particle Collision Event

A side collision event has 2 particles in question. Check for obsoletion.

First, we move all particles to their positions that they will be at the time of this collision event.
Update the velocities based on the physics formula. Determine whether or not both particles will collide with
any other particles. Create events from the predictions and shove them into the PQ.


## Runtime of the Event-Driven Simulation

Note how consuming an event takes at most linear time.
Popping and inserting from the PQ takes linear-rithmic time.
The only costly operation is the single n^2 operation at the beginning to
determine the initial collisions. Since the simulation is on average linear-rithemic,
we may consider it to be reasonably efficient.
