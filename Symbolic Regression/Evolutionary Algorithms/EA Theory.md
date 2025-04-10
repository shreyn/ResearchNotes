## Schema Theory (Fundamental Theorem of EA)
- Let $X = \{0, 1\}^l$ be a space of binary strings of length $l$. A **schema** is a template for $\{0, 1, *\}^l$, where $*$ is a wildcard ($0$ or $1$). 
	- Ex: $H = 1 * 0 * 1 = \{10001, 11001, 10011, 11011\}$
- Define:
	- $o(H)$ : *order* of schema $H$: number of fixed positions in the template. Ex: $o(H) = 3$.
	- $\delta(H)$ : *defining length* of schema $H$: distance between fixed positions. Ex: $\delta(H) = 5$. 
	- *fitness of schema*: average fitness of all strings matching the schema.
	- $m(H, t)$ : number of individuals at pop. $P_t$ that match schema $H$.
	- $\bar f(t)$ : average fitness of population at generation $t$. 
	- $\hat f(H)$ : average fitness of individuals matching schema $H$. 
	- $p_c$ : prob. of crossover occurring
	- $p_m$ : prob. of mutation occurring
	- Higher fitness is better
- Statement:
$$E[m(H, t+1)] \geq \left(m(H, t) \cdot \frac{\hat f(H)}{\bar f(t)} \cdot \left(1-p_c \cdot \frac{\delta (H)}{l-1}\right)\cdot \left(1-p_m\right)^{o(H)}\right)$$
	- Breakdown:
		- $E[m(H,t+1)]$ : expected number of individuals matching schema $H$ at generation $t+1$. 
		- $m(H,t)$ : number of individuals currently matching schema $H$ (starting count before selection, variation)
		- $\frac{\hat f(H)}{\bar f(t)}$ : selection pressure. 
			- $\Rightarrow$ If $\hat f(H) > \bar f(t)$ (schema fitness > overall fitness), the schema is more likely to be selected for reproduction 
		- $1-p_c \cdot \frac{\delta(H)}{l-1}$ chance that schema $H$ is preserved during crossover
			- $l-1$ : number of possible crossover points
			- If crossover occurs, the schema survives iff the crossover point falls *outside* the defining length
			- $\frac{\delta(H)}{l-1}$ : chance that crossover point falls within defining length (disrupting schema)
			- $p_c \cdot \frac{\delta(H)}{l-1}$ : prob. that crossover occurs and disrupts schema
			- $\Rightarrow$ Penalizes longer schema (larger $\delta (H)$), since the crossover point has more opportunity to land in the wider schema region. 
		- $(1-p_m)^{o(H)}$ : prob. that mutation does not affect schema $H$.
			- $1-p_m$ : prob that each bit does not mutate
			- raising to $o(H)$ gives prob that all of the fixed positions don't mutate.
			- $\Rightarrow$ higher order schema are more likely to be disrupted by mutation (more fixed positions, more spots for disruption)
	- In other words,
		- The expected number of those matching schema $H$ in the next generation is at least: the number that match $H$ now, times the relative fitness of the schema, times the chance it survives crossover, times the chance it survives mutation.
		- ==$\Rightarrow$ short, low-order schema with above average fitness tend to survive and grow==
			- these "good traits" will spread $\Rightarrow$ Building Block Hypothesis
	- **Building Block Hypothesis**:
		- building blocks (short, low order schema with high fitness) are the basic units of good solutions
		- because of the design of EA, the algorithm preserves, amplifies, and combines these building blocks
		- $\Rightarrow$ even without knowing the structure of $f$, EA can converge on good solutions
- Schema Theory gives a theoretical justification for how EAs intelligently search. They exploit patterns (schemata).
	- EA biases towards structures that have good fitness and that are compact and stable
	- Over time, these good traits are amplified, so EA converges to the optimal solution.
