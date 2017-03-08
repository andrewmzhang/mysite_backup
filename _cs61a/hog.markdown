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
* **Piggy Back.** When the current player scores 0, the opposing player receives points equal to the number of dice rolled that turn.
* **Free Bacon.** A player who chooses to roll zero dice scores one more than the largest digit in the opponent's total score.
* **Hog Wild.** If the sum of both players' total scores is a multiple of seven (e.g., 14, 21, 35), then the current player rolls four-sided dice instead of the usual six-sided dice.
* **Hogtimus Prime.** If a player's score for the turn is a prime number, then the turn score is increased to the next largest prime number. For example, if the dice outcomes sum to 19, the current player scores 23 points for the turn. This boost only applies to the current player. Note: 1 is not a prime number!
* **Swine Swap.** After the turn score is added, if the last two digits of each player's score are the reverse of each other, the players swap total scores.



## Solve

This problem is recognizable as an Expectimax Tree. Simply put, an expectimax tree is consisted of state nodes and action nodes. The tree is rooted at a state node indicating the scores for both players at zero and zero, as well as who is about to play. Each state either offers a set of possible action nodes or evaluates to a discrete number of points which represents the number of points expected (but not guaranteed) should a user reach that state. Each action node has a value equal to the following summation:

<div lang="latex">
	$action's exp value = $Value(action) = \sum_{n=0}^{ps} expPoints(state_{n}) * prob(state_{n})\\
	\\
	\indent ps$ = possible states from current action$ \\
	\indent state_{n}$ = nth state visitable from action$ \\
	\indent expPoints(x)$ = number of expected points when visiting state $x \\
	\indent prob(x)$ = probability of reaching state $x$ when taking this action$ \\
	\\
</div>







Or, if one prefers a solve with Dynamic Programming:

Here’s my logical deduction on how a final strategy can be developed:

   First, we recognize that given a score and opponent’s score, there exists an optimal number of rolls. In other words, a best strategy exits for every score and opponent score

   a. In theory you could devise a 100 x 100 matrix and try every single combination of 0 – 10 in each of the elements and see which configuration gives you the highest avg score. However, there are about 11^10,000 combinations, which is too many to solve for…

   The best number of rolls must the be number of rolls that maximizes our chances of winning. This is the definition of the best number of rolls.

   Given 1 and 2, there must be a function such that, given score, opponent’s score, and number of dice to roll (k), it returns the probability of winning by rolling k number of dice.

   For the function works as follows: iterate through all possible points score-able by rolling k dice, find the (probability of scoring p with k dice) * (probability of winning with p points), and sum up all values. The sum is the probability of winning by rolling k dice.

   a. The probability of scoring p points with k dice is a simple statistical problem.

   b. The probability of winning with p points is a simple logical problem, described below:

       I. If my score + p points >= 100, I’ve already won. Return 1

       II. If my opponent’s score >= 100, I’ve already lost. Return 0

       III. If it’s not I or II, then I’ve made my move, but no one has yet won. Now my opponent will roll d dice, where d is the optimal number of dice for him/her (or 5, if you are playing vs the AI). The probability we will win is the probability the opponent will not win with d dice, or 1 – probability opponent wins by rolling d dice.

   Be aware: watch out for recursion depth, as it tends to get large. Also try storing things into tables so that we do not waste time calculating redundant things. aka Memoization

   Be aware: of the special rules and when they apply. I got really badly tripped up since I didn't pay attention to the piggy back and piggy out rules. An interesting note is if one ignores the piggy back rule, a minimax tree will be unable to solve the problem, since there will be loops in the tree. Solving will require a Markov Decision Process.
