---
title: "Introduction to quantum cryptography"
date: 2026-09-09 00:00:00 +0700
category: [quantum]
tags: [quantum, theory]
---

# 0. Assumptions
- All symbols used to denote vectors in this blog (unless otherwise specified) represent column vectors.

### Mathematical notations
- $$\overline{x}$$ ; $$x^*$$ $$(x \in \mathbb{C})$$: the conjugate of $$x$$
- $$[n]$$: the integer set $$\{1, 2, \dots, n\}$$


# 1. Basic notations

## 1.1. Quantum state

### Definition
A **qubit** (or quantum bit) is the basic unit of information in quantum computing.

Unlike a classical bit, which is always in a definite state $$0$$ or $$1$$, a qubit can exist in a **superposition** of both states simultaneously. This is possible as a qubit is physically realized as a two-state (or two-level) quantum-mechanical system.

This is a special case of a more general fact about quantum systems:

> **Postulate:** A *quantum state* is represented by a unit vector in a complex Hilbert space $\mathcal{H}$.
{: .prompt-definition}

Precisely speaking, a qubit is a quantum state with two levels in Hilbert space $\mathcal{H} \cong \mathbb{C}^2$.


## 1.2. Quantum notation - basis - measurement

Important recap on complex number: $$\overline{a}\cdot\overline{b} = \overline{ab}, \quad a, b \in \mathbb{C}$$.

### 1.2.1. Dirac notation
A quantum state vector can be expressed in the following form:

**Ket: column vector**

$$|\psi\rangle = \begin{pmatrix} a_1 \\ a_2 \\ \vdots \\ a_m \end{pmatrix} \qquad |\varphi\rangle = \begin{pmatrix} b_1 \\ b_2 \\ \vdots \\ b_m \end{pmatrix}$$

**Bra: row vector**

$$\langle\psi| = \begin{pmatrix} \overline{a_1} & \overline{a_2} & \dots & \overline{a_m} \end{pmatrix}$$

$$\langle\varphi| = \begin{pmatrix} \overline{b_1} & \overline{b_2} & \dots & \overline{b_m} \end{pmatrix}$$

**Bra-ket: inner product**

$$\langle\varphi|\psi\rangle = \begin{pmatrix} \overline{b_1} & \overline{b_2} & \dots & \overline{b_m} \end{pmatrix} \begin{pmatrix} a_1 \\ a_2 \\ \vdots \\ a_m \end{pmatrix} = \sum_{i \in [m]} \overline{b_i} a_i \in \mathbb{C}$$

**Ket-bra: outer product**

$$|\varphi\rangle\langle\psi| = \begin{pmatrix} b_1 \\ b_2 \\ \vdots \\ b_m \end{pmatrix} \begin{pmatrix} \overline{a_1} & \overline{a_2} & \dots & \overline{a_m} \end{pmatrix} = \begin{pmatrix} b_1\overline{a_1} & b_1\overline{a_2} & \dots & b_1\overline{a_m} \\ b_2\overline{a_1} & b_2\overline{a_2} & \dots & b_2\overline{a_m} \\ \vdots & \vdots & \ddots & \vdots \\ b_m\overline{a_1} & b_m\overline{a_2} & \dots & b_m\overline{a_m} \end{pmatrix} \in \mathbb{C}^{m \times m}$$

**Tensor product**

$$A = \begin{pmatrix} a_{11} & \dots & a_{1n} \\ \vdots & \ddots & \vdots \\ a_{m1} & \dots & a_{mn} \end{pmatrix}, \qquad B = \begin{pmatrix} b_{11} & \dots & b_{1l} \\ \vdots & \ddots & \vdots \\ b_{k1} & \dots & b_{kl} \end{pmatrix}$$

$$A \otimes B = \begin{pmatrix} a_{11}B & \dots & a_{1n}B \\ \vdots & \ddots & \vdots \\ a_{m1}B & \dots & a_{mn}B \end{pmatrix} = \begin{pmatrix} a_{11}b_{11} & \dots & a_{1n}b_{1l} \\ \vdots & \ddots & \vdots \\ a_{m1}b_{k1} & \dots & a_{mn}b_{kl} \end{pmatrix} \in \mathbb{C}^{mk \times nl}$$

Tensor product of quantum states is defined as follows:

$$|\psi_{AB}\rangle = |\psi_A\psi_B\rangle \overset{\text{def}}{=} |\psi_A\rangle |\psi_B\rangle = |\psi_A\rangle \otimes |\psi_B\rangle$$

> *For example:*
>
> $$|00\rangle = |0\rangle \otimes |0\rangle = \begin{pmatrix} 1 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \\ 0 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \\ 0 \\ 0 \end{pmatrix}, \qquad |01\rangle = |0\rangle \otimes |1\rangle = \begin{pmatrix} 1 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \\ 0 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix},$$
> 
> $$|10\rangle = |1\rangle \otimes |0\rangle = \begin{pmatrix} 0 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \\ 1 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 1 \\ 0 \end{pmatrix}, \qquad |11\rangle = |1\rangle \otimes |1\rangle = \begin{pmatrix} 0 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \\ 1 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 0 \\ 1 \end{pmatrix}.$$

> **Some properties between these operations:**
1. In general,
>
   $$
   \langle a|b \rangle = \langle b|a \rangle^*
   $$
>
2. With 
$|a\rangle, |c\rangle \in \mathbb{C}^m$ and $|b\rangle, |d\rangle \in \mathbb{C}^n$:
>
   $$ \text{InnerProd}(\langle a| \otimes \langle b|, |c\rangle \otimes |d\rangle) = \text{InnerProd}(\langle a|c\rangle, \langle b|d\rangle)$$
>
   or in short:
> 
   $$ \boxed{\langle ab|cd \rangle = \langle a|c\rangle \langle b|d\rangle = \langle b|d\rangle \langle a|c\rangle = \langle ba|dc \rangle} $$
3. With arbitrary
$|a\rangle, |b\rangle, |c\rangle, |d\rangle$:
>
   $$ \text{OuterProd}(|a\rangle \otimes |b\rangle, \langle c| \otimes \langle d|) = \text{TensorProd}(|a \rangle\langle c|, |b \rangle\langle d|)$$
