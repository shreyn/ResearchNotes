## Games, Old and New
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