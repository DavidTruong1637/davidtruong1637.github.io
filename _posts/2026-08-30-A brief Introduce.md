---
title: "Introduction to quantum cryptography"
date: 2026-08-30 00:00:00 +0700
category: [quantum]
tags: [quantum, theory]
---

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

### Dirac notation
A quantum state vector can be expressed in the following form:

**Ket: column vector**

$$|\psi\rangle = \begin{pmatrix} a_1 \\ a_2 \\ \vdots \\ a_m \end{pmatrix} \qquad |\varphi\rangle = \begin{pmatrix} b_1 \\ b_2 \\ \vdots \\ b_m \end{pmatrix}$$

**Bra: row vector**

$$\langle\psi| = \begin{pmatrix} \overline{a_1} & \overline{a_2} & \dots & \overline{a_m} \end{pmatrix}$$

$$\langle\varphi| = \begin{pmatrix} \overline{b_1} & \overline{b_2} & \dots & \overline{b_m} \end{pmatrix}$$

where $$\overline{x}$$ $$(x \in \mathbb{C})$$ is the conjugate of $$x$$.

**Bra-ket: inner product**

$$\langle\varphi|\psi\rangle = \begin{pmatrix} \overline{b_1} & \overline{b_2} & \dots & \overline{b_m} \end{pmatrix} \begin{pmatrix} a_1 \\ a_2 \\ \vdots \\ a_m \end{pmatrix} = \sum_{i \in [m]} \overline{b_i} a_i \in \mathbb{C}$$

**Ket-bra: outer product**

$$|\varphi\rangle\langle\psi| = \begin{pmatrix} b_1 \\ b_2 \\ \vdots \\ b_m \end{pmatrix} \begin{pmatrix} \overline{a_1} & \overline{a_2} & \dots & \overline{a_m} \end{pmatrix} = \begin{pmatrix} b_1\overline{a_1} & b_1\overline{a_2} & \dots & b_1\overline{a_m} \\ b_2\overline{a_1} & b_2\overline{a_2} & \dots & b_2\overline{a_m} \\ \vdots & \vdots & \ddots & \vdots \\ b_m\overline{a_1} & b_m\overline{a_2} & \dots & b_m\overline{a_m} \end{pmatrix} \in \mathbb{C}^{m \times m}$$

**Tensor product**

$$A = \begin{pmatrix} a_{11} & \dots & a_{1n} \\ \vdots & \ddots & \vdots \\ a_{m1} & \dots & a_{mn} \end{pmatrix}, \qquad B = \begin{pmatrix} b_{11} & \dots & b_{1l} \\ \vdots & \ddots & \vdots \\ b_{k1} & \dots & b_{kl} \end{pmatrix}$$

$$A \otimes B = \begin{pmatrix} a_{11}B & \dots & a_{1n}B \\ \vdots & \ddots & \vdots \\ a_{m1}B & \dots & a_{mn}B \end{pmatrix} = \begin{pmatrix} a_{11}b_{11} & \dots & a_{1n}b_{1l} \\ \vdots & \ddots & \vdots \\ a_{m1}b_{k1} & \dots & a_{mn}b_{kl} \end{pmatrix} \in \mathbb{C}^{mk \times nl}$$

Tensor product of quantum states is defined as follows:

$$|\psi_{AB}\rangle = |\psi_A\psi_B\rangle \overset{\text{def}}{=} |\psi_A\rangle |\psi_B\rangle = |\psi_A\rangle \otimes |\psi_B\rangle$$

For example:

$$|00\rangle = |0\rangle \otimes |0\rangle = \begin{pmatrix} 1 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \\ 0 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \\ 0 \\ 0 \end{pmatrix}, \qquad |01\rangle = |0\rangle \otimes |1\rangle = \begin{pmatrix} 1 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \\ 0 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix},$$

$$|10\rangle = |1\rangle \otimes |0\rangle = \begin{pmatrix} 0 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \\ 1 \begin{pmatrix} 1 \\ 0 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 1 \\ 0 \end{pmatrix}, \qquad |11\rangle = |1\rangle \otimes |1\rangle = \begin{pmatrix} 0 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \\ 1 \begin{pmatrix} 0 \\ 1 \end{pmatrix} \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 0 \\ 1 \end{pmatrix}.$$

### Bases
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

Consequently, we have the general representation of qubit w.r.t a chosen basis (e.g. the computational basis):

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle, \quad \alpha, \beta \in \mathbb{C}, \quad |\alpha|^2 + |\beta|^2 = 1,$$

where $$\alpha, \beta$$ are called **amplitudes**.

### Measurement
When a qubit in the state $$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$ is measured, it collapses to **only one** of the two basis states:

