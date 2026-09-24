# IQC: Introduction to Quantum Computing

_Quantum Computing_

[Course link](https://pennylane.ai/codebook/introduction-to-quantum-computing)


## Unit notes


<!-- notion-import -->
## Notes from Notion

_Copied from [🔆 IQC (introduction to quantum computing)](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)._

---

### Ch1 Summary - Content

*Introduction to Quantum Computing*

---

<details>
<summary><b>States</b></summary>

A qubit is only ever *in *one state, never two at the same time. It’s just that sometimes, the state may be a linear combination of the basis states

$$
|0〉 = \begin{pmatrix}
1\\
0\\
\end{pmatrix}
, |1〉=
\begin{pmatrix}
0\\
1\\
\end{pmatrix}
\\
|Ψ〉 = Arbitrary State\\
$$

When States are orthogonal, they form a *computational basis.*

The amplitudes of the states in a computational basis can be considered the parts of the  arbitrary state representation.

$$

\\
|\psi⟩ = \alpha|0⟩ + \beta|1⟩ = \begin{pmatrix}\alpha\\\beta\end{pmatrix}
$$

To compute the bra of a state is to take its complex conjugate.

$$
|\phi⟩ = \gamma|0⟩+\delta|1⟩\\
〈\phi| = \gamma^*〈0| + \delta^*〈1| \\
〈\phi| = (\gamma^*\delta^*)
$$

The inner product of two states can be calculated by multiplyingthe bra of one state with the ket of the other state.

$$
|\psi⟩ = \alpha|0⟩ + \beta|1⟩\\
|\phi⟩ = \gamma|0⟩+\delta|1⟩\\
〈\phi|\psi⟩ = \begin{pmatrix}\gamma^*&\delta^*\end{pmatrix}\begin{pmatrix}\alpha\\\beta\end{pmatrix}
$$

Note that, to check normalization, you can calculate the inner product of a state with itself. A product of 1 means the state is normalized.

$$
|\psi⟩=\frac{3}{5}|0⟩-\frac{4}{5}e^{\frac{\pi}{6}i}|1⟩\\

〈\psi|\psi⟩=(\frac{3}{5}〈0|-\frac{4}{5}e^{-\frac{\pi}{6}i}〈1|)(\frac{3}{5}|0⟩-\frac{4}{5}e^{\frac{\pi}{6}i}|1⟩)\\
=\frac{9}{25}+\frac{16}{25}\\
\textbf{=1}\checkmark
$$

</details>

<details>
<summary><b>Measurement</b></summary>

At the beginning of an algorithm, we create qubit states and put them in superposition

At the end of algorithm, we need to get info *from* the qubits via measurement

Measurement in quantum computing is probabilistic. Prob( \|0⟩ ) = **\|α\|\^2** , Prob(\|1⟩ ) = **\|β\|\^2. <br>**We must have 100% total: \|α\|\^2 + \|β\|\^2 = 1

</details>

<details>
<summary><b>Quantum Circuits</b></summary>

We represent sequences of operation via ***quantum circuits***.

Circuits start with a set of ***wires*** representing a set of qubits (one wire per qubit). A set of qubits is called a register.

</details>

<details>
<summary><b>Unitary Matrices/Operations</b></summary>

All operations (gates) on qubits are effectively Unitary Matrices (normalized and orthogonal)

Since $`UU\dagger=I`$, then $`U^{-1}=U^\dagger`$.

</details>

### Ch 1 Summary - Code

*Introduction to Quantum Computing*

---

<details>
<summary>Making an array:</summary>

```python
ket_1 = np.array([0, 1])
```

</details>

<details>
<summary>Defining a quantum circuit:</summary>

```python
dev = qml.device('default.qubit,wires=[0,1])

@qml.qnode(dev)
def my_first_circuit(theta):
	qml.Hadamard(wires=0)
	qml.CNOT(wires=[0,1])
	qml.RZ(theta,wires=0)

return qml.probs(wires=[0,1])
```

</details>

<details>
<summary>Reusing a quantum node</summary>

```python
# This creates a QNode, binding the function and device
my_qnode = qml.QNode(my_circuit, dev)

# We set up some values for the input parameters
theta, phi, omega = 0.1, 0.2, 0.3

# Now we can execute the QNode by calling it like we would a regular function
my_qnode(theta, phi, omega)
```

</details>

<details>
<summary>Making a unitary matrix</summary>

```python
dev = qml.device("default.qubit", wires=1)

U = np.array([[1, 1], [1, -1]]) / np.sqrt(2)

@qml.qnode(dev)
def apply_u():

    ##################
    # YOUR CODE HERE #
    ##################

    # USE QubitUnitary TO APPLY U TO THE QUBIT
    qml.QubitUnitary(U,wires=0)
    
    # Return the state
    return qml.state()
```

</details>

<details>
<summary>Initializing states</summary>

\|0\> : Default

\|1\> : qml.PauliX(wires=X)

\|+\> : qml.Hadamard(wires=X)

\|-\>:  qml.PauliX(wires=X) → qml.Hadamard(wires=X)

</details>

<details>
<summary>Measuring Shortcuts</summary>

Return an array of probabilities for a set of wires:

```python
qml.probs(wires=[0,1,2])
```

</details>

---

## Lessons

1. [All About Qubits](./01-all-about-qubits/)
2. [Quantum Circuits](./02-quantum-circuits/)
3. [Unitary Matrices](./03-unitary-matrices/)
