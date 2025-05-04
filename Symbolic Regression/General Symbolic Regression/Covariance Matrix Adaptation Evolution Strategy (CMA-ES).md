CMA-ES is a optimization algorithm for finding the minimum of an unknown, possibly rugged, non-differentiable function. 
## Overview
We have an expression like $f(x) = a\cdot x^2 + b \cdot x + c$. We want to find the values of $a, b, c$ that make the expression fit some data well.
CMA-ES treats this like **searching a landscape**. Each point in the landscape is a guess for the constants. 
Instead of testing one guess at a time, it uses a **cloud of guesses**, and keeps refining where that cloud is. 
This "cloud" is modeled by a multivariate normal distribution:
$$x \sim \mathcal{N}(\mu, \Sigma)$$
- $\mu$ is the center of the cloud (the current best estimate for the constants).
- $\Sigma$ is a shape matrix (tells how wide the cloud is in each direction, whether it is stretched / rotated)
The algorithm basically moves around and stretches the "cloud" based on evaluating samples of the cloud.
## Algorithm
We want to minimize a function:
$$f : \mathbb{R}^n \rightarrow \mathbb{R}$$
This $f(\mathbf{x})$ is typically the fitness of a symbolic expression evaluated using constants $\mathbf{x} \in \mathbb{R}^n$.
#### Initialization
- Mean (center of search):
$$\mathbf{m}^{(0)} \in \mathbb{R}^n$$
	- Starting point of the search
- Step size:
$$\sigma^{(0)} > 0$$
	- Controls the overall spread of the sampling distribution (like the radius of the search ball)
- Covariance Matrix:
$$\mathbf{C}^{(0)} = \mathbf{I}_n$$
	- Start with identity (assume all variables are uncorrelated and equally scaled)
	- As the algorithm progresses, this matrix will learn variable dependencies and scale directions.
- Evolution Paths (tracks momentum):
$$\mathbf{p}_{\sigma}^{(0)} = 0, \hspace{1em} \mathbf{p}_{c}^{(0)} = 0 $$
	- Used to help smooth out the trajectory in updates 
#### Iterate for $t$: 
1. Sampling new candidate solutions
	- For population size $\lambda$, sample $\lambda$ new candidate constants:
$$x_k^{(t)} = m^{(t)} + \sigma^{(t)}\cdot z_k^{(t)}\hspace{0.5em} \text{with} \hspace{0.5em} z_k^{(t)} \sim \mathcal{N}(0, \mathbf{C}^{(t)}) \hspace{0.5em} \text{for} \hspace{0.5em} k = 1, \dots, \lambda$$
		- $z_k^{(t)}$: sampled "direction + shape" from Gaussian
		- $\sigma^{(t)}$: how large a step to take
		- $x_k^{(t)}$: new guess for constants
	- We draw random samples from a normal distribution centered at current mean and shaped by the current covariance
- 