CMA-ES is a optimization algorithm for finding the minimum of an unknown, possibly rugged, non-differentiable function. 
## Overview
We have an expression like $f(x) = a\cdot x^2 + b \cdot x + c$. We want to find the values of $a, b, c$ that make the expression fit some data well.
CMA-ES treats this like **searching a landscape**. Each point in the landscape is a guess for the constants. 
Instead of testing one guess at a time, it uses a **cloud of guesses**, and keeps refining where that cloud is. 
This "cloud" is modeled by a multivariate normal distribution:
$$x \sim \mathcal{N}(\mu, \Sigma)$$
- $\mu$ is the center of the cloud (the current best estimate for the constants).
- $\Sigma$ is a shape matrix (tells how wide the cloud is in each direction, whether it is stretched / rotated)
