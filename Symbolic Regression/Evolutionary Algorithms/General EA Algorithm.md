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
## Algorithm
1. **<u>Initial Population</u>**
	- randomly generate initial population
$$P^0 = {x_1^{0}, x_2^{0}, ... , x_{\mu}^{0}} \subseteq X$$
2. **<u>Evolution Loop</u>** 
	1. **Evaluate Fitness**
		- $\forall x \in P^{(t-1)}, f(x)$
	2. **Select Parents**
		- create a selection operator $Select(P^{t-1}, \lambda)$, which selects $\lambda$ parents from current population based on fitness
		- **Tournament Selection**: run a "mini competition" among a small group of individuals, select the winner
			- Parameters:
				- $k$ : tournament size
				- $\lambda$ : number of individuals to select overall (number of parents)
			- Procedure: 
				1. sample $k$ individuals uniformly from population $P$ 
					- $S = \{x_{i_{1}}, x_{i_{2}}, ..., x_{i_{k}}\}, x_{i_{j}} \in P$
				2. Evaluate fitness
					- $\forall s \in S, f(s)$
				3. Select the "winner" (the lowest $f(s)$)
					- $x^* = min_{s \in S} f(s)$ 
				4. Add $x^*$ to parent pool
				5. Repeat $\lambda$ times $\Rightarrow$ parent pool of size $\lambda$
			- [[EA Theory#Selection Pressure]] (the choice of k)
	3. **Variation**
		- Using the selected parents, we want to create offspring that explore the search space and combine useful traits. 
		1. Crossover:
			- Takes 2 parents, exchanges parts of their representation to create children
				- "exploitation": combining useful building blocks from individuals
			- Ex: Vectors
				- Let parents be $x_{1} = [1.5, 2.0, 3.7],  x_{2} = [4.0, 1.0, 2.5]$. 
				- Arithmetic crossover (average): child $= \alpha x_{1} + (1-\alpha)x_{2}, \alpha \in [0, 1]$
				- For $\alpha = 0.5$, child = $[2.75, 1.5, 3.1]$
			- Keeps children "in between" the parents
		2. Mutation:
			- takes the child, makes a small, random change to part of its representation
				- "exploration": venturing out into new regions of search space (prevents premature convergence)
			- Ex: small noise to a vector
				- child = $[1,2,3] \rightarrow [1.2, 1.9, 3]$ 
		- Key Params:
			- Crossover rate: % of parent pool that undergoes crossover
			- Mutation rate: chance of mutation per individual / gene
			- Mutation strength: size of mutation
	4. **Replacement**
		- after forming new offspring, who makes it to the next generation?
			- rule for forming next population from current pop. and new offspring
		- Common Strategies:
			1. ($\mu + \lambda$) replacement
				- $\mu$: current population, $\lambda$: offspring
				- We have fitness for $\mu$ already, calculate fitness for $\lambda$
				- Combine $\mu$ and $\lambda$ into one list, rank this combined list by fitness
				- Select the $\mu$ best individuals (keeps population size fixed)
			2. ($\mu , \lambda$) replacement
				- "ignore" old population, only look at offspring
				- keep $\mu$ best individuals from offspring only
				- Stronger selection pressure, promotes exploration, but risk of forgetting good individuals too quickly
			3. Steady State replacement (fitness aware)
				- compare an offspring to the worst in current pop. 
				- If offspring is better, "kill" the worst one in current pop, insert offspring. Else discard child.
				- Repeat for all offspring
		- Elitism:
			- Regardless of strategy, can force best $E$ individuals to survive to next pop
3. <u>Repeat Evolution Loop</u>, until goal is reached / $T$ generations
4. 
		