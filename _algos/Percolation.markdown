---
layout: other 
title:  Percolation
description: Solution to the backwash without 2 Quick Unions.
date:   2016-01-12 08:00:00 -0800
---

## Problem Statement
Here are the rules of the Percolation Problem from Princeton:

**The model**. We model a percolation system using an N-by-N grid of *sites*. Each site is
either *open* or *blocked*. A *full* site is an open site that can be connected to an open site
in the top row via a chain of neighboring (left, right, up, down) open sites. We say the system
*percolates* if there is a full site in the bottom row. In orther words, a system
percolates if we fill all open sites connected to the top row and that process fills some
open site on the bottom row. (For the insulating/metallic materials example, the open sites
correspond to metallic materials, so that a system that percolates has a metallic path from
top to bottom, with full sites conducting. For th porous substance example, the open sites
correspond to empty space through which wter might flow, so that a system that percolates lets water
fill open sites, flowing from top to bottom.)

![Percolation Example](/files/percolates.png)

## Simplistic Solution
A simplistic approach involves creating a column-spanning top site and a column-spanning bottom
site. By connected every single open site in row 1 to the top site and every single open site in row N
with the bottom site, checking to see if the grid percolates is just a call to connected(top, bottom);
if top is connected to bottom, the system percolates.


## Issues with Simplistic Approach (BACKWASH)
The issue arrises when we try to ask if an open node is filled. With the implementation of
WeightedQuickUnionUF, to determine if a node is full, we ask if node *i* is connected to the top. This works until the system percolates. Once it percolates, every node connected to the bottom node that has
no direct path to the top will be connected to top, since unions are commutative; node is connected
to bottom is connected to top. This creates backwash.

## Solutions to Backwash
Prof. Sedgewick [metions](https://www.cs.princeton.edu/courses/archive/fall10/cos226/precepts/15UnionFind-Tarjan.pdf)
that a possible solution is to abandon top and bottom sites and store two bits with each root element of a tree of connected components.
The first bit is true if the tree has a node in the top row. Bit two will be true if the tree has a
node in the bottom row. Updated these values, according to Sedgewick, takes constant time per union.
Once a node has both bits true, we will know the system percolates. Additional calls to union will not
change the fact that the system percolates. Unfortunately this solution requires modifying the original
API.


Another simple solution involves using two Union finds. One that has a bottom site and one that does not.
This however introduces memory problems, since there is twice as much data in circulation, much of it
duplicate.

A solution that does not require modifying the original API yet also keeps memory in check utilizes
Prof. Sedgewick's idea. The solution involves the following:

1. Note that *i* and *j* are zero indexed for simplicity (not 1-indexed).
2. Upon construction create an NxN array of chars, called stats. This is responsible for storing if
an *i*, *j* site is connected to top, bottom, both, neither, or closed.
3. Do not designate a top nor bottom global site.
4. Initialize a boolean, *percolates*, to false.
5. On *open(i, j)*:
	1. Check the open neighbors of (*i*, *j*) site. Find those neighbors' roots.
	2. For each neighbor root, see if any are conected to the top, bottom, or both do this by checking
	the stats matrix.
	3. Check if (*i*, *j*) is at the top or bottom row
	4. Determine if connecting (*i*, *j*) to its neighbors would connect it to the top, bottom, or
	neither
	5. Union (*i*, *j*) with its neighbors. Then, set the new root of (*i*, *j*) to the proper char
	value in the stats matrix.
	6. If (*i*, *j*)'s root is now connected to top and bottom, set percolates to true
6. On *percolates()*, return the boolean *percolates*
7. On *isFull(i, j)* check stats for (*i*, *j*). Check to see if the site is connected to the top.

## Credits
[This](http://www.sigmainfy.com/blog/avoid-backwash-in-percolation.html) post from sigmainfy.com

[Rober Sedgewick's notes](https://www.cs.princeton.edu/courses/archive/fall10/cos226/precepts/15UnionFind-Tarjan.pdf)

[Princeton Percolation Problem](http://coursera.cs.princeton.edu/algs4/assignments/percolation.html)