- with probability 
$$|\alpha|^2$$, the outcome is $$|0\rangle$$
- with probability 
$$|\beta|^2$$, the outcome is $$|1\rangle$$

Informally speaking, a qubit only has one of the two states, but we can't know what it is until we measure it. What we do know is the probability of getting each state as the result when we measure a qubit prepared in a given formula.

Thus, this implies that the sum of all probabilities must equal 1: 
$$|\alpha|^2 + |\beta|^2 = 1$$.

Overall, a register with *n* qubits w.r.t the computational basis in general has:

- Hilbert space: $$\mathbb{C}^2 \otimes \dots \otimes \mathbb{C}^2 \cong \mathbb{C}^{2^n}$$

- Quantum state:
$$|\varphi\rangle = \sum_{i \in \{0, 1\}^n} \alpha_i |i\rangle, \quad \alpha_i \in \mathbb{C}, \quad \sum |\alpha_i|^2 = 1$$

- Quantum state collapses to 
$$|i\rangle$$ with probability $$|\alpha_i|^2$$


## 1.3. Quantum unitaries
W.r.t a complex matrix

$$A = \begin{pmatrix} a_{11} & \dots & a_{1n} \\ \vdots & \ddots & \vdots \\ a_{m1} & \dots & a_{mn} \end{pmatrix},$$

we define the **adjoint** of A as:

$$A^\dagger = \overline{A^T} = \begin{pmatrix} \overline{a_{11}} & \dots & \overline{a_{m1}} \\ \vdots & \ddots & \vdots \\ \overline{a_{1n}} & \dots & \overline{a_{mn}} \end{pmatrix}$$

### Unitary operators
A unitary operator, represented by a square matrix (U), describes the evolution of quantum states and satisfies:

> $$\boldsymbol{UU^\dagger = U^\dagger U = I} \text{, i.e. } \boldsymbol{U^\dagger = U^{-1}}$$
{: .prompt-noteframe}

> **Statement:** For every $$v \in \mathbb{C}^{n}$$ and unitary operator $$U \in \mathbb{C}^{n \times n}$$:
$$||Uv|| = ||v||$$.
Therefore, for any valid quantum state $$|\psi\rangle$$, $$U|\psi\rangle$$ is also guaranteed to be a valid quantum state.
{: .prompt-statement}

> **Proof:**
<br>
With a vector $$v$$, we have that:
$$||v||^2 = \langle v|v \rangle$$
<br>
After applying a unitary operator $$U$$:
<br>
$$
\qquad ||Uv||^2 = \langle Uv|Uv \rangle = \overline{(Uv)^T} |Uv\rangle = \overline{v^T}\text{ }\overline{U^T}\text{ }|Uv\rangle = \langle v|U^\dagger U|v\rangle = \langle v|v \rangle
$$ (since $$U^\dagger U = I$$)
<br>
Hence, $$||Uv||^2 = ||v||^2$$ and $$||Uv|| = ||v||$$.
{: .prompt-proof}

<!-- > **Note:** Since a unitary operator preserves the norm of a vector, applying it to a quantum state keeps the total probability equal to 1, ensuring the state remains valid. -->

### Commonly used unitary operators

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
maps the computational basis to the Hadamard basis and vice-versa.
{: .prompt-statement }

### Quantum circuits
A unitary operator $$U$$ can be visualized as a gate, where the number of input states equals the number of output states.

<div align="center">
  <img src="/Resources/Introduction to quantum cryptography/unitary operator.png" alt="Unitary operator" width="150" style="border-radius: 20px;">
</div>

> **Note:**
1. If $$A, B \in \mathbb{C}^{n \otimes n}$$ are unitary, then $$AB$$ is also unitary.
2. If $$A \in \mathbb{C}^{n \otimes n}, B \in \mathbb{C}^{m \otimes m}$$ are unitary, then $$A \otimes B$$ is also unitary.
3. Let $$A \in \mathbb{C}^{2^n \otimes 2^n}$$. The unitary $$cA \in \mathbb{C}^{2^{n+1} \otimes 2^{n+1}}$$ (*controlled A*) is the unitary that:
>
$$
cA: \sum_{y \in \{0, 1\}^n} \alpha_{0y}|0\rangle|y\rangle + \alpha_{1y}|1\rangle|y\rangle \mapsto \sum_{y \in \{0, 1\}^n} \alpha_{0y}|0\rangle|y\rangle + \alpha_{1y}|1\rangle \textcolor{red}{A} |y\rangle
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
      <img src="/Resources/Introduction to quantum cryptography/cA.png" alt="cA unitary" width="150" style="border-radius: 8px;">
    </div>
    <p style="font-style: italic; font-size: 0.85rem; color: var(--text-muted-color); margin-top: .3rem;">
      Circuit with cA
    </p>
  </div>
</div>