>
   or in short:
> 
   $$ \boxed{|ab\rangle \langle cd| = |a \rangle\langle c| \otimes |b \rangle\langle d|}$$
4. Let 
$\mathcal{B}$ be an orthonomal basis of a Hilbert space $\mathcal{H}$ and $|i\rangle, |j\rangle \in \mathcal{B}$. Then:
>
   $$
   \langle i|j \rangle = 
   \left\{
   \begin{aligned}
   & 0, \quad \text{if }i \neq j \\
   & 1, \quad \text{if }i = j
   \end{aligned}
   \right.
   $$
{: .prompt-statement}

> **Note:** It is important to distinguish between the operations of "$\cdot$" and "$\otimes$".
>
> Particularly, "$\cdot$" is used to represent multiplication between two matrices sized $m \times n$ and $n \times k$ in that order, whereas "$\otimes$" represents the Tensor product between two matrices with arbitraty sizes. In general, the result given by a operator is different from another.
>
> Inner and outer product are just two versions of multiplication between vectors, both using "$\cdot$". Only inner product reqiures two vectors with the same length.
>
> Both operators can be omitted from mathematical formulas under certain circumstances. Based on a previous definition, if two mathematical objects are both kets or bras, the operation between them is for sure "$\otimes$", and it is the only case where "$\otimes$" can be absent from the formula. W.r.t. other objects, by nature, it would very likely that the operation is "$\cdot$".
>
> In general, there is no precedence rule for the two operators. Brackets are used if further clarification is needed.
{: .prompt-notebox}


### 1.2.2. Bases
Two fundamental reference frames used to measure and represent qubit states are:

- Computational basis:

> $$|0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \qquad |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$
{: .prompt-definition}

- Hadamard basis:

> $$|+\rangle = \begin{pmatrix} \frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} \end{pmatrix} = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle), \qquad |-\rangle = \begin{pmatrix} \frac{1}{\sqrt{2}} \\ -\frac{1}{\sqrt{2}} \end{pmatrix} = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$$
{: .prompt-definition}

<div align="center">
  <img src="/Resources/Introduction to quantum cryptography/Computational vs Hadamard basis.png" alt="Illustration of two bases" width="400" style="border-radius: 20px;">
</div>

Consequently, we have the general representation of qubit w.r.t. a chosen basis (e.g. the computational basis):

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle, \quad \alpha, \beta \in \mathbb{C}, \quad |\alpha|^2 + |\beta|^2 = 1,$$

where $$\alpha, \beta$$ are called **amplitudes**.

### 1.2.3. Measurement

> **Postulate:** When a qubit in the state 
$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$ is measured, it collapses to **only one** of the two basis states:
>
- with probability 
$$|\alpha|^2$$, the outcome is $$0$$
- with probability 
$$|\beta|^2$$, the outcome is $$1$$
{: .prompt-definition}

Informally speaking, a qubit only has one of the two states, but we can't know what it is until we measure it. What we do know is the probability of getting each state as the result when we measure a qubit prepared in a given formula.

Thus, this implies that the sum of all probabilities must equal 1: 
$$|\alpha|^2 + |\beta|^2 = 1$$.

Overall, a register with *n* qubits w.r.t the computational basis in general yields:

- Can be represented in a quantum circuit diagram by n parallel wires with the order from top to bottom as same as the register.

- Hilbert space: $$\mathbb{C}^2 \otimes \dots \otimes \mathbb{C}^2 \cong \mathbb{C}^{2^n}$$.

- Quantum state:
$$|\psi\rangle = \sum_{i \in \{0, 1\}^n} \alpha_i |i\rangle, \quad \alpha_i \in \mathbb{C}$$.

- $$|\psi\rangle$$ 
is said to be **normalized** if 
$$\||\psi\rangle\| = 1$$, that is $$\langle\psi|\psi\rangle = 1$$, or equivalently $$\sum |\alpha_i|^2 = 1$$. If no further specification is mentioned, every quantum state $|\psi\rangle$ is assumed to be normalized.

- Quantum state collapses to 
$$|i\rangle$$ with probability $$|\alpha_i|^2$$.

> **Note:** A measurement can only be performed once a measurement basis is choosen as a reference frame. Different bases yield different results.
{: .prompt-notebox}


#### Partial measurement
If we want to measure some qubits of $$|\psi\rangle$$, we have the partial measurement. For instance, measuring the first qubit of $$|\psi\rangle$$ yields:

- The first qubit output is 
$$b$$ with probability $$p_b = \sum_{i \in \{0, 1\}^{n-1}} |a_{bi}|^2$$

- Quantum state collapses to 
$$\sum_{i \in \{0, 1\}^{n-1}} \frac{\alpha_{bi}}{\sqrt{p_b}}|bi\rangle$$

#### Projective measurement
Measuring the state $$|\psi\rangle$$ according to the projectors $$\Pi_1, \dots, \Pi_k$$, s.t. $$\sum_{i=1}^k \Pi_i = I$$:

- Output is 
$$i \in [k]$$ with probability $$\|\Pi_i |\psi\rangle\|^2$$
- Quantum state collapses to 
$$\frac{\Pi_i |\psi\rangle}{\|\Pi_i |\psi\rangle\|}$$

