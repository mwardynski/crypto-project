![image](https://github.com/mwardynski/crypto-zk/assets/61807667/0329409b-089f-4002-ae90-92a358cee254)

**Course: Cryptography methods in data science**

**Authors: Marcin Wardyński, Bartłomiej Jamiołkowski** 

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

![image](_img/att.24TRmkT9G9Tp4tAzAh6VSn7Q9bU3NqzVJg8EeaQxDQg.jpg)

### Academic example of ZKP
One of the first valid general ZKP systems was proposed by Goldreich, Micali, and Widgerson, specifically to verify that a prover knew the 3-coloring of a graph.
The 3-coloring graph problem is an NP-Complete problem stated as follows:

“Given a graph G, can you color the nodes with <3 colors such that for every edge $\{u, v\}$ we have $f(u) \neq f(v)$?” 

![image](https://github.com/mwardynski/crypto-zk/assets/61807667/e68e8b94-5995-40b2-9556-34974238ecd0)

Proposed ZKP system works in the following way:
1. The prover conceals each vertex of a 3-coloring solution of the graph, making it unobservable to the verifier.
2. The verifier selects an edge of the graph at random, and the prover then reveals the two vertices of this chosen edge. The prover demonstrates that
   these two vertices are of different colors. If they are the same color, it indicates that the prover is being dishonest and does not possess the correct
   solution. If the vertices are of different colors, the verifier gains partial (but not complete) confidence in the prover’s honesty. Notably, the prover
   has a probability of $(E-1)/E$ of cheating successfully, where $E$ represents the number of edges in the graph. This process then proceeds to the next step.
4. The prover once more hides all vertices of the graph and randomly permutes the assignment of the three colors in the solution. Again, the verifier selects
   a random edge to verify its validity (the two vertices must be differently colored). While the prover might still be cheating, the probability that the
   prover successfully deceives in both rounds is $((E-1)/E) * ((E-1)/E) = ((E-1)/E)^2$, which is lower than the probability in the previous round.
5. By repeating this process for multiple rounds (n), the likelihood of the prover being able to deceive the verifier can be reduced to a negligible level.
   Probability of Prover Cheating: $(((E - 1) / E) ^ n)$

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
has three fundamental properties of ZKP. By creting protocol for these  NP-Complete problem they succesfully proven that all statements in NP can be verified 
through their zero-knowledge protocol.

These revelation leads us to more efficient and more applicable verisions of ZKP called zk-SNARKs (Zero Knowledge Succinct Non-interactive Argument of
Knowledge). This type of ZKP can be verified in a matter of milliseconds with a proof length of a hundreds of bytes. The “non-interactive” aspect refers to
the fact that a prover can send a single message to a verifier, without having many back-and-forth interactions. zk-SNARKs were fist introduced in 2011 year
by: Nir Bitansky, Ran Canetti, Alessandro Chiesa, and Eran Tromer in their paper “From Extractable Collision Resistance to Succinct Non-Interactive Arguments
of Knowledge, and Back Again”. They proved that proposed an extractable collision hash function (ECRH) implies that a modified version of “Di Crecsenzo and Lipmaa’s
protocol is a succinct non-interactive argument for NP. This modified version odf the protocol is called SNARKs.

## Adjustments needed for Blockchain

- Privacy (Zero-Knowledge) - respect the privacy of prover's data
- Scalability - log(N) verification, N log(N) proving
- Universality - applicable to general computation
- Transparency - no trusted setup
- State-of-the-art cryptography - e.g. post-quantum secure

STARK = Scalable Transparent Argument of Knowledge  
SNARK = Succinct Non-Interactive Argument of Knowledge

![image](_img/UTKB4bILBsDzDMEGU9feYg.png)


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



## Mathematics behind SNARK

### Arithmetization

Arithmetization is the process of transforming a computational integrity problem into a problem about polynomials. In SNARKs, this involves encoding the original problem as a set of polynomial
equations that can be checked for correctness.

1. The original problem is reduced to QSPs (Quadratic Span Programs) proposed by Gennaro, Gentry, Parno, and Raykova.

2. A QSP consists of multiple polynomials **v0...vi, w0...wj** over a field **F** and a target polynomial **t**.

3. The QSP is accepted for an input and a witness if and only if **t** divides **va ∗ wb**, where **va** and **wb** is constructed from the witness and the original polynomials **v0...vi, w0...wj**.
Thus the prover shows that **t ∗ k = va ∗ wb** for some other polynomial **k**. 

This reduction effectively turns the problem of verifying computational integrity into a problem of verifying polynomial relationships.

### Integrity and succinctness

Because of the complexity of large polynomials and large runtime of multiplying and dividing polynomials, this QSP is hard to completely verify in practice. Therefore verifier chooses a
secret point **s** such that **t(s) * k(s) = va(s) * wb(s)**.

Currently, the most common constructions of SNARKs involve a CRS (Common Reference String) and a set-up of initial parameters. Firstly, we choose a group and agenerator **g**, and an encryption
scheme **E** where **E(x) = g^x**. Then, the verifier secretly chooses **s** as well as another value **z** and publicly posts as part of the CRS the following:

E(s^0), E(s^1), ... , E(s^d)

E(zs^0), E(zs^1), ... , E(zs^d)

where **d** is the maximum degree of all polynomials.

Once these values are calculated and posted, the verifier must discard the secret point **s** for security reasons, so that the prover cannot obtain it to falsely create proofs. The prover must then use
these published values above to prove that he can compute a polynomial function **f**. We can see that any prover can compute **m = E(f(s))** for any function **f** without knowing the verifier’s secret
value **s**

### Example

Consider a function **f(x) = x^2 + x**. The prover computes the following:

E(f(s)) = E(s^2 + s) = g^(s^2 + s) = g^(s^2) * g^s = E(s^2) * E(s)

Each of **E(s^2)** and **E(s)** can be taken from the publicly published CRS. By the same token, the prover can also compute **n = E(z * f(s))**, and sends both **m** and **n** to the verifier.




The reason that the verifier needs **n** in addition to **m** is that earlier the verifier had discarded **s**, so there is no way to check that the prover correctly solved the polynomial **f** at **s**.
Thus a way around this is once the verifier receives the values **m** and **n**, the verifier must check that **m** and **n** match through an pairing function �, which is chosen with the group
chosen in the CRS setup phase, such that the following holds for all inputs values **x** and **y**:


Pairing function **p** is a computable bijection such that **p: NXN -> N**. We can see immediately that this pairing function **p** becomes useful, as we can plug in the pairs **n**, **g** and
**m**, **g^z** into the pairing function, and if the results match, then we know that the prover solved the polynomial correctly at **s**, as shown by the following equations:

#### Zero Knowledge

To add zero-knowledge, we modify **m** and **n** with the prover choosing a random value � to “shift” the value of **f(s)** before encryption. So prover computes new:

m = E(� + f(s))

n = E(z * (� + f(s)))

and sends it to a verifier.

E(� + f(s)) = g^(� + f(s)) = g^� + g^f(s) = E(f(s)) * E(�)

We can see from above that the prover can still compute **m** from the public parameters in the CRS, and by the same token, the prover can also compute **n**. Once the verifier receives **m**
and **n**, the values are inputted into the pairing function **p** in a similar fashion to the example above:

p(m, g^z) = p(g^(� + f(s), g^z) = p(g, g)^(z * (� + f(s)))

p(n, g^z) = p(g^(z * (� + f(s)), g) = p(g, g)^(z * (� + f(s)))

From the equations above, we see that verification process still functions properly, and the verifier’s computation is still limited to the pairing function.

As mentioned previously, we want to protect
knowledge of �(�(�)) and �(�) from leaking to the verifier. It is clear that �(�(�)) is not
leaked, as the prover no longer sends this value to the verifier for validation. For �(�)  the only
useful information that a malicious verifier can extract from the values �` and �` is � + �(�).
Since � is a random value only known to the prover13, it is now apparent that the malicious
verifier can no longer deduce the value of �(�), and thus we have shown the new modified
protocol has zero-knowledge

## Mathematics behind STARK

### Arithmetization

Arithmetization - use of *low degree polynomials* to argue about computation
- Goes back to 1930's and modified for interactive proof systems in late 1980's

Polynomial of *degree d*: $P(X) = \sum_{i \leq d} a_iX^i$  

Function $f: S \to \mathbb{F}$ : "lookup table" having a value from $\mathbb{F}$ for each each element from the domain $S$.  
Function is of *degree d*, if it evaluates a *degree d* polynomial: $\forall x_0 \in S, f(x) = P(x)$

Fact: Two distinct polynomials of *degree d* interact at at most *d* points, so two distinct functions of *degree d* can interact on very low number of points if only the domain is big enough.

The fallowing arithmetization will be presented using AIR (Algebraic Intermediate Representation) approach, but there are other options, like R1CS (Rank-1 Constraint System).

### Integrity and succinctness

Prover represents the trace of the program, or any other data, as polynomial and then uses *error correcting code* technique to add redundancy and create function of the same degree, but evaluated on much bigger domain - this function is called $f$.  
Prover needs to create yet another function, $g$, which is of a higher degree than $f$, and evaluate it on the same domain as $f$.  
Then prover commits to these two such functions, $f, g$, and allows the verifier to conduct the verification, which takes up to $O(log(N))$

Veryfier runs the following test:
1. selects random $x_0$ belonging to the domain extended using error correcting code
2. queries $f(x_0)$ and assigns the result to $\alpha$
3. queries $g(x_0)$ and assigns the result to $\beta$
4. accepts the prover's claim only if $C(\alpha) = \beta D(x_0)$

$C(X)$ - the constraint polynomial - vanishes when specified constraints are fulfilled  
$D(X)$ - the domain polynomial - vanishes on domain of interest 

### Example

Prover has a list of $10^6$ integers, all of them from range $\{1, ..., 10\}$.  
Verifier wants to check the prover's claim, without the need of iterating over all of the list entries.

Prover commits to the two following functions:
- $f$, being of degree $10^6$ and evaluated over $10^9$ points
- $g$, degree $10^7-10^6$ and also evaluated over $10^9$ points

Verifier sets polynomials $C(X), D(X)$ in the following way:
- $C(X) = (X-1)(X-2)...(X-10)$ - vanishes on any value from $\{1, ..., 10\}$
- $D(X) = (X-1)(X-2)...(X-10^6)$ - same like above, but for the claim domain, which is $\{1, ..., 10^6\}$

#### Completeness

The prover is honest and says the truth.

Let $P(X)$ be the interpolant of $f$.  
Then $C(P(X))$ vanishes on $x_0 \in \{1, ..., 10^6\}$.

*Corollary:* $Q(X) = C(P(X))$ vanishes on domain $\{1, ..., 10^6\}$ only if $\exists Q'(X), deg(Q') = deg(Q) - deg(D) \leq 10^7-10^6$, such that $Q'(X)D(X) = Q(X)$.

If prover is honest, then $g$ is the evaluation of $Q'(X)$, because $C(\alpha) = C(P(x_0)) = Q(x_0) = Q'(x_0)D(x_0) = \beta D(x_0)$. 

#### Soundness

The prover cheats, so $\exist x_0, f(x_0) \notin \{1, ..., 10\}$.

Let $P(X)$ be the interpolant of $f$  
$C(P(X))$ doesn't vanish on all $x_0 \in \{1, ..., 10^6\}$  
$\forall Q'(X), Q'(X)D(X)=0$  on all $x_0 \in \{1, ..., 10^6\}$, because $D(x_0)=0$  
So for any $Q'(X)$ of degree $10^7-10^6$, the polynomial $Q'(X)D(X)$ is distinct from $C(P(X))$ and of degree $10^7$  
The evaluations of $C(P(X))$ and $Q'(X)D(X)$ disagree on $10^9-10^7$ points out of $10^9$ points  

For random $x_0 \in \{1, ..., 10^6\}$, test fails with probability of 99%. The probability can be higher, if $f, g$ are selected accordingly.

#### Zero Knowledge

Prover appends to the initial list of integeres a few (e.g. 10) random entries, so the corresponding polynomial is of $deg = 10^6+10$ and any values after first $10^6$ entries are fully random.

Verifier selects random $x_0 \in \{10^6+1, ..., 10^9\}$

#### Enforcing low-degreeness

To the right more homomorphic strucure, where you can add things
To the left more unstructured, so quantum resistance, but hard to add anything 


### Code for STARK

Jupiter notebooks with lectures to it:

https://starkware.co/stark-101/


Proof recursion -> prover prooing, that it run a verifier - it can go down many times

Proof systems