---
## Selection Pressure
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
		- What is the prob that a group of $k$ individuals includes someone of rank $r$, and no one better?
			- $P_{r}$(best rank in group is $r$) = $P_r$ (everyone in group is <u>rank r or worse</u> ) $-$ $P_r$ (everyone in group is <u>strictly worse than rank r </u>) 
			- The difference is therefore prob when rank $r$ is in the group, and it is the best
	- <u>Formula</u>:
		- $\mu$ total individuals, ranked from 1 to $\mu$
		- $\frac{r}{\mu}$: fraction of pop. that is better than or equal to ($\leq$) r. 
			- Ex: $\mu = 100, r = 30. \frac{r}{\mu} = \frac{30}{100} = 0.3 \Rightarrow 30$% of pop. is ranked 30 or better
		- $1- \frac{r}{\mu}$ fraction of pop. that is strictly worse than ($>$) r.
		- $\frac{r-1}{\mu}$: fraction of pop. that is strictly better than (<) r. 
			- Ex: $r-1 = 29$, so $\frac{r-1}{\mu} = \frac{29}{100}$  (strictly better than 30)
		- $1-\frac{r-1}{\mu}$ fraction of pop. that is worse than or equal to ($\geq$) r. 
			- Ex: $1 - \frac{29}{100} = \frac{71}{100}$ (includes r, plus everyone who is higher)
		- Therefore,
$$P_r = \left(1-\frac{r-1}{\mu}\right)^k - \left(1-\frac{r}{\mu}\right)^k$$
----
## No Free Lunch Theorem
- $\Rightarrow$ ==There is no universally best optimization algorithm. Averaged across all possible problems, every optimizer performs equally.== 
	- Holds true for any optimizer
- If an algorithm works well on some problems, it will perform worse on others.
- This means optimizers *must* exploit specific function structure to perform well.
- In the context of EA, crossovers, mutations, and selection must align with problem-specific properties.
---
## Drift and Premature Convergence
- **Genetic Drift**: random fluctuations in genotype frequencies, even without selection pressure, that dominate
	- causes some traits to be chosen more purely by chance
- Over time, drift can lead to loss of some genotypes, and these can't return unless mutation reintroduces them.
	- reduces genetic diversity
- **Premature Convergence**: (local optima)
	- all individuals become similar, so crossovers become ineffective
- Therefore, it is important to maintain strong mutations to promote diversity. 
---
## Markov Chains
- <u>Finite Stochastic Processes</u>
	- $S$ : a finite set of state spaces (set of all possible populations)
	- $X_t$ : state of a system at time $t$ (population at time $t$). 
	- $\{X_t\}_{t \in T}$ : stochastic process over each $t \in T$ (evolution of populations over time)
- <u>Markov Chain</u>
	- $\Rightarrow$ a finite stochastic process that satisfies the **Markov Property**:
	-  $\forall s_0, s_1, ..., s_t, s_{t+1},$ 
$$P(X_{t+1}=s_{t+1} \mid X_t = s_t, X_{t-1} =  s_{t-1}, ..., X_0 = s_0) = P(X_{t+1} = s_{t+1} \mid X_t = s_t)$$
		- The probability of the next state only depends on the current state (memoryless)
	- In EAs, the generation $t+1$ is computed only using generation $t$. 
		- Parents are selected from $X_t$
		- Variation is applied to offspring
		- Replacement forms $X_{t+1}$
		- Therefore, EA is a Markov Chain.
- <u>Transition Probabilities</u>
	- For any $i, j \in \{1, 2, ..., T\},$ the **transition probability** is: 
	$$P_{ij} = P(X_{t+1} = s_j \mid X_t = s_i)$$
		- $\Rightarrow$ the prob. that the system moves to state $s_j$ at time $t+1$, given it was at state $s_i$ at $t$.
	- Properties:
		- nonnegative: $0 \leq P_{ij} \leq 1$
		- total probability is 1: $\sum^{n}_{j = 1} P_{ij} = 1$ 
			- the system must move *somewhere* in the next generation
- <u>Transition Matrix P</u>
	- Arrange transition probabilities in a matrix:
$$P = \begin{bmatrix}P_{11} & P_{12} & \cdots & P_{1T} \\ P_{21} & P_{22} & \cdots & P_{2T} \\ \vdots & \vdots & \ddots & \vdots \\ P_{T1} & P_{T2} & \cdots & P_{TT}    \end{bmatrix} \in \mathbb{R}^{T\times T}$$  









