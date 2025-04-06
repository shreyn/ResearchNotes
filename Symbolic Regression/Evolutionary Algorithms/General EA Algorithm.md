___
## Background
- inspired by Darwinian evolution: organisms better adapted to an environment tend to survive and reproduce, passing on their "good traits"
	- Over time, population becomes "better"
- Let:
	- $X$ : search space (set of all valid solutions)
	- $f: X \rightarrow \mathbb{R}$ : fitness (error) function
	- $\mu$ : size of population
	- $\lambda$ : number of offspring generated
	- $T$ : total number of generations
- Trying to optimize: $x^* = min_{x \in X} f(x)$
- Since $X$ is very large / unstructured / discrete, we cannot search exhaustively / with traditional techniques
	- EA provides a stochastic search procedure
---
## General Algorithm
1. **<u>Initial Population</u>**
	- randomly generate initial population
$$P^0 = {x_1^{0}, x_2^{0}, ... , x_{\mu}^{0}} \subseteq X$$
2. **<u>Evolution Loop</u>** 
	1. Evaluate Fitness
		- $\forall x \in P^(t-1), f(x)$
	2. Select Parents
		- create a selection operator $Select(P^{t-1}, \lambda)$, which selects $\lambda$ parents from current population based on fitness
		- **Tournament Selection**: run a "mini competition" among a small group of individuals, select the winner
			- Parameters:
				-$k$ : tournament size
				-$\lambda$ : number of individuals to select overall (number of parents)
			- Procedure: 
				1. sample $k$ individuals uniformly from population $P$ 
					- $S = \{x_{i_{1}}, x_{i_{2}}, ..., x_{i_{k}}\}, x_{i_{j}} \in P$
				2. Evaluate fitness
					- $\forall s \in S, f(s)$
				3. Select the "winner" (the lowest $f(s)$)
					- $x^* = min_{s \in S} f(s)$ 
				4. Add $x^*$ to parent pool
				5. Repeat $\lambda$ times $\Rightarrow$ parent pool of size $\lambda$
				6. 
		