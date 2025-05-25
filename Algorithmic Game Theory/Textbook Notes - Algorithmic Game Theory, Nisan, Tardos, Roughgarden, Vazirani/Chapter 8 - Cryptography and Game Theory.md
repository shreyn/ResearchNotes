## 8.1 Cryptographic Notions and Settings
**Multi-Party Computation (MPC)**:
- There are $n \geq 2$ parties, where each party $P_i$ holds input $t_i$, and they want to compute a function together $s = f(t_1, \dots, t_n)$, using their inputs. 
	- The goal is that each party will learn the output of the function ($s$), but each party will not learn any additional info about the input of any other parties.
- One important aspect of MPC is to reach the goal of computing the function value, but while assuming that some parties may deviate from protocol.
	- Honest party: follows protocol without deviation. 
	- Faulty party: doesn't follow protocol
		- Honest-but-curious (semihonest): follow protocol, but try to learn as much as possible about the inputs of other parties
		- Malicious - trying to learn about inputs, and influence the computed output value.
	- We assume there is an adversary who controls all the faulty parties (they work together)
		- We say that they can "corrupt" up to some threshold $k$ of the $n$ parties
**Security of MPC**:
- Assume there is a trusted party who privately receives the inputs of all parties, calculates the output $s$, and transmits this value to the parties.
	- This computes the correct output, and the parties don't learn any info about the others' inputs
	- Called the **ideal model**
- The security of MPC says that a protocol is secure if: the honest parties compute the same correct outputs as in the ideal model, and the protocol does not expose more information than the interaction with the trusted party (in the ideal model)
Remember from this: [[Chapter 1 - Basic Solutions and Computational Issues#Correlated Equilibrium]]
- In CE, a trusted third party (mediator) samples a joint strategy from a known public distribution
	- Tells each player privately what move to make
	- Equilibrium holds if no player wants to deviate from what they are told
	- 