> Check out [3.2.](#32-measurement-outcomes-for-mixed-states) for more details about projectors.


### 1.2.4. Putting it all together
Consider a quantum state $$|\psi\rangle = \sum_{i \in \{0, 1\}^n} \alpha_i|i\rangle$$. We have the following properties:
<br>
- <div class="math-left">$$\langle\psi| =  \sum_{i \in \{0, 1\}^n} \overline{\alpha_i}\langle i|$$</div>

> **Proof**:
<br>
We have: $$|\psi\rangle = \sum_{i \in \{0, 1\}^n} \alpha_i|i\rangle = \sum_{i \in \{0, 1\}^n} |\alpha_i i\rangle$$
<br>
Hence: $$\langle\psi| = \sum_{i \in \{0, 1\}^n} \langle\alpha_i i| = \sum_{i \in \{0, 1\}^n} \overline{\alpha_i i^T} = \sum_{i \in \{0, 1\}^n} \overline{\alpha_i} \langle i|$$
{: .prompt-proof}

- Measuring 
$$|\psi\rangle$$ in basis $$\{|\varphi_j\rangle\}_{j \in \{0, 1\}^n}$$ causes the quantum state to collapse to $$|\varphi_j\rangle$$ with probability $$|\langle\psi|\varphi_j\rangle|^2 = |\sum_{i \in \{0, 1\}^n} \overline{\alpha_i}\langle i|\varphi_j\rangle|^2$$
<br>
- I.e., we can write 
$$
|\psi\rangle = \sum_{i \in \{0, 1\}^n} \beta_i|\varphi_j\rangle$$ where $$\beta_i = \langle\psi|\varphi_j\rangle = \sum_{i \in \{0, 1\}^n} \overline{\alpha_i}\langle i|\varphi_j\rangle
$$


## 1.3. Quantum unitaries
W.r.t a complex matrix

$$A = \begin{pmatrix} a_{11} & \dots & a_{1n} \\ \vdots & \ddots & \vdots \\ a_{m1} & \dots & a_{mn} \end{pmatrix},$$

we define the **adjoint** of A as:

$$A^\dagger = \overline{A^T} = \begin{pmatrix} \overline{a_{11}} & \dots & \overline{a_{m1}} \\ \vdots & \ddots & \vdots \\ \overline{a_{1n}} & \dots & \overline{a_{mn}} \end{pmatrix}$$

Then, it is clear that 
$|\psi\rangle = \langle\psi|^\dagger$.

### 1.3.1. Unitary operators
A unitary operator, represented by a square matrix (U), describes the evolution of quantum states and satisfies:

> $$\boldsymbol{UU^\dagger = U^\dagger U = I} \text{, i.e. } \boldsymbol{U^\dagger = U^{-1}}$$
{: .prompt-noteframe}

> **Statement:** For every $$v \in \mathbb{C}^{n}$$ and unitary operator $$U \in \mathbb{C}^{n \times n}$$:
$$\|Uv\| = \|v\|$$.
Therefore, for any valid quantum state $$|\psi\rangle$$, $$U|\psi\rangle$$ is also guaranteed to be a valid quantum state.
{: .prompt-statement}

> **Proof:**
<br>
With a vector $$v$$, we have that:
$$\|v\|^2 = \langle v|v \rangle$$
<br>
After applying a unitary operator $$U$$:
<br>
$$
\qquad \|Uv\|^2 = \langle Uv|Uv \rangle = \overline{(Uv)^T} |Uv\rangle = \overline{v^T}\text{ }\overline{U^T}\text{ }|Uv\rangle = \langle v|U^\dagger U|v\rangle = \langle v|v \rangle
$$ (since $$U^\dagger U = I$$)
<br>
Hence, $$\|Uv\|^2 = \|v\|^2$$ and $$\|Uv\| = \|v\|$$.
{: .prompt-proof}

<!-- > **Note:** Since a unitary operator preserves the norm of a vector, applying it to a quantum state keeps the total probability equal to 1, ensuring the state remains valid. -->

### 1.3.2. Commonly used unitary operators

#### The Pauli X gate

> $$
> X = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}
> $$
{: .prompt-definition }

Acting on the basis states:

$$
\boxed{X|0\rangle = |1\rangle, \quad X|1\rangle = |0\rangle}
$$

$$
X|+\rangle = |+\rangle, \quad X|-\rangle = -|-\rangle
$$

> **Properties:**
- $$X^2 = I$$ 
($$X^\dagger = X$$).
- $$X$$
is the **NOT operation** in the **computational basis**. In a quantum circuit, it may be represented as a symbol "$$\oplus$$".
- $$\{|+\rangle, |-\rangle\}$$ 
are the $$\pm 1$$ eigenvectors of $$X$$, so it is also called as the $$X$$-basis.
<br>
- $$X = |+\rangle\langle+| - |-\rangle\langle-|$$
.
{: .prompt-statement }

#### The Pauli Z gate

> $$
> Z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}
> $$
{: .prompt-definition }

Acting on the basis states:

$$
Z|0\rangle = |0\rangle, \quad Z|1\rangle = -|1\rangle
$$

$$
\boxed{Z|+\rangle = |-\rangle, \quad Z|-\rangle = |+\rangle}
$$

> **Properties:**
- $$Z^2 = I$$ 
($$Z^\dagger = Z$$).
- $$Z$$
is the **NOT operation** in the **Hadamard basis**.
- $$\{|0\rangle, |1\rangle\}$$ 
are the $$\pm 1$$ eigenvectors of $$Z$$, so it is also called as the $$Z$$-basis.
<br>
- $$Z = |0\rangle\langle0| - |1\rangle\langle1|$$
.
{: .prompt-statement }

#### The Hadamard gate

> $$
> H = \frac{1}{\sqrt2}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}
> $$
{: .prompt-definition }

Acting on the basis states:

> $$
H|0\rangle = |+\rangle, \qquad H|1\rangle = |-\rangle
\\
H|+\rangle = |0\rangle, \qquad H|-\rangle = |1\rangle
$$
{: .prompt-noteframe}

> **Properties:**
- $$H^2 = I$$ 
($$H^\dagger = H$$).
- $$H$$
switches between the computational basis and the Hadamard basis.
{: .prompt-statement }

### 1.3.3. Quantum circuits
A unitary operator $$U$$ can be visualized as a gate, where the number of input states equals the number of output states.

<div align="center">
  <img src="/Resources/Introduction to quantum cryptography/unitary operator.png" alt="Unitary operator" width="150" style="border-radius: 20px;">
</div>

> **Note:**
1. If $$A, B \in \mathbb{C}^{n \otimes n}$$ are unitary, then $$AB$$ is also unitary.
2. If $$A \in \mathbb{C}^{n \otimes n}, B \in \mathbb{C}^{m \otimes m}$$ are unitary, then $$A \otimes B$$ is also unitary.
3. Let $$A \in \mathbb{C}^{2^n \otimes 2^n}$$ be unitary. The unitary $$cA \in \mathbb{C}^{2^{n+1} \otimes 2^{n+1}}$$ (*controlled A*) is the unitary that acts on one control qubit and an $$n$$-qubit target register, transforming a general joint state as follows:
>
$$
cA: \sum_{y \in \{0, 1\}^n} \left(\alpha_{0y}|0\rangle|y\rangle + \alpha_{1y}|1\rangle|y\rangle\right) \mapsto \sum_{y \in \{0, 1\}^n} \left(\alpha_{0y}|0\rangle|y\rangle + \alpha_{1y}|1\rangle \textcolor{red}{A} |y\rangle\right)
$$
{: .prompt-notebox}

