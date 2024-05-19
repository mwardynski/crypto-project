## Proofs of Computational Integrity

## General idea behind Zero-Knowledge Proof

Zero-Knowledge Proofs (ZKP) are cryptographic methods used to prove knowledge about a piece of data (problem or statement) to verifier, without
revealing the data itself by a prover. ZKP assumes that series of actions can only be performed accurately by a prover if he or she is not guessing.
In other case a prover will be proven wrong by the verifier's test with a high degree of probability.

### Intuitive example of a ZKP

The most common example, used to describe ZKP concept, is the strange cave of Ali Baba. Imagine a cave with one entrance that has two 
paths A and B. Ends of these paths are connected together. However, they are separated with a door, which opens only if someone knows the 
secret. Outside a cave are two people Bob and Alice. Alice is a prover and Bob is a verifier. Alice enters a cave to prove Bob that she knows 
the secret. She chooses randomly path A or B. At the same time Bob is still outside a cave and he does not know what path has been chosen. After
that Bob enters a cave and asks Alice to come outside appearing from path A or B. Alice has 50% chance to fool Bob, because Bob can ask Alice
to come outside through the same path Alice chosen earlier. Therefore this task is repeated many times in order to reduce chances of Alice 
cheating.

- Different introduction into ZK, not so trivial: https://youtu.be/3uSG-Xp5slM?si=fWT2JZlz6-zmSeSO

### Probabilistics in ZKP

1. The prover conceals each vertex of a 3-coloring solution of the graph, making it unobservable to the verifier.
2. The verifier selects an edge of the graph at random, and the prover then reveals the two vertices of this chosen edge. The prover demonstrates that
   these two vertices are of different colors. If they are the same color, it indicates that the prover is being dishonest and does not possess the correct
   solution. If the vertices are of different colors, the verifier gains partial (but not complete) confidence in the prover’s honesty. Notably, the prover
   has a probability of (E-1)/E of cheating successfully, where E represents the number of edges in the graph. This process then proceeds to the next step.
4. The prover once more hides all vertices of the graph and randomly permutes the assignment of the three colors in the solution. Again, the verifier selects
   a random edge to verify its validity (the two vertices must be differently colored). While the prover might still be cheating, the probability that the
   prover successfully deceives in both rounds is ((E-1)/E) * ((E-1)/E) = ((E-1)/E)^2, which is lower than the probability in the previous round.
5. By repeating this process for multiple rounds (n), the likelihood of the prover being able to deceive the verifier can be reduced to a negligible level.
   Probability of Prover Cheating: (((E - 1) / E) ^ n)

## First introduction of ZK proof

Zero-Knowledge Proof was first introduced in a 1985 MIT paper from Shafi Goldwasser and Silvio Micali called “The Knowledge Complexity of Interactive 
Proof-Systems”. In mentioned paper researchers presented three fundamental properties that define Zero-Knowledge Proofs:

- Completness - if statement is true, then a verifier can be convinced by a prover that he or she also posseses knowledge about the truth of statement;

- Soundness - if a statement is false, then no prover can convince a verifier that they possess knowledge about the correct statement;

- Zero-knowledge - if the statement or solution is true, then a verifier learns nothing more than it is true.

In 1991 a valid general zero-knowledge proof system was proposed by another group of researchers: Goldreich, Micali, and Widgerson in order to verify that
a prover knows solution of the 3-coloring graph problem without revealing it to a verifier. If the order of the coloring of the vertices of the graph
was different in each round, a verifier was unable to link the edges revealed between subsequent rounds to construct a solution to this problem.
In other words they showed that a verifier could not identify actual solution to the 3-coloring solution. As a result it can be noticed that proposed protocol 
has three fundamental properties of ZKP. 

“Given a graph G, can you color the nodes with <3 colors such that for every edge {u, v} we have f(u) =/= f(v)?” 

![image](https://github.com/mwardynski/crypto-zk/assets/61807667/e68e8b94-5995-40b2-9556-34974238ecd0)

By creting protocol for these  NPComplete problem they succesfully proven that all statements in NP can be verified through their zero-knowledge protocol.

These revelation leads us to more efficient and more applicable verisions of ZKP called zk-SNARKs (Zero Knowledge Succinct Non-interactive Argument of
Knowledge). This type of ZKP can be verified in a matter of milliseconds with a proof length of a hundreds of bytes. The “non-interactive” aspect refers to
the fact that a prover can send a single message to a verifier, without having many back-and-forth interactions. zk-SNARKs were fist introduced in 2011 year
by: Nir Bitansky, Ran Canetti, Alessandro Chiesa, and Eran Tromer in their paper “From Extractable Collision Resistance to Succinct Non-Interactive Arguments
of Knowledge, and Back Again”. They proved that proposed an extractable collision hash function (ECRH) implies that a modified version of “Di Crecsenzo and Lipmaa’s
protocol is a succinct non-interactive argument for NP. This modified version odf the protocol is called SNARKs.


interactive proof model by Goldwasser, Micali, and Rackoff in 1985 - https://en.wikipedia.org/wiki/Interactive_proof_system
  (also well described in https://fisher.wharton.upenn.edu/wp-content/uploads/2020/09/Thesis_Terrence-Jo.pdf pages: 4-6)

## Adjustments needed for Blockchain

- Privacy (Zero-Knowledge) - respect the privacy of prover's data
- Scalability - log(N) verification, N log(N) proving
- Universality - applicable to general computation
- Transparency - no trusted setup
- State-of-the-art cryptography - e.g. post-quantum secure

STARK = Scalable Transparent Argument of Knowledge
SNARK = Succinct Non-Interactive Argument of Knowledge

### STARK vs SNARK

| STARK                                                    | SNARK                                       |
|----------------------------------------------------------|---------------------------------------------|
| - Transparent - no trusted setup                         | - Non-Interactive - proof is single message |
| - Scalable - verification in log(N), proving in N log(N) | - Succinct - verification in log(N)         |
| - Succinct setup - at most log(N)                        | - Setup - at least in linear time           |

Gotcha:
Non-interactive STARK is transparent SNARK
Transparent SNARK with succinct setup is STARK

#### Reason for having many different implementation

They do the following things differently:

1. Method of Arithmetization 
2. Enforcement of low degreenes
3. Cryptographic assumptions used to get low degreenes

### Characteristics of various concepts

![w:700](_diag/compare.drawio.png)

## Mathematics backstage

### SNARK

https://fisher.wharton.upenn.edu/wp-content/uploads/2020/09/Thesis_Terrence-Jo.pdf chapter 3

### STARK

#### Arithmetization

Show description and examples from: https://youtu.be/9VuZvdxFZQo?si=mVkJdfD0Re_gWND9

Proover transmits two polynomial to confirm his statement $f, g$

Validator evaluates the thow polynomials $f, g$ to verify with 99% certainty the statements of the proover.
To do it validator uses two polynomials: $C(X), D(X)$
$C(X)$ - constraint polynomial of degree $d$, which vanishes 


AIR - algebraic intermediate representation
concept for prooving the program correctness, without the need for verifier to run it

Proof recursion -> prover prooing, that it run a verifier - it can go down many times

Proof systems

#### Enforcing low-degreeness

To the right more homomorphic strucure, where you can add things
To the left more unstructured, so quantum resistance, but hard to add anything 


### Code for STARK

Jupiter notebooks with lectures to it:

https://starkware.co/stark-101/



