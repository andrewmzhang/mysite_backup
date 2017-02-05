---
layout: page
title:  Hog Optimal Solve FA2015
date:   2016-6-12 11:47:12 -0800
---

Here's my version of solving the Fall 2015 version of the game of hog.
Here are the rules copied from the CS61A website:

## Problem Definition:

In Hog, two players alternate turns trying to be the first to end a turn with at least 100 total points. On each turn, the current player chooses some number of dice to roll, up to 10. That player's score for the turn is the sum of the dice outcomes.


To spice up the game, we will play with some special rules:


* **Pig Out.** If any of the dice outcomes is a 1, the current player's score for the turn is 0.



## Boids and Hawks

To implement the boids and hawk, make the boid follow these rules:

1. Avoid colliding with the Hawk
2. Avoid collision with neighbors
3. Match velocity with neighbors
4. Move towards center of mass of nearest neighbors.
5. Always accelerate towards the center (for viewing purposes)

Make the hawk do the following:

1. Accelerate towards the nearest boid.

Neighbors of a boid are defined as the 10 nearest boids. This is achieved with
the nearest neighbor search. The runtime of the kNN search is possibly O(k * lg N),
since k insertions into a bounded PQ is required.

By tweaking weights for boid rules 1 thru 5, proper flocking may be achieved.
The following animation is made by weights provided by Princeton's Evan Sparano
(Fall 2013).

### Credit

Evan Sparano's good acceleration values.

Algs4 Part 1 Kd-Tree vidoes, Prof. Sedgewick, Prof. Wayne, Princeton