<div style="display: flex; justify-content: space-evenly; flex-wrap: wrap; padding: 1rem 0;">
  <div style="text-align: center;">
    <div style="height: 160px; display: flex; align-items: center; justify-content: center;">
      <img src="/Resources/Introduction to quantum cryptography/AB.png" alt="AB unitary" width="180" style="border-radius: 8px;">
    </div>
    <p style="font-style: italic; font-size: 0.85rem; color: var(--text-muted-color); margin-top: .3rem;">
      Circuit with AB
    </p>
  </div>

  <div style="text-align: center;">
    <div style="height: 160px; display: flex; align-items: center; justify-content: center;">
      <img src="/Resources/Introduction to quantum cryptography/AxB.png" alt="A ⊗ B unitary" width="150" style="border-radius: 8px;">
    </div>
    <p style="font-style: italic; font-size: 0.85rem; color: var(--text-muted-color); margin-top: .3rem;">
      Circuit with A ⊗ B
    </p>
  </div>

  <div style="text-align: center;">
    <div style="height: 160px; display: flex; align-items: center; justify-content: center;">
      <img src="/Resources/Introduction to quantum cryptography/cA example.png" alt="cA example" width="270" style="border-radius: 8px;">
    </div>
    <p style="font-style: italic; font-size: 0.85rem; color: var(--text-muted-color); margin-top: .3rem;">
      Circuit with cA and its operation on
      <br>
      two qubits in computational basis states
    </p>
  </div>
</div>

In general, quantum circuits are drawn left to right, in the order gates are applied. But when written as matrix products, the composition order reverses because of how matrix multiplication works.

> *For example:*
> <br>
> Given a quantum circuit:
> <div align="center">
>   <img src="/Resources/Introduction to quantum cryptography/quantum circuit example.png" alt="Quantum circuit example" width="400" style="border-radius: 16px;">
> </div>
> The quantum state represented by the circuit can be written as:
> <div align="center">
> $$(S_1 \otimes c(Y)_{23} \otimes I_4)(c(Z)_{12} \otimes I_3 \otimes X_4)(H_1 \otimes c(X)_{24} \otimes I_3)|q_0\rangle|q_1\rangle|q_2\rangle|q_3\rangle$$
> </div>


# 2. Entanglement
## 2.1. Entangled states
A bipartite state 
$$|\psi\rangle_{AB}$$ is said to be a **product state** if there exists $$|\psi_1\rangle_A$$ and $$|\psi_2\rangle_B$$ such that:
<div class="math-center">$$|\psi\rangle_{AB} = |\psi_1\rangle_A \otimes |\psi_2\rangle_B$$</div>

> If a quantum state is not a product state, it is **entangled**.
{: .prompt-definition}

> *For example:* 
> $$\frac{1}{\sqrt{2}}(|00\rangle + |10\rangle) = \left(\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)\right) \otimes |0\rangle$$ is a product state, whereas $$\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$ is an entangled state.


## 2.2. Bell basis
**Bell states** (also called **EPR pairs**, which stands for *Einstein-Podolsky-Rosen* pairs) are four specific maximally entangled quantum states of two qubits. They represent the simplest examples of quantum entanglement.

Four maximally entangled two-qubit Bell states form a maximally entangled basis, known as the **Bell basis**, of the four-dimensional Hilbert space for two qubits, which are often described and denoted as:

$$
\{|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)\text{, }\text{ } \\
|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle), \\
|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle), \\
|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)\}\text{ }
$$

For convenience, I would use alternative symbols for EPR pairs as 
$$|\Psi_{00}\rangle, |\Psi_{01}\rangle, |\Psi_{10}\rangle, |\Psi_{11}\rangle$$ respectively. Then, the quantum circuit for Bell state $$|\Psi_{ab}\rangle, a, b \in \{0, 1\}$$ can be visualized as:

<div align="center">
  <img src="/Resources/Introduction to quantum cryptography/bell state.png" alt="Bell state" width="400" style="border-radius: 16px;">
</div>

> **Proof:**
> <br>
> The quantum state described by the image is:
> <div class="math-center">
> $$
> \begin{aligned}
> |\Psi_{ab}\rangle &= (c(X)_{21})(I_1 \otimes H_2)|a\rangle|b\rangle \\
> &= (c(X)_{21})\bigg(|a\rangle \otimes \frac{1}{\sqrt{2}}\big(|0\rangle + (-1)^b|1\rangle\big)\bigg) \\
> &= (c(X)_{21})\big(\frac{1}{\sqrt{2}}|a\rangle|0\rangle + \frac{(-1)^b}{\sqrt{2}}|a\rangle|1\rangle\big) \\
> &= \frac{1}{\sqrt{2}}|a\rangle|0\rangle + \frac{(-1)^b}{\sqrt{2}}X|a\rangle|1\rangle \\
> &= \frac{1}{\sqrt{2}}|a0\rangle + \frac{(-1)^b}{\sqrt{2}}|(1-a)1\rangle
> \end{aligned}
> $$
> </div>
> Asserting $$(a, b) = \{(0, 0), (0, 1), (1, 0), (1,1)\}$$ into the formula, we would obtain four Bell states as our result.
{: .prompt-proof}

> *Note:* Since all four Bell states are maximally entangled, we adopt 
$$
|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle),
$$
as the default EPR pair throughout this post due to its widespread application, unless stated otherwise.


## 2.3. Perfect correlation

<div align="center">
<b>But why should we care about Bell states?</b>
</div>

Because it holds a special property in its structure. To see this clearly, consider an EPR pair 
$|\Phi^+\rangle$, which consists of two qubits, shared between Alice and Bob. When measured in the computational basis, $|\Phi^+\rangle$ collapses to either $|00\rangle$ or $|11\rangle$.

