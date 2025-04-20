P is a 3-dimensional array (tensor) that is (n+1) x (n+1) x (n+1)
$P[i_1][i_2][j]$ is the probability that two parents, one with fitness $i_1$ and other with fitness $i_2$, produce a child with fitness $j$.

We use a simulation to estimate these probabilities.

## Generating the Tensor
#### For each parent fitness pair $(i_1, i_2)$, 
1. Generate the parent pairs genotypes
	1. Parent 1 has exactly $i_1$ 1's in it
	2. Parent 2 has exactly $i_2$ 1's in it 
2. Apply crossover
3. Apply mutation
4. Evaluate child fitness 
5. Repeat 1-4 for many trials to get an estimate of how often a child of fitness $j$ occurs from the parents of $i_1$ and $i_2$
6. Normalize the results to make a probability distribution
	- $P[i_1][i_2][j] = \frac{\text{number of times child of fitness j was produced}}{\text{number of total trials for (i1, i2)}}$

## Interpretation of the Tensor
Each row of the tensor $P[i_1][i_2]$ is a probability distribution over fitness level $j$.

However, this tensor is three dimensional, as it relies on both parent 1 and parent 2 states to determine the state of the child. We need to transform this 3 dimensional structure to a 2D matrix.

## Converting Tensor to 2D Markov Chain
We can convert the tensor to a 2D Markov process that tracks: How the **distribution of fitness** in the population evolves over generations.

Let $v(i)$: the **fraction** of individuals in the current population with fitness $i$. This is a probability distribution.

Our goal is to estimate the probability that a generated child has fitness $j$ in the next generation, given that parents are selected according to the current fitness distribution $v$.

We consider that:
- $v(i_1)$: prob. of selecting a parent with fitness $i_1$
- $v(i_2)$:  prob. of selecting a parent with fitness $i_2$
- $v(i_1) \cdot v(i_2)$: prob. of selecting a pair of parents $(i_1, i_2)$ (since there is replacement)
- $P[i_1][i_2][j]$: prob. that these two parents produce a child with fitness $j$ (from the tensor)

We want to get $M[j]$, which is the probability distribution that a generated child has fitness $j$. This is the **sum of all the different ways that a child of fitness $j$ can be created, from all possible combinations of parent fitness levels (all possible parent pairs)**.
Therefore,
$$M[j] = \sum_{i_1=0}^n \sum_{i_2 = 0}^n v(i_1) \cdot v(i_2) \cdot P[i_1][i_2][j]$$
This is the weighted sum of child fitness probabilities over all possible combinations of parent fitnesses.

Each $M[j]$ is a **new probability distribution**, 