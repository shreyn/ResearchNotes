## 1.1 Games, Old and New
### Prisoner's Dilemma
![[Pasted image 20250518184711.png]]
- The only stable solution is that both players confess (in the other 3 cases, a player can switch to confessing and have a better outcome).
- However, the best option for both players is both silent, but this is not stable, since there is always the fear that the other person will betray.
#### Ex: ISP Routing
![[Pasted image 20250518185201.png]]
- Suppose ISP 1 wants to send data from s1 to t1. Acting selfishly, they would follow the shortest path to ISP 2, and then ISP 2 would have to send the data a much further distance, even though ISP 1 could have gone in a straight line (shortest overall distance)
- Same concept as Prisoner's Dilemma - both ISPs would continue to send data through their selfish routes
### Tragedy of the Commons
#### Pollution
- Prisoner's dilemma for many participants
- Suppose there are $n$ countries, where each country can choose either to pollute or not pollute (pass regulations). Polluting creates a cost of 1 for all the other countries, not polluting causes a cost of 3 for that specific country.
- Suppose $k$ countries pollute. Then the cost to these countries is $k$. The cost of the remaining countries (not pollute) is $k + 3$. 
- The only stable solution is that all countries pollute ($n$ cost for all countries), but all countries would have been better off if they all didn't pollute ($3$ cost per country, assume $n > 3$).
The above games have a unique optimal strategy regardless of what other players play.
#### Tragedy of the Commons
- Suppose $n$ people want to use a shared resource (bandwidth of 1). Suppose each person $i$ can choose to use some bandwidth $x_i \in [0,1]$. Assume that the quality of the bandwidth decreases as more is used (rival good). 
- The players will overuse the common resource s.t. the total value of the resource is less than optimal
### Coordination
Multiple outcomes can be stable.
#### Battle of the Sexes
 - Boy prefers baseball, girl prefers softball, but they both prefer to play together (not different games).
 - Both baseball or both softball are stable.
#### Routing congestion
- Both players have to send traffic through either route A or route B. Route A is slightly shorter (better) than route B.
- However, if both players send traffic through same route, there is congestion, and they both suffer.
### Randomized (Mixed) Strategies
Not all games have stable solutions.
#### Penny game
- Two players each have a penny. They choose either Heads or Tails.
- Player 1 wins if they match, Player 2 wins if they don't match.
- There is no optimal strategy; players should just randomize their choices.

## 1.2 Games, Strategies, Costs, and Payoffs
Now we will define games formally. The games above are all **one-shot simultaneous move** games, where all players simultaneously choose a decision from a set of choices.
### Defining a Simultaneous Move Game
- Let a game consist of $n$ players. Each player $i$ has a set of possible strategies $S_i$.
- Each player selects a strategy $s_i \in S_i$, and the strategies selected by all the players can be represented as a vector $s = [s_1, \dots, s_n]$. 
- To describe a game, we give each player a preference ordering on these outcomes (done by a T, R binary relation on $S$).
- An easy way to find preferences is to assign a value for each player for each outcome. This can be done through costs or payoffs (interchangeable since cost = -payoff)
### Standard Form Games and Compactly Represented Games
- Standard form: listing all possible strategies and associated payoffs
	- Is convenient for small player games
- However, for most games, the payoffs can  exponentially increase (routing paths) / infinite (bandwidth example).
- There are ways to avoid these descriptions (payoffs depend on some players, etc)
## 1.3 Basic Solution Concepts
Will formalize the idea of "stability" in section 1.1. 
### Dominant Strategy Solution
Def: each player has a unique best strategy, independent of the strategies played by the other players.
- We would like to use **mechanism design** to design games that have nice dominant strategy solutions.
### Vickrey Auction: Designing Games with Dominant Strategy Solutions
**This is pretty interesting**
Suppose we are faced with designing an auction to sell something.
Setup:
- Assume each bidder $i$ has a value $v_i$ for the painting.
- The payoff for not winning is 0, and payoff for winning at price $p$ is $v_i - p$.
- We assume each bidder places bid in an envelope at the same time, and we decide who wins using the envelopes.
How would we design a game for this?
- Simplest way: winner is the highest bidder, charge him his bid. 
- This does not have a dominant strategy (the player's strategy depends on what he assumes about the strategies of other players, results in unpredictable behavior).
**Vickrey's mechanism** (second price auction) solves this.
- Winner is highest bidder, but he pays the value of the second highest bid (not his highest bid).
- Each player's dominant strategy is to report his true value (telling the truth) as the bid, independent of the other players.
	- How? Suppose you value at $100. If someone bids less than 100, then you win, pay their price (so you profit). If someone bids more than 100, you lose, pay nothing (profit 0). Either way, you never lose money (overpay).
Some properties of Vickrey auction:
1. Winner is the one who values it the most
	- Goal is to design s.t. selfish players lead to socially optimal outcome.
2. Dominant strategy games like Vickrey is very simple to play.
	- One can play the game by just asking for all the player's valuation functions, and basically play the game for them. (revelation principle)
### Pure Strategy Nash Equilibrium
Games rarely have dominant strategy solutions.
**Nash Equilibrium**: no player can change their strategy and improve their payoff, assuming other players have same strategy. It is in every player's best interest to stick to their strategy.
- Dominant strategy are Nash equilibria
- There can be multiple Nash equilibria
### Mixed Strategy Nash Equilibria
Pure strategy equilibria are those where each player deterministically plays his strategy.
However, some games do not have pure strategy equilibria (pennies game).
However, if players are allowed to randomize, and each player picks either strategy with prob. of 0.5, then there is a stable solution
- The expected payoff of each player is now 0, and neither player can improve by choosing a different randomization.
**Mixed strategy**: Suppose each player chooses a prob. distribution over his set of possible strategies.
- Nash proved that any game with a finite set of players, finite set of strategies, has a Nash equilibrium of mixed strategies.
### Games with no Nash Equilibria
Games with infinite players or infinite strategies may not have Nash equilibria.
#### Ex: Pricing Game
Suppose there are 2 players selling to 3 buyers. Each buyer wants to buy 1 unit of product. Buyers A and C only have access to one seller. Buyer B can buy from any of the two sellers.
The sellers set a price $p_i \in [0,1]$. Buyers A and C buy from sellers 1 and 2, respectively. Buyer B buys from the cheaper seller (ties going to seller 1).
- One strategy is for each seller to set price of 1, which guarantees income of 1 from A or C (they don't have a choice). 
- Or, they can also try to compete for B (but they aren't allowed to set different price to B and A/C). 
	- Each player has uncountably many available strategies ($[0,1]$), and it doesn't have a Nash equilibrium.
### Correlated Equilibrium
#### Ex: Intersection (like a STOP sign)
Two players drive up to an intersection at the same time. If both cross, there is an accident. Successfully crossing has payoff of 1, not crossing pays 0, accident is -100.
- This has 3 Nash equilibria: two are letting one car cross, the other is a mixed equilibrium where both players cross (very small prob.)
In correlated equilibrium, a coordinator can choose strategies for both players, but the strategies have to be stable (it is in each player's interest to follow the strategy). 
- This assumes some external trusted coordinator, etc.
## 1.4 Finding Equilibria and Learning in Games
How easy is it to find an equilibrium, and how does natural gameplay lead players to equilibrium?
### Complexity of Finding Equilibria
Will be discussed more in Chapter 2, 3. 
### Two-Person Zero-Sum Games
Def: For a choice of strategies, the sum of the payoffs of the two players is zero.
Linear programming can be used to find solutions to two-person zero-sum games.
### Best Response and Learning in Games
It should be the case that natural game playing strategies will lead to the equilibrium over time.
One method of game playing is following the best response.
- Repeatedly allow each player to make an improving or best response (where utility of new strategy > utility of old strategy)
## 1.5 Refinement of Nash: Games with Turns and Subgame Perfect Equilibrium
Many games have turns of moves (card games, board games).
The problem is that Nash is a little difficult to understand in these games.
#### Ex: Ultimatum Game
- Seller S tries to sell to Buyer B. 
- First, seller S offers price $p$. Then Buyer B reacts to the price.
	- Seller gets payoff $p$. Buyer has value $v$, so payoff is $v-p$ if buys, 0 if doesn't buy.
- Assume there is full information (the seller knows $v$, so they would just offer price just under $v$)
	- This leads to the first player (seller) to collect almost all of the profit
To find Nash:
- One strategy for Buyer is to buy if price is under $v$. (equilibrium is $v$)
- However, there are many equilibrium:
	- Another strategy is buy if price is smaller than $m$ (where $m \leq v$). This leads to equilibrium to be $m$.
Subgame perfect equilibrium is used to show that the alternate buying strategy (buying at $p = m \leq v$) is unnatural.
## 1.6 Nash Equilibrium without Full Information: Bayesian Games
Above games have been with full information (all players know utilities and strategies of all others).
#### Ex: Bayesian First Price Auction
- Suppose instead of players knowing all other valuations, they know the valuations from independent probability distributions that are public knowledge.
- How should player $i$ bid knowing his own valuation $v_i$, and the distribution of valuations of other players?
## 1.7 Cooperative Games
The above games are non-cooperative games (each player acts selfishly, and do not coordinate their moves).
### Strong Nash Equilibrium
In a cooperative game, w assume that a group of players can change their strategies together, and they all benefit.
We say a vector of strategies is a **strong Nash Equilibrium** if no subset of the group of players can all together change their strategies, and improve each of their payoffs. 
- However, very few games have this.
### Fair Division and Costsharing: Transferable Utility Games
**Transferable utility**: dividing some value or sharing a cost between some players. 
## 1.8 Markets and their Algorithmic Issues
Modern capitalist markets use pricing mechanisms that have little intervention. It is therefore natural that economics uses equilibrium theory.
Algorithms can be used for this analysis.
