## 4.1 Introduction
We want to analyze these kinds of problems:
- An algorithm chooses an action
- The environment makes its "move" (some metric)
- The algorithm calculates the loss for its action
- Repeats
We want to develop adaptive algorithms.
One method is through **regret analysis**:
- We want to develop an algo that doesn't have an alternative algo with a better loss
- Regret is the difference between our algo and the better algo.
- **External Regret**: compares performance to the best single action in retrospect.
	- implies the alternative action performs the same action in all time steps
Another method is through **swap/internal regret**.
- "every time you did X, you should have done Y instead"
**Partial information model / Multiarmed Bandit (MAB):** 
- The algo can only observe the loss of the action actually chosen, not of the actions not chosen.
