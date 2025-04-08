# Selection Pressure
- $\Rightarrow$ how strongly the EA favors better performing individuals (given a group of individuals, who gets to be a parent?)
	- high pressure = fast convergence, risk of local minima; low pressure = slow progress, higher diversity
	- Ex: Let population have fitness: 
		- A: 0.1 (best), B: 0.2, C: 0.5, D: 1.0, E: 2.0 (worst)
		- Let $k = 2$ 
			- For each parent we want, randomly pick 2 individuals (with replacement, so there can be 2 of the same in the same group), choose the one with better fitness. 
		- How often does A win? (3 cases)
			1. A is picked twice $\rightarrow$ A always wins $\rightarrow$ $P_{AA} = (\frac{1}{5})^{2} = \frac{1}{25}$ 
			2. A, someone else $\rightarrow$ A wins, since it is the best $\rightarrow$ $P_{AB}+ P_{AC} + P_{AD} + P_{AE} = [(\frac{1}{5} \cdot \frac{1}{5}) + (\frac{1}{5} \cdot \frac{1}{5})) \cdot 4 = \frac{8}{25}$ 
			$\Rightarrow$ P(A wins) = $\frac{1}{25} + \frac{8}{25} = \frac{9}{25} \approx 0.36$ 
		- Doing the same for B, notice prob. decreases $\Rightarrow$ as rank gets worse, prob decreases
- **General Formula for Rank-Based Selection**
	- <u>Intuition</u>:
		- $\mu$ individuals, ranked $r$ from 1 (best) to $\mu$ (worst). $P_{r}$ is probability that individual of rank $r$ wins a tournament
		- In other words, how often rank $r$ is the best in $k$-sized groups
			- Depends on how often $r$ is sample $\wedge$ who else is sampled with it
		- ==Being chosen as a parent = _r_ being included in _k_-sized group, AND _r_ wins (highest rank among that group)== 
		- To find this probability, it is a kind of set difference:
			- What is prob of rolling a 3 on a die?
				- $P[x=3] = P[x\geq 3] - P[x \geq 4]$
					$\Rightarrow$ $P[x\geq 3]: \{3, 4, 5, 6 \}, P[x\geq 4]: \{4, 5, 6\}$ 
					$\Rightarrow$ $P[x \geq 3] - P[x \geq 4] = P[3]$
			- 