1. [Introduction](#introduction)
2. [Math](#math)
   - [Abstract Algebra](#abstract-algebra)
   - [Groups](#groups)
   - [Rings](#rings)
   - [Fields](#fields)
   - [Elliptic Curves](#elliptic-curves)
   - [Pairings](#pairings)
   - [Polynomials](#polynomials)
   - [Lagrange Polynomial (Interpolation)](#lagrange-polynomial-interpolation)
   - [Schwartz-Zippel Lemma](#schwartz-Zippel-lemma)

# Introduction

There exist a lot of lists/blogs on how to study zk. But most don’t explain the connections between individual topics and why each is important in the grand scheme. This series aims to overcome these shortcomings. We will cover the main idea of every topic and share a link with good resource to dive deeper.

# Math

ZK is often described as moon math; this section tries to bring it to earth. It describes the essential mathematical preliminaries, which themself aren’t that complicated. However, there is no reason to reinvent the wheel. Thus the primary purpose of this guide is to aggregate all the necessary topics and list resources with quality materials to learn from.

### Abstract algebra

Abstract algebra is the study of algebraic structures such as groups, rings, and fields, which are sets with operations obeying specific axioms. Advanced theories include Galois theory, which connects field extensions to group theory, and module theory, a generalization of vector spaces over rings.

Why do we need them? 

- In zero-knowledge proofs, abstract algebra is employed through cryptographic protocols using algebraic structures like finite fields and elliptic curve groups. Abstract algebra is favored in cryptographic systems due to the discrete nature and finite size of algebraic structures like finite fields and groups. Finiteness ensures efficient computation, well-defined operations and prevents information leakage by reducing susceptibility to approximation attacks. It also simplifies complexity analysis and provides stronger security guarantees due to the difficulty of inverse problems (e.g., discrete logarithm, factoring) and bounded search space, which enables controlled security levels and resistance to brute-force attacks. Unlike continuous and unbounded real or complex numbers, finite structures offer manageable computational overhead and improved security.

Resources:

- https://explained-from-first-principles.com/number-theory/ (Excellent resource covering all the essential topics from the Number Theory - groups, fields, generators, Elliptic curves)

### Groups
Group theory is the subset of abstract algebra. Even though we can see “group” in the name of this theoretical math subject, the group is not the most basic object. The following part describes the most trivial objects from the group theory. Every object inherits properties of all the previous ones.

$$
M\ - set\ of\ numbers\ (\mathbb{Z},\mathbb{R},[0,1,2],...)
$$

$$
\circ \ - \  binary \ operation \ (+,\times,...)
$$


$$ $$

$$
Magma:
$$

$$
\forall x,y \in M \ \Rightarrow \ (x\circ y) \in M\
$$

$$
(result \ of \ operation \ \circ \ is \ an \ element \ from \ M)
$$


$$ $$

$$
Semigroup:
$$

```math
\forall a,b,c \in M \ 
\Rightarrow \ a\circ (b\circ c) = (a\circ b)\circ c
```

$$
(\circ \ - \ assicoiative \  operation)
$$


$$ $$

$$
Monoid:
$$

```math
\forall a \in M; \ &#8707 \ e \in M \ \Rightarrow \ a\circ e = e\circ a = a
```

$$
(e \ - \ identity \ element)
$$

$$ $$

$$
Group:
$$

```math
\forall a \in M; \ &#8707 \ a^{-1}  \in M; \ &#8707 \ e \in M
```

```math
\Rightarrow a \circ a^{-1} = a^{-1} \circ a = e
```

$$
(every \ element \ has\ an \ inverse \ element)
$$

$$ $$

$$
Abelian\ group:
$$

```math
\forall a,b, \in M \Rightarrow a \circ b = b \circ a
```

$$
(\circ \ - \ commutative\ operation)
$$

We also name the object based on operation it is using. For example group $G(M, *)$ is called *multiplicative* group and $G(M, +)$ is called *additive.* 

### Rings
The ring by simple definition is a group with both *multiplicative* and ***additive*** operations.

$$
Ring \ R:
$$

$$
set\ of\ 2\ operations
$$

```math
+ \ : R + R \rightarrow R
```

```math
\times : R \times R \rightarrow R 
```

$$
\bullet \ R \ is \ Abelian \ group \ for \ +
$$

$$
\bullet \ \times \ is \ associative \\ 
$$

$$
\bullet \ + and \ \times \ are \ distributive:
$$

```math
\forall a,b,c \in R
```

```math
a \times (b + c) = (a\times b) + (a\times c) \quad and \quad  
(a + b)\times c = (a\times c) + (b\times c)
```

### Fields

The Field is the Ring with a possibility of division (the multiplicative group of ring R is commutative)

```math
Multiplicative \ group \ of \ the \ field \ T = T^*
```

```math
T^* = (T \  \backslash \ \{0\}, \ \times)
```

Why fields?

- For constructing SNARKs, we need to work over finite fields. Why? Using integers would blow up the proof size, which then would be too large to comply with SNARK requirements - by [Thaler.](https://youtu.be/4018OYyoAf8?t=7417)
- [Vitalik](https://vitalik.ca/general/2017/01/14/exploring_ecp.html) provides another motivation for using fields in Elliptic curves pairings

Resources:

- https://www.youtube.com/playlist?list=PLcPzhUaCxlCiddOGuYdDbFlZhH8nwtR8D (Video course about finite fields)
- https://explained-from-first-principles.com/number-theory/#finite-fields (Finite fields chapter in already mentioned resource)

### Elliptic Curves

Instead of using just an algebra object (Field, Ring,..) where a set of numbers must be explicitly specified, we can use mathematical functions. The set of numbers (points) that satisfy the function defines the set of numbers of this object. The cubic function used for this purpose is called the Elliptic Curve, defined (usually, but not by rule) over a field $F$.

An elliptic curve is a special type of cubic curve (with order 3). The basic **formula** of an Elliptic curve has a following form:

```math
y^2=x^3+ax+b
```

Where **a** and **b** are the parameters of the curve. Another parameter is **p**, that specifies the field $F_p$ over which the curve is defined. (*!some choices of parameter have their known weakness and should by avoided*). 

EC can be used in very similar ways to groups. However to accomplish this behaviour, the curve has to be **smooth** - be differentiable infinitely many times. To achieve this we define **discriminant**: 

```math
-4^3 - 27b^2 \ne 0 
```

If the equation is True, the curve is ready to perform group-like operation such as adding.

Points at EC can be **added** together and produce a third point. We use the word “add” because the operation combines two points to create a third one; it is associative and commutative. However, the operation itself on paper looks quite different from the “adding” we know. Using [geometry](https://curves.xargs.org/) is the easiest way to describe how the operation works on EC. Algebraic representation of “adding” operation on Elliptic curves:

```math
A=(x_1, y_1)
```

```math
B=(x_2,y_2)
```

```math
C =A+B\\
\lambda =
    \begin{cases}
      \frac {y_1-y_2}{x_1-x_2} & for \ x_1 \ne x_2\\
      \\
      \frac {3x_1^2+a}{2y_1} & for \ x_1 = x_2
    \end{cases}  \\     
```

$$
Then
$$

```math
x_3=\lambda^2-x_1-x_2 \\
y_3=\lambda(x_3-x_1)+y_1\\
C=(x_3,y_3)
```

Elliptic curves also have a $O$ (zero) so-called “**point at infinity**” as a identity element. We can use geometry representation again and translate “**point at infinity**” as a point of intersection of two parallel lines. There is no intersection, exactly. Nevertheless, the importance of the point at infinity cannot be doubted as the importance of zero in a commutative group. The zero is essential for finding the generator of the Elliptic curve - repeatedly adding a point to itself until generate all the items of the group including $O$ point.

Why do we need EC? 

- Elliptic curves provide the same amount of security but use fewer bits. In computer systems, EC also requires less hardware and less power consumption. Thus they work very well for asymmetrical cryptography.
- Bitcoin and Ethereum are using [secp256k1](https://www.secg.org/sec2-v2.pdf) curve for their private-public key cryptography algorithm - ECDSA aka Elliptic Curves Digital Signature Algorithm.
- Elliptic curves are fundamental in zkSNARKS, however the curve mentioned above is not friendly for such systems. S in STARKs stands fro *scalable*, on the other hand S in SNARKS stands for *succinct*, which represent some kind of size limitation to keep the SNARK system effective. Many numbers in such systems are limited in 254 bits, which can cause 256 bits curve to overflow. Thats why we can see modified ECDSA algorithms with tuned curves specifically for ZK algorithms.

Resources:

- https://mathworld.wolfram.com/EllipticCurve.html (Definition of Elliptic Curve by Wolfram)
- https://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/ (Well-written EC basics. Adding points, generators, DL problem,..)
- https://curves.xargs.org/ (More playful and visual resource)

### Pairings

Elliptic curve pairing (or bilinear maps) work as a form of “encrypted multiplication” and it keeps the secret private. The main functionality of pairing is to map values to the special object that allows us to perform certain arithmetic operations with it, but it keeps the original values obscure.

Why? 

- In ZkSNARKs a prover needs to prove a specific knowledge he/she has for solving a problem. It can be done by sharing the secret, but in such a case the property of “Zero Knowledge” will be lost. The correct way is to keep the secret and use a pairing on an elliptic curve, which will hide the secret but convince the verifier.
- Pairings underlie many of the cryptographic objects in a modern zero-knowledge cryptography: BLS digital signatures, KZG polynomial commitments, and zkSNARKs.

Resources: 

- https://medium.com/@VitalikButerin/exploring-elliptic-curve-pairings-c73c1864e627 (Vitalik blog post)
- https://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.pdf (chapter #15 of Thaler book)
- https://www.youtube.com/watch?v=WyT5KkKBJUw (ZKP MOOC lecture 6)

### Polynomials

Polynomial ****is an expression consisting of **indeterminates** (also called variables) and **coefficients**. The formal definition of a polynomial is following:

```math
a_0 + a_1X^1 + ... + a_{n-1}X^{n-1} + a_nX^n
```

```math
coeficients: (a_0,...,a_n)
```

```math
indeterminates: (X^1,...X^n)
```

Polynomials are an essential part of the mathematical and algebraic “language”. It is used in almost every field of mathematics to express formulas and equations formally. One term of the polynomial is called monomial. The degree of a monomial (often called term) is the sum of all its exponents. For Example: $x^3y^2$ is the monomial and its degree is $3+2=5$.
The degree of a polynomial is the largest degree out of all the degrees of monomials. For example, a degree of the polynomial $4x^2y + 2y^2 + x + 1$ is $3$.
The degree defines different behavior and properties of the polynomial. Some of them also have their own names:

- 0 degree polynomials are called constant
- $1^{st}$ degree polynomials are called linear
- $2^{nd}$ degree polynomials are called quadratic
- $3^{th}$ degree polynomials are called cubic
- Higher-degree polynomial do not have special names

You can perform any arithmetic operations on polynomials where the result is another polynomial. More advanced operations, such as factoring and working with remainders of polynomials, can be seen a lot in the world of modern cryptography.

Why polynomials?

- Polynomials are the heart of (not only) cryptography. We can encounter them in simple cryptographic protocols as well as in more complex ones such as PLONKs. Knowledge of polynomials and how to work with them is essential for truly understanding the world of ZK.
- A more specific motivation is a polynomial commitment scheme. It is the main ingredient of constructing general-purpose zero-knowledge proof called PLONK. The commitment is an object representing a polynomial and allowing it to be verified evaluations of that polynomial without needing to actually contain all of the data in the polynomial [[cited](https://vitalik.ca/general/2019/09/22/plonk.html) Vitalik’s post].

### Lagrange Polynomial (Interpolation)

Lagrange interpolation is used for a problem, where we have a set of coordinates *(x, y)* and we want to find a polynomial that passes through all of those points. The more data points that are used in the interpolation, the higher the degree of the resulting polynomial, and therefore the greater oscillation it will exhibit between the data points. Therefore, a high-degree interpolation may be a poor predictor of the function between points, although the accuracy at the data points will be "perfect.” [cited [wolfram](https://mathworld.wolfram.com/LagrangeInterpolatingPolynomial.html)]

WHY?:

- It is present, for example, in QAP for transforming R1CS to polynomials.
- PLONKs (Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge)

Resources:

- https://mathworld.wolfram.com/LagrangeInterpolatingPolynomial.html (Formulas of interpolation by Wolfram)
- https://vitalik.ca/general/2016/12/10/qap.html (QAP by vitalik

### Schwartz-Zippel Lemma

The lemma helps to detect when a [polynomial](https://www.notion.so/zk-zero-to-zk-hero-595db3097f04466d9ee88e891bce673a) is identically zero or identical to another polynomial. The formal statement of the Lemma:

```math
F - Field
```

```math
f(x_1,x_2,...,x_n) - polynomial \ of \ degree\  d, \ f \ is \ not \ a \ zero \ polyn.
```

```math
S - finite \ subset \ of \ F 
```

```math
(r_1,r_2,...,r_n) -  randomly \ chosen \ from \ S
```

```math
Lemma: \ The \ probability \ that \ f(r_1,r_2,...,r_n) = 0 \ is \le \frac {d} {|S|}
```

If there are two polynomials of degree 1, these are identical or intersect at most once. If the degree is 2, these polynomials are identical or intersect at most twice. The Schwartz-Zippel lemma is formally defining this pattern for any degree $d$. As we can see from the formula, it does not compare polynomials, but it is evaluating the probability that a polynomial evaluates to zero.

Why?

- The lemma is an important part of the commitment schemes. If we choose a random sample point $r$ from the finite field $F_p^{\le d}$, the probability of non-zero polynomial evaluated to zero at the point $r$ is  $\le \frac {d} {p}$. Because the polynomial has the degree at most $d$ it has at most $d$ roots in the finite field. What does it mean - the polynomial at the point $r$ is equal to zero only if the point $r$ hits one of the roots of the polynomial. Because the probability $\frac {a}{p}$ of hitting the root is negligible; when it happens that the evaluation of $f(r) = 0$, then $f$ is identically to zero with high probability. This is called a zero test for a committed polynomial, and we can see it, for example, in PLONK.

Sources:

- https://www.cs.ubc.ca/~nickhar/W12/Lecture9Notes.pdf (Section 1 - Polynomial Identity Testing, University of British Colombia)
- [https://en.wikipedia.org/wiki/Schwartz–Zippel_lemma](https://en.wikipedia.org/wiki/Schwartz%E2%80%93Zippel_lemma) (Wikipedia - definition of the lemma)



_Complexity chapter - coming soon_
