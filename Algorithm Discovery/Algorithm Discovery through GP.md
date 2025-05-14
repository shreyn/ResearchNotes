Unlike [[General Symbolic Regression]], where the goal is to evolve expressions that approximate a numeric function, here we evolve structured programs that perform algorithmic tasks such as sorting, searching, or optimization.

The central idea is to search over the space of syntactically valid programs, represented as syntax trees, using genetic programming ([[EA Theory]]). Each candidate program is evaluated on a task-specific fitness function that measures how well it performs the desired computation.

The long term goal is to evolve interpretable, efficient, and novel algorithms that solve defined tasks. 
## Problem Statement
Given a set of input-output examples for a task (ex. sorting a list), we want to discover an algorithm that:
- Produces correct outputs for all test cases,
- Is syntactically and semantically valid,
- Generalizes to unseen inputs,
- Is simple/efficient (fitness constraints)

Search space is described here: [[Search Space - Type Constraints]]

**Strongly Typed Genetic Programming (STGP)** is used, which is a variant of traditional [[Evolutionary Algorithm]].
- Mutation: a randomly generated replacement subtree must match the return type of the node it replaces
- Crossover: subtrees may only be exchanged if their return types are equal.
This ensures that every individual in the population is valid, even after variation.