That is, if Alice and Bob both agree to measure in the computational basis, any outcome Alice obtains will instantly match Bob's result.

More importantly, this agreement holds for any *arbitrary* basis: as long as Alice and Bob measure in the same basis, their outcomes remain identical. This feature is known as $\textcolor{red}{\text{perfect correlation}}$ in a quantum communication channel between Alice and Bob.

The following theorem formalizes this property.

> **Theorem:**
For every *real basis* $$\{|\varphi_0\rangle, |\varphi_1\rangle\} \subset \{\mathbb{R}^2, \mathbb{R}^2\}$$: $$\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle) = \frac{1}{\sqrt{2}}(|\varphi_0\varphi_0\rangle + |\varphi_1\varphi_1\rangle)$$
{: .prompt-statement}

> **Proof:**
<br>
Let $$|\varphi_0\rangle = a|0\rangle + b|1\rangle = \begin{pmatrix} a \\ b \end{pmatrix}, |\varphi_1\rangle = c|0\rangle + d|1\rangle = \begin{pmatrix} c \\ d \end{pmatrix}$$.
<br>
Since $$\{|\varphi_0\rangle, |\varphi_1\rangle\}$$ is a real basis, $$a, b, c, d \in \mathbb{R}$$ and $$|a|^2 = a^2, |b|^2 = b^2, |c|^2 = c^2, |d|^2 = d^2$$.
<br>
We have:
<br>
$$
\qquad |\varphi_0\varphi_0\rangle = (a|0\rangle + b|1\rangle)(a|0\rangle + b|1\rangle) = a^2|00\rangle + ab(|01\rangle + 10\rangle) + b^2|11\rangle \\
\qquad |\varphi_1\varphi_1\rangle = (c|0\rangle + d|1\rangle)(c|0\rangle + d|1\rangle) = c^2|00\rangle + cd(|01\rangle + 10\rangle) + d^2|11\rangle
$$
<br>
Hence:
<br>
$$
\qquad \frac{1}{\sqrt{2}}(|\varphi_0\varphi_0\rangle + |\varphi_1\varphi_1\rangle) = \frac{1}{\sqrt{2}}\big((a^2 + c^2)|00\rangle + (ab + bd)(|01\rangle + |10\rangle) + (b^2 + d^2)|11\rangle\big) \text{ } (*)
$$
<br>
Furthermore, let $$U = \begin{pmatrix} a & c \\ b & d \end{pmatrix}$$ be the unitary operator that converts $$\{|0\rangle, |1\rangle\}$$ basis to $$\{|\varphi_0\rangle, |\varphi_1\rangle\}$$ basis. We have:
<br>
$$
\qquad \text{U is unitary} \iff
\left\{
\begin{aligned}
& \||\varphi_0\rangle\| = 1 \iff a^2 + b^2 = 1 \\
& \||\varphi_1\rangle\| = 1 \iff c^2 + d^2 = 1 \\
& \langle\varphi_0|\varphi_1\rangle = ac + bd = 0
\end{aligned}
\right.
$$
<br>
However, since $$U$$ is unitary, $$U^\dagger = \begin{pmatrix} a & b \\ c & d \end{pmatrix}$$ must also be unitary, which must implies:
<br>
$$
\qquad a^2 + c^2 = 1; \qquad b^2 + d^2 = 1; \qquad ab + cd = 0
$$
<br>
Substitute the result into $$(*)$$ gives us:
<br>
$$
\qquad \frac{1}{\sqrt{2}}(|\varphi_0\varphi_0\rangle + |\varphi_1\varphi_1\rangle) = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)
$$
{: .prompt-proof}

This basis-independent perfect correlation underlies practical protocols, such as:

- Quantum key distribution (E91)
- Bell inequality tests certifying non-classical entanglement
- Device-independent quantum cryptography


## 2.4. Quantum teleportation - superdense coding

### Quantum teleportation

We showed earlier that 
$$|\Phi^{+}\rangle = \frac{1}{\sqrt2}(|00\rangle + |11\rangle)$$ keeps the same coefficient pattern under any change of basis, as long as the elements are real. This independence of basis choosing is what makes the EPR pair behave as a genuinely shared resource between two separated parties. Regardless of which orthonormal basis Alice and Bob agree to measure in, their outcomes remain perfectly correlated. This correlation structure is closely related to what quantum teleportation exploits.

> - **Quantum teleportation** transfers the *quantum information* encoded in a qubit from a sender Alice to a receiver Bob at a different location, regardless of how far the two places, without physically moving the particle itself.
<br>
- Neither side needs to know the state being sent, and its contents are never directly measured, only correlations mediated by the EPR pair are used.
{: .prompt-definition }

Crucially, using this method, A and B are able to share a random bit without communication once they have agreed in advance on a common basis. This shared randomness does not make the process faster than light: whichever basis Alice measures in, Bob's particle on its own shows no change in its statistics, so he has no way to tell whether or when Alice has measured at all. This no-signaling property is what guarantees the correlations alone can never carry information faster than light — actually transferring a state still requires Alice to send Bob some classical information about her measurement outcome.

### Superdense coding

> - **Superdense coding** is the dual protocol: instead of using entanglement plus classical bits to send a *qubit*, it uses entanglement plus a *qubit* to send classical bits — twice as many as the qubit alone could carry.
<br>
- Alice and Bob start by sharing an EPR pair, exactly as in teleportation. To send one of four possible 2-bit messages, Alice applies one of four fixed operations to her own half of the pair, then physically sends that single qubit to Bob.
<br>
- Bob, now holding both particles, performs a joint measurement on the pair, which unambiguously reveals which operation Alice applied and therefore which 2-bit message she intended to send.
{: .prompt-definition }

Superdense coding is often described as the mirror image of teleportation: teleportation uses a shared EPR pair plus classical bits to send quantum information, while superdense coding uses a shared EPR pair plus a physically sent qubit to send classical information. Neither protocol beats the speed of light — teleportation still needs classical bits, and superdense coding still needs a physically sent qubit — but both show how entanglement lets a fixed resource carry more than it otherwise would.

