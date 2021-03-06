---
layout: other 
title:  Kd Tree + Boid Animations
description: Simulation of a flock of 10k boids + 1 hawk with Kd-Tree implementation.
date:   2017-01-12 11:47:12 -0800
---

## Videos

### 10,000 Boids Video 1, 1 hawk
<div class="videoWrapper">
<iframe src="https://drive.google.com/file/d/0B3JVHi3wgViAUUJveGFoVzBsWkU/preview" width="720" height="720"></iframe>
</div>

### 10,000 Boids Video 2, 1 hawk
<div class="videoWrapper">
<iframe src="https://drive.google.com/file/d/0B3JVHi3wgViAb3VxR1ZvSGRlTmM/preview" width="720" height="720"></iframe>
</div>

### 10,000 Boids Video 3, 100 hawks WIP

## Description

Kd trees allow for geometric search in O(lg N) time. This is the concept: Upon
every insertion into the KdTree structure, a node is added to a binary search
tree. At every even numbered height, the new point is less than the point at the
node if it falls to the left of the point. At every odd numbered height, less is
defined by being below the point at the compared node. To demonstrate, here's an
image.

<center>
	<img class="img-rounded img-responsive" src="/files/kdtree-tree.svg" alt="Kd Tree Diagram">
</center>

The see if the tree contains point, do a binary search. This operation is trivial.

To find the nearest neighbor to a query point q. A recursive search is required.
 First check to see if the current node (starting from the root) beats our
 current 'champion' point. If it does, replace it. Then search the
 side of the tree that query point is on (repeat this recursive search on that
 appropriate child). Then check to see if the unsearched child could
 contain a closer point than the champion. This is done in practice by
 associated a rectangular area with each node that denotes its
 children's points could like. For instance the root node should
 have a rect that spans all the 2D plane. The left child should have a rect
 that covers everywhere where x-coord is less than root.point.x. The right
 side should have something similar. If query->champion distance is greater
  than query->rect_of_unsearched_child distance, then search that child node.

Implementing the k-Nearest Neighbor search is an extension of the nearest
neighbor search. Merely keep a bounded Max Priority Queue of points where the
key is the distance between the point to the query point. Make sure the queue
does not exceed size k by expelling the max node every time an exceed occurs.

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
