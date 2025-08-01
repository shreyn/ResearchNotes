## 2.1 Pseudorandom Generators (PRGs)
In symmetric cryptography, secure systems need lots of randomness. However, generating long truly-random strings is impractical. Instead, we use a short random seed, and "stretch" it into a longer string that looks random to any adversary.

Let $G: \{0,1\}^n \rightarrow \{0,1\})^{l(n)}$ be a deterministic function, where:
- $n$ is seed length (input size)
- $l(n) > n$ is the output length (expansion)
- $G$ is efficiently computable (polynomial time)
**Def:** A function $G$ is a PRG if:
1. Expansion: $l(n) > n$
2. Pseudorandomness: No probabilistic polynomial time (PPT) adversary $A$ can distinguish the output of $G$ from a truly random string of length $l(n)$ with more than negligible advantage.
	- Basically, you can't tell that it was created from a smaller string
### Next-Bit Test (Yao's Characterization)
Suppose someone gives you a long string $y \in \{0,1\}^m$. You know the string was either:
- chosen uniformly at random from $\{0,1\}^m$, or
- computed as $G(s)$ for a random seed $s \in \{0,1\}^n$, $n \leq m$
Can you look at the first $i$ bits, and guess the next bit $y_{i+1}$ with better than 50% accuracy?
- If you can, then $y$ is not truly random, you are detecting structure
**Yao's Theorem**: A generator $G$ is pseudorandom iff no PPT algo can predict the next bit of $G(s)$ with some advantage.
- Why is this powerful? This is a **local test** for pseudorandomness
	- Don't have to distinguish the entire string form uniform
$$\text{"can't distinguish output from uniform"} \Leftrightarrow \text{"can't predict the next bit given prior bits"}$$
### One-Way Functions (OWFs)
Can we really build a secure PRG?
Relies on the bedrock of cryptography: **Some functions are easy to compute but hard to invert.**
**Def:** A function $f:\{0,1\}^n \rightarrow \{0,1\}^*$ is one-way if:
- $f$ is easy to compute (poly-time)
- But hard to invert: no adversary can find a preimage $x$ from $y = f(x)$ easily.
Key Theorem:
- If one-way functions exist, then pseudorandom generators exist
$$\text{OWFs} \Leftrightarrow \text{PRGs}$$
Proof of how to construction a PRG from an OWF..
## 2.2 Stream Ciphers
Main idea: use a PRG to generate a keystream, then XOR it with the plaintext.
Let:
- $G: \{0,1\}^n \rightarrow \{0,1\}^m$ be a secure PRG
- $k \in \{0,1\}^n$ is the secret key
- $m\in \{0,1\}^m$ is the plaintext message
- $c \in \{0,1\}^m$ is the ciphertext
Then:
$$Enc_k(m) = G(k) \oplus m = c$$
$$Dec_k = G(k) \oplus c = m$$
- just like the one-time pad, but with a pseudorandom keystream instead of a truly random one
Correctness:
$$Dec_k(c) = G(k) \oplus c = G(k) \oplus (G(k)\oplus m ) = m$$
- since $x \oplus x = 0$, and $0 \oplus m = m$
### IND-CPA security
"indistinguishability under Chosen Plaintext Attack"
- since the PRG is secure, the ciphertext $c = G(k) \oplus m$ is also indistinguishable from random
**Keystream Reuse is BAD!**
- Suppose we encrypt two messages $m_1, m_2$ with the same key $k$.
$$c_1 = G(k) \oplus m_1, \hspace{1em} c_2 = G(k) \oplus m_2$$
- Then an attacker who intercepts the ciphertexts can compute:
$$c_1 \oplus c_2 = (G(k) \oplus m_1) \oplus (G(k)\oplus m_2) = m_1 \oplus m_2$$
- The XOR of two plaintexts leaks a bunch of info
- Called a **two-time pad attack**
- 