> For a deeper dive into quantum teleportation and superdense coding, see [this paper](https://static.uni-graz.at/fileadmin/_Persoenliche_Webseite/hohenester_ulrich/diploma/radkohl21.pdf#page=12.08).


# 3. Mixed states
## 3.1. Density matrices and mixed states

So far, the states we have learned about are **pure states**, which are states we can point to and say, "the system is *exactly* this vector".

A **mixed state**, by contrast, is not a single vector at all. It is a statistical mixture, a statement of the form "the system is 
$|\psi_1\rangle$ with probability $p_1$, or $|\psi_2\rangle$ with probability $p_2$, $\dots$" and we simply do not know which. And this is captured by the **density matrix**.

> - A **mixed state** is described by a probability distribution over pure states, used when the system is prepared in one of several pure states with some probability $p_i$, and only the ensemble $(p_i, |\psi_i\rangle)_{i\in[k]}$ is known.
<br>
- The **density matrix** of a pure state 
$$|\psi\rangle$$ is defined as $$|\psi\rangle\langle\psi|$$. This allows us represent a mixed state as a density matrix where the above distribution is considered:
>
  $$\rho = \sum_{i=1}^k p_i |\psi_i\rangle\langle\psi_i|$$
>
- Therefore, the density matrix of a pure state is actually a special case of the density matrix of a mixed state, when the state distribution consists of just a single state.
{: .prompt-definition }

> *For example:*
<br>
- $$|\Phi^{+}\rangle\langle\Phi^{+}| = \frac{1}{2}(|00\rangle + |11\rangle)(|00\rangle+|11\rangle) = \frac{1}{2}(|00\rangle\langle00| + |00\rangle\langle11| + |11\rangle\langle00| + |11\rangle\langle11|)$$
<br>
- Distribution 
$$(\frac{1}{2}, |0\rangle), (\frac{1}{2}, |1\rangle) \\
\frac{1}{2}|0\rangle\langle0| + \frac{1}{2}|1\rangle\langle1| = \frac{1}{2} \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix} + \frac{1}{2} \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix} = \begin{pmatrix} \frac{1}{2} & 0 \\ 0 & \frac{1}{2} \end{pmatrix}
$$
<br>
- Distribution 
$$(\frac{1}{2}, |+\rangle), (\frac{1}{2}, |-\rangle) \\
\frac{1}{2}|+\rangle\langle+| + \frac{1}{2}|-\rangle\langle-| = \frac{1}{4} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix} + \frac{1}{4} \begin{pmatrix} 1 & -1 \\ -1 & 1 \end{pmatrix} = \begin{pmatrix} \frac{1}{2} & 0 \\ 0 & \frac{1}{2} \end{pmatrix}
$$
<br>
- Distribution 
$$(\frac{2}{3}, |-\rangle), (\frac{1}{3}, |0\rangle) \\
\frac{2}{3}|-\rangle\langle-| + \frac{1}{3}|0\rangle\langle0| = \frac{1}{3} \begin{pmatrix} 1 & -1 \\ -1 & 1 \end{pmatrix} + \frac{1}{3} \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix} = \begin{pmatrix} \frac{2}{3} & -\frac{1}{3} \\ -\frac{1}{3} & \frac{1}{3} \end{pmatrix}
$$

> **Some properties of density matrices:**
1. Trace is 1.
2. Positive semi-definitive (all its eigenvalues are non-negative).
3. If 
$$\{|\psi_i\rangle\}_{i \in [k]}$$ is an orthonormal basis of a Hilbert space $\mathcal{H}$, then we have the **resolution of identity**, that is: $$\sum_{i = 1}^k |\psi_i\rangle\langle\psi_i| = I. \\$$Indeed, since 
$$\{|\psi_i\rangle\}$$ is a basis of $\mathcal{H}$, every $$|\varphi\rangle \in \mathcal{H}$$ can be expressed as: $$|\varphi\rangle = \sum_{j=1}^k p_j |\psi_j\rangle. \\$$Multiply both sides by $$\langle\psi_i|$$ from the left, we have: $$\langle\psi_i|\varphi\rangle = \sum_{j=1}^k p_j \langle\psi_i|\psi_j\rangle = p_i.\\$$Applying the operator $$\sum_{i = 1}^k |\psi_i\rangle\langle\psi_i|$$ on any arbitrary $$|\varphi\rangle$$ gives us:
$$
\\ \qquad \bigg(\sum_{i = 1}^k |\psi_i\rangle\langle\psi_i|\bigg) |\varphi\rangle = \sum_{i = 1}^k |\psi_i\rangle\langle\psi_i|\varphi\rangle = \sum_{i=1}^k p_i |\psi_i\rangle = |\varphi\rangle, \\
$$which implies that $$\sum_{i = 1}^k |\psi_i\rangle\langle\psi_i| = I$$.
{: .prompt-statement}


## 3.2. Measurement outcomes for mixed states

<div align="center">
<b>Given a mixed state represented by a density matrix $\rho$, how do we calculate the probability of a given measurement outcome?</b>
</div>

Suppose that 
$|\psi\rangle \in \mathcal{H}$ is a **pure state** where $$\{|i\rangle\}$$ be an orthonormal basis, and $\rho_\psi = |\psi\rangle \langle \psi|$ is the density matrix generated by $|\psi\rangle$. Let $$\Pi_i$$ be the projector corresponding to output $i$, defined as:

$$\Pi_i = |i\rangle \langle i|$$

> **Some properties of projectors:**
- $\Pi_i^\dagger = \Pi_i$
- $\Pi_i^2 = \Pi_i$
- $\Pi_i \Pi_j = 0  \qquad (i \neq j)$
- Resolution of identity: $\sum_i \Pi_i = I$
{: .prompt-statement}

We redefine the probability of outcome 
$i$ when measure $$|\psi\rangle$$ based on the **Born rule**:

$$
\operatorname{P}(i \text{ | } \psi) = \langle \psi| \Pi_i |\psi\rangle = \langle \psi| i \rangle \langle i | \psi\rangle = |\langle \psi|i\rangle|^2 \quad \text{or equivalently} \quad \operatorname{P}(i \text{ | } \psi) = \|\Pi_i|\psi\rangle\|^2
$$

