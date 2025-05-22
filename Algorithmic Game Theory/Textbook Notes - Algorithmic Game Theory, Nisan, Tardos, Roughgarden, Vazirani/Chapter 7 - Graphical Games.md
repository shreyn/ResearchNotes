## 7.1 Introduction
Multiplayer games with large number of players are hard to represent.
**Graphical games represent multiplayer games where there are large populations, and the payoffs of each player is determined by the actions of a small subpopulation (neighbors).**
## 7.2 Preliminaries
- A multiplayer game has $n$ players, each with a finite set of pure strategies/actions. 
- $a_i$ is the action of player $i$. For simplicity, assume a binary action space ($a_i \in {0,1}$). 
- The payoffs for player $i$ is given by matrix $M_i$
	- $M_i(a)$ is the payoff to player $i$ from the action $a$.
- The action 0 and 1 are pure strategies. A mixed strategy is a probability $p_i \in [0,1]$ that the player will play 0.
- $p[i:p'_i]$ is the prob. distribution which is the same as $p$, except in the $i$th component, where the value has been changed to $p'_i$ .
- A Nash Equilibrium (NE) is a mixed strategy $p$ s.t. for all players $i$, and any value $p'_i \in [0,1]$,  $M_i(p) \geq M_i(p[i:p'_i])$.
	- I.e. no player can improve their expected payoff by deviating from the NE. 
	- An approximate NE is one where no player can improve their payoff by more than some $\epsilon$. 
**Graphical Game**:
- Each player $i$ is represented by a vertex in a graph $G$. 
- $N(i) \subseteq \{1, \dots, n\}$ is the neighborhood of player $i$
	- i.e. the vertices $j$ s.t. the edge $(i,j)$ appears in $G$
- $a$ is a vector where $a_i$ is the strategy of $i$
- $a^i$ is the projection of $a$ onto the players in $N(i)$
	- i.e. $a^i$ contains only the strategies of player $i$ and its neighbors
- **Def**: A graphical game is a pair $(G, \mathcal{M})$, where $G$ is a graph over the vertices $\{1, \dots, n\}$, and $\mathcal{M}$ is a set of $n$ local game matrices. 
	- $\mathcal{M} = \{M_1, \cdots M_n\}$. Each $M_i$ is player $i$'s local payoff function
	- The input to $M_i$ is not the full $a$, but instead the local projection $a^i$ (strategies of $i$ and its neighbors)
	- Therefore, $M_i(a^i)$ is the payoff for any player $i$, which depends only on the actions taken by the players in $N(i)$.
**Correlated Equilibria (CE)**:
In CE, players can coordinate their strategies via a shared random signal (trusted third party).
- A trusted party samples a joint action
	- The party uses a probability distribution $P$ over all players' actions (joint distribution)
	- Picks one joint action $a$
	- Gives each player $i$ just their own action $a_i$ (a suggestion)
- Should the player follow the suggestion?
	- The player knows the distribution $P$, and they were told to play $a_i$.
	- They ask "should i really play a_i? What if i deviate?"
- CE condition: It must be in player $i$'s best interest to follow the suggestion $a_i$.
	- i.e. payoff of following $a_i \geq$ payoff of deviating to some other action $a'_i$
	- 