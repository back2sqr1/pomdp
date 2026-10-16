# Notes for POMDPs

POMDP - partially observable Markov Decision Processes (POMDPs).

Large term effects are not always transparent - some actions have long term ramifications

You would like to choose the action that makes the right tradeoffs for immediate rewards and future gains.

MDP (Markov decision process) is a way to model problems for automating decision making in uncertain environments

Policy - best action

Markov assumption - the next state is soley determined by the current state and not previous

## Value Iteration

value iteration tries to find the value function for a horizon of 1.
This means the value of a state is given by a single decision. (No future effects calculated).

Horizon length 2 means you calculate the action from 2 steps ahead

Use probability to get expected value of uncertain actions

## What is a POMDP

MDPs and CO-MDP (completely observable) are synonymous

Partial observability clouds the idea of a current state. We don't know what state we are in.

POMDPs are MDPs with observations that give hints to what state we are in.

Observations are in the form of an observation model. Which is a function that tells us the probability of each observation for each state.

We have a probability distribution over all states. We perform an action then update the distribution.

## Solving a POMDP

For CO-MDPs we map states to actions
For POMDPS we map probability distributions over states to actions

Probability distributions over states and belief states are the same.

The entire probability space is known (the set of all possible probability distributions) as the belief space.

### Relationship between state, action, and observation

States come after actions, and can only be determined with certainty after an observation. So you go from state -> action -> observation. Before you make an action, there are a bunch of belief states (probabilities you are either in one state or another).

For example, for a two state system, your probability you are in state s1 is $p$, and $1-p$ in state $s_2$. If you imagine your belief state as a point on a line, your actions give you probabilities to be on points on the line.

### Converting from POMDP to CO-MDPs

POMDPs become continuous CO-MDPs over a belief space. Which means we can run value iteration on them.

Being continuous is a problem because you can't map an infinite space. However, we constrain the problem in that the value iteration will have a finite number of linear segments.

Imagine the value function being a function over the belief space (remember they are probabilities), so you plug in a probability of being in state s1, and you get a value of being there.

This function can be restricted to be composed of only a collection of line segments.

Each of these line segments can be hyper planes in the belief space. Each hyperplane is a vector (normal). You get the value by plugging in the value of the equation of the hyperplane. So your value is the dot product of the belief state (probability) to the hyper plane (line).

In order to find your value, you get the largest value after a bunch of plugging.

## Working with infinite probabilities

Given $n$ states and $m$ actions and $k$ observations, there are $mk$ new belief states it could be.

### Computing a belief state value from an action and observation

Given a belief state $b$ and doing action $a_1$ and getting observation $z1$. Given these fixed values, we just transform our belief state from $b$ to $b'$.

Your immediate reward is the point on the line that corresponds to your belief state.

### Computing all belief state values for an action and observation

We transform our belief state and use the horizon 1 value function. This transformation also makes a PWLC (piecewise linear combination).

### Computing belief state value for a single action

It becomes calculating the expected value of values from new belief states after an action.

### Calculating the best policy

After certain observations, they induce different actions. So there are partitions of best actions, which result in an overall value function which is also a PWLC.

## Solution Form

The solution comes in the form of $V'$, the set of vectors (lines) we desire for the next stage of value iteration.

Each vector has a particular strategy over a horizon $h$.

## Sondik / Monahan's Enumeration

For each observation it selects a vector from $V$ and transforms it vectors to construct a potential vector for $V'$

## Sondik's One-Pass Algorithm

Start with an arbitruary belief point, define a set of constraints over a belief space where the vector is guaranteed to be dominant.

You refine your estimate over a period of regions based on sets of constraints where the best action remains the best action. This allows for the final region to be optimized by linear opt.

## Cheng's Linear Support Algorithm

Starts with Sondik's idea but less constraints. It picks a point, generates a vector for that point then checks the region to see if it is correct.

Generate all possible region cornerrrs.