Since 
$\operatorname{P}(i \text{ | } \psi)$ is a scalar number, it is possible to write:

$$
\operatorname{P}(i \text{ | } \psi) = \operatorname{tr}(\langle \psi| \Pi_i |\psi\rangle) \overset{\operatorname{tr}(AB) = \operatorname{tr}(BA)}{\underset{A \in M^{m \times n}, B \in M^{n \times m}}{=}} \operatorname{tr}(|\psi\rangle \langle\psi| \Pi_i) = \operatorname{tr}(\rho_\psi \Pi_i)
$$

Hence, we can obtain the formula for measuring outcome 
$i$ from a mixed state $\rho$ using a basis $$\{|i\rangle\}$$:

$$
\begin{aligned}
\operatorname{P}(i) &= \sum_k p_k \operatorname{P} (i \text{ | } \psi_k) \\
&= \sum_k p_k \operatorname{tr}(\rho_{\psi_k} \Pi_i) \\
&= \operatorname{tr} \left(\sum_k p_k \rho_{\psi_k} \Pi_i \right) \\
&= \operatorname{tr} \left( \bigg(\sum_k p_k |\psi_k\rangle\langle\psi_k|\bigg) \Pi_i \right) \\
&= \boxed{\operatorname{tr} \left(\rho \Pi_i \right)}
\end{aligned}
$$

> *For example:*
<br>
With distribution 
$$((\frac{2}{3}, |-\rangle), (\frac{1}{3}, |0\rangle))$$ and its mixed state $$\rho = \begin{pmatrix} \frac{2}{3} & -\frac{1}{3} \\ -\frac{1}{3} & \frac{1}{3} \end{pmatrix}$$:
>
$$
\operatorname{P}(-) = \operatorname{tr}(\rho \Pi_{-}) = \operatorname{tr} \left( \frac{1}{3} \begin{pmatrix} 2 & -1 \\ -1 & 1 \end{pmatrix} \frac{1}{2} \begin{pmatrix} 1 & -1 \\ -1 & 1 \end{pmatrix} \right) = \operatorname{tr} \left(\frac{1}{6} \begin{pmatrix} 3 & -3 \\ -2 & 2 \end{pmatrix} \right) = \frac{5}{6}
$$



## 3.3. Mixed states in bipartite systems

Based on the fundamental definition, the density matrix of a statistical distribution of pure states is written as a sum of classical probabilities multiplied by the outer product of each pure state.

However, expanding our scope to a composite framework, let us assume the entire system is a bipartite system comprised of subsystems $A$ and $B$.

Suppose that we want to find the mixed state 
$\rho_{AB}$ regarding to a set of pure states $$\{|\psi_m\rangle_{AB}\}$$, $$|\psi_m\rangle_{AB} \in \mathcal{H}_A \otimes \mathcal{H}_B$$. Choose $$\mathcal{B}_A$$ and $$\mathcal{B}_B$$ to be an **orthonormal basis** of $A$ and $B$ respectively, $i, k$ are indexing variables such that $$|i\rangle_A \in \mathcal{B}_A, |k\rangle_B \in \mathcal{B}_B$$. Observe that each $|\psi_m\rangle_{AB}$ can be expressed as a linear combination (superposition) of vectors in $\mathcal{B}_A$ and $\mathcal{B}_B$:

$$
|\psi_m\rangle_{AB} = \sum_{i, k} p_{ik}^{(m)} (|i\rangle_A \otimes |k\rangle_B)
$$

Taking the complex conjugate of this composite state and put on new notations 
$j, l$ corresponding to $i, k$ for separating two sources ($$|j\rangle_A \in \mathcal{B}_A, |l\rangle_B \in \mathcal{B}_B$$) gives us:

$$
\langle\psi_m|_{AB} = \sum_{j, l} \left(p_{jl}^{(m)}\right)^* (\langle j|_A \otimes \langle l|_B)
$$

Hence, the mixed state 
$\rho_{AB}$ is calculated as:

<span id="(1)"></span>

$$
\begin{aligned}
\rho_{AB} &= \sum_{m} p_m |\psi_m\rangle\langle\psi_m|_{AB} \\
&= \sum_{m} p_m \left[ \left(\sum_{i, k} p_{ik}^{(m)} \left(|i\rangle_A \otimes |k\rangle_B\right)\right) \left(\sum_{j, l} \left(p_{jl}^{(m)}\right)^* \left(\langle j|_A \otimes \langle l|_B\right)\right) \right] \\
&= \sum_{m} p_m \left[\sum_{i,j,k,l} p_{ik}^{(m)} \left(p_{jl}^{(m)}\right)^* \left(|i\rangle_A \otimes |k\rangle_B\right)\left(\langle j|_A \otimes \langle l|_B\right) \right] \\
&= \sum_{i,j,k,l} \left[\sum_m p_m p_{ik}^{(m)} \left(p_{jl}^{(m)}\right)^*\right] |i \rangle\langle j|_A \otimes |k \rangle\langle l|_B \\
&= \boxed{\sum_{i,j,k,l} p_{ijkl} |i \rangle\langle j|_A \otimes |k \rangle\langle l|_B} \qquad \boxed{1}
\end{aligned}
$$


## 3.4. Partial trace and reduced states

Suppose $\rho_{AB}$ describes the joint state of a two-party system $A$ and $B$. In many situations, we only have physical access to subsystem $A$, since $B$ might belong to a different party, or simply be inaccessible to us. We would still like to correctly predict the outcome statistics of *any* measurement performed only on $A$, without needing to know anything about $B$.

This raises a task for finding some description of $A$ *alone* (independent of $B$) that still gives the correct measurement statistics. The intuition mirrors classical probability: if $p(x,y)$ is a joint distribution over two variables and we only care about $X$, we recover its marginal by summing out $Y$:

$$
p(x) = \sum_y p(x,y)
$$

The quantum analogue of this "summing out" is the **partial trace**, and applying it to $\rho_{AB}$ gives what is called the **reduced state** of $A$:

