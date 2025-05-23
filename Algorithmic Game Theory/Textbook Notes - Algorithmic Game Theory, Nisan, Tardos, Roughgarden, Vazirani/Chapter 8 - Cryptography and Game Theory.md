## 8.1 Cryptographic Notions and Settings
**Multi-Party Computation (MPC)**:
- There are $n \geq 2$ parties, where each party $P_i$ holds input $t_i$, and they want to compute a function together $s = f(t_1, \dots, t_n)$, using their inputs. 
	- The goal is that each party will learn the output of the function ($s$), but each party will not learn any additional info about the input of any other parties.
- 