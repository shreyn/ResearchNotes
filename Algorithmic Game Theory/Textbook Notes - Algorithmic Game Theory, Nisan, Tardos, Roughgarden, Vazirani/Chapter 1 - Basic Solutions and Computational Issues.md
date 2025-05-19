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
#### Vickrey Auction: Designing Games with Dominant Strategy Solutions
**This is pretty interesting**