>
- Let 
$\rho_{AB}$ be a bipartite mixed state having the same form as [[1]](#(1)) and $|u\rangle_B \in \mathcal{B}_B'$ where $\mathcal{B}_B'$ is some orthonormal basis of $B$. The **partial trace** over $B$ is formally defined as:
>
  $$
  \begin{aligned}
  \operatorname{tr}_B(\rho_{AB}) :&= \boxed{\sum_u \left(I_A \otimes \langle u|_B\right) \rho_{AB} \left(I_A \otimes |u\rangle_B\right)} \\
  &= \sum_u \left(I_A \otimes \langle u|_B\right) \big(\sum_{i,j,k,l} p_{ijkl} |i \rangle\langle j|_A \otimes |k \rangle\langle l|_B\big) \left(I_A \otimes |u\rangle_B\right) \\
  &= \sum_{i, j, k, l, u} p_{ijkl} |i \rangle\langle j|_A \otimes \left(\langle u|k \rangle\langle l|u \rangle\right)_B \\
  &= \sum_{i, j, k, l, u} p_{ijkl} |i \rangle\langle j|_A \otimes \left(\langle l|u \rangle\langle u|k \rangle\right)_B \\
  &= \sum_{i, j, k, l} p_{ijkl} |i \rangle\langle j|_A \otimes \langle l| \bigg(\sum_u |u \rangle\langle u| \bigg) |k \rangle \\
  &= \sum_{i, j, k, l} p_{ijkl} |i \rangle\langle j|_A \otimes \langle l| I_B |k \rangle \\
  &= \sum_{i, j, k, l} p_{ijkl} \delta_{lk} |i \rangle\langle j|_A, \quad \text{where } \delta_{lk} = \langle l|k \rangle
  \end{aligned}
  $$
>
  Since 
  $k, l$ are basis vectors of $B$, we have that $$\delta_{lk} \neq 0 \iff \delta_{lk} = 1 \iff l = k$$.
>
- Hence, the partial trace over system $B$, which is equivalent to the **reduced state** of system $A$, can be written in the form:
>
  $$
  \boxed{\rho_A = \operatorname{tr}_B(\rho_{AB}) = \sum_{i, j, k} p_{ijkk} |i \rangle\langle j|_A}
  $$
>
- In a simplier manner, w.r.t. an operation of the form
$$M_A \otimes N_B$$ where $$M_A$$ operates on $$\mathcal{H}_A$$ and $$N_B$$ operates on $$\mathcal{H}_B$$, $$\operatorname{tr}_B$$ is defined such that:
>
  $$
  \operatorname{tr}_B(M_A \otimes N_B) = M_A \cdot \operatorname{tr}(N_B)
  $$
{: .prompt-definition }


> **Remark:**
- $\operatorname{tr}(\rho_A) = 1$.
- $\rho_A$ contains all information accessible by measurements performed only on $A$.
- Information about correlations with $B$ is generally lost.
{: .prompt-notebox}

> *For example:*
<br>
Density matrix of pure state 
$$|\Phi^+\rangle$$ for both parties $A$ and $B$:
<br>
$$
\rho_{AB} = |\Phi^+\rangle \langle\Phi^+| = \frac{1}{2}(|00\rangle + |11\rangle)(\langle 00| + \langle 11|) = \frac{1}{2} (|00\rangle\langle 00| + |00\rangle\langle 11| + |11\rangle\langle 00| + |11\rangle\langle 11|)
$$
> When the EPR pair is separately shared for $A$ and $B$, we trace out the second qubit to obtain the reduced state for $A$, which is a mixed state described by:
> 
$$
\rho_A = \operatorname{tr}_B(\rho_{AB}) = \frac{1}{2}|0\rangle\langle0| + \frac{1}{2}|1\rangle\langle1| = \begin{pmatrix} \frac{1}{2} & 0 \\ 0 & \frac{1}{2} \end{pmatrix}
$$

Applying the same calculation, we obtain
$$\rho_A = \rho_B = \frac{I}{2}$$. Hence, subsystems $A$ and $B$ are said to be **maximally mixed**.


## 3.5. No-signaling

We have shown that the reduced state of subsystem 
$A$ in a shared EPR pair is $\rho_A = \frac{I}{2}$, and this entails special properties for the subsystem $A$. Measuring an outcome $i$ regarding to *any* one-dimensional projector $\Pi_i$ gives:

$$
\operatorname{P}(i) = \operatorname{tr} \left(\frac{I}{2} \Pi_i \right) = \frac12\operatorname{tr}\left(\Pi_i \right) = \frac12
$$

That is, any measurement on $A$ yields a uniform outcome distribution. This is known as **local randomness**.

<div align="center">
<b>So what does this mean?</b>
</div>

It means that although A and B share an entangled state, <u>A cannot extract any information about B's state or actions <b>without any classical communication</b></u>. Whichever basis A chooses to measure, her outcome is *completely random*. Furthermore, without a prior agreement on the measurement basis, A cannot be sure whether B uses the same basis, and so cannot confirm if their outcomes match. This lack of information prevents A from knowing B's measurement result with certainty, reducing her prediction to a random guess.

This constraint of quantum communication is formalised as the **no-communication theorem** (also referred to as the **no-signaling** principle). It preserves the principle of causality in quantum mechanics and ensures that information transfer does not violate Einstein's theory of special relativity: *"no signal or information can travel faster than the speed of light in a vacuum"*.

> **Key Takeaways:** Perfect correlation & No-signaling
<br>
- **Perfect correlation:**
  - *Mechanism:* When an entangled pair is measured in the *same* basis on both sides, the two outcomes are perfectly correlated. Each individual outcome is still random, but knowing one instantly determines the other, and this holds for *any* shared real basis, not just one fixed choice.
  - **Application:** Serves as the functional resource enabling practical protocols, including Quantum Key Distribution (E91), Quantum Teleportation, and Superdense Coding.
- **No-Signaling Principle:**
  - **Mechanism:** Prohibits faster-than-light information transfer via quantum measurement alone, preserving causality and consistency with special relativity.
  - **Physical basis:** Driven by *local randomness* $\rho_A = \rho_B = \frac{I}{2}$ for a EPR pair, each party’s *local* outcome statistics remain uniformly random regardless of what the other party does. Without classical communication conveying the other party's basis choice, a local outcome carries no exploitable signal.
{: .prompt-statement}