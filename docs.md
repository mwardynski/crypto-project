## Proofs of Computational Integrity

## General idea behind Zero-Knowledge proof

Zero-Knowledge Proofs (ZKP) are cryptographic methods used to prove knowledge about a piece of data, without revealing the data itself.
The most common example, used to describe ZKP concept, is the strange cave of Ali Baba. Imagine a cave with one entrance that has two 
paths A and B. Ends of these paths are connected together. However, they are separated with a door, which opens only if someone knows the 
secret. Outside a cave are two people Bob and Alice. Alice is a prover and Bob is a verifier. Alice enters a cave to prove Bob that she knows 
the secret. She chooses randomly path A or B. At the same time Bob is still outside a cave and he does not know what path has been chosen. After
that Bob enters a cave and asks Alice to come outside appearing from path A or B. Alice has 50% chance to fool Bob, because Bob can 
ask Alice to come outside using the same path what has been chosen earlier by Alice. Therefore this task is repeated many times in order to
reduce chances of alice cheatning.

- Example of a cave (maybe where's Waldo?)
- Different introduction into ZK, not so trivial: https://youtu.be/3uSG-Xp5slM?si=fWT2JZlz6-zmSeSO

## First introduction of ZK proof

- interactive proof model by Goldwasser, Micali, and Rackoff in 1985 - https://en.wikipedia.org/wiki/Interactive_proof_system
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



