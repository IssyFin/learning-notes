# Unitary Matrices

_Quantum Computing › Introduction to Quantum Computing_

[Course link](https://pennylane.ai/codebook/introduction-to-quantum-computing/unitary-matrices)

<!-- notion-import -->
## Notes from Notion

_Copied from **Unitary Matrices** in [🔆 IQC (introduction to quantum computing)](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)._

### Operations on Qubits

All operations (gates) on qubits are effectively Unitary Matrices; they are normalized (have length 1) and $`UU^† = U^†U = I_n`$, where $`U^†`$ is the conjugate transpose.

Each unitary matrix is given a function name for the transform, and we use those in our quantum circuits:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

### Unitary Parameterization

The exercises below will demonstrate how we can define our matrices with only 3 real numbers (despite the nature of complex numbers)

#### Exercise I.3.1

Suppose we write $`U=\begin{pmatrix}a&b\\c&d\end{pmatrix}`$. Evaluate the matrix product $`UU^†`$ and write down the set of equations that must be satisfied for U to be unitary.

What does this tell us about the rows of U? Do the same for $`U^†U`$and check what this tells you about the columns of U.

---

First, Find $`U^†`$:

$$
U^† = ((U)^T)^*\\
U^† = (\begin{pmatrix}a&b\\c&d\end{pmatrix}^T)^*\\
U^† = \begin{pmatrix}a&d\\c&b\end{pmatrix}^*=\begin{pmatrix}a'&c'\\b'&d'\end{pmatrix}
$$

Second, find $`UU^†`$:

$$
UU^†=\begin{pmatrix}a&b\\c&d\end{pmatrix}\begin{pmatrix}a'&c'\\b'&d'\end{pmatrix}=\begin{pmatrix}1&0\\0&1\end{pmatrix}\\
UU^† = \begin{pmatrix}aa'+bb'&ac'+bd'\\ca'+db'&cc'+dd'\end{pmatrix}=\begin{pmatrix}1&0\\0&1\end{pmatrix}\\
$$

Third, break up the row equations:

$$
aa^*+bb^*=|a|^2+|b|^2=1\\
cc^*+dd^*=|c|^2+|d|^2=1\\
a^*c+b^*d=0\\
ac^*+bd^*=0
$$

Repeat for $`U^†U`$:

$$
U^†U=\begin{pmatrix}a'&c'\\b'&d'\end{pmatrix}\begin{pmatrix}a&b\\c&d\end{pmatrix}=\begin{pmatrix}1&0\\0&1\end{pmatrix}\\
U^†U=\begin{pmatrix}aa'+cc'&a'b+c'd\\b'a+d'c&bb'+dd'\end{pmatrix}=\begin{pmatrix}1&0\\0&1\end{pmatrix}
$$

$$
aa'+cc'=|a|^2+|c|^2=1\\
bb'+dd'=|b|^2+|d|^2=1\\
ab^*+cd^*=0\\
a^*b+c^*d=0
$$

These equations show us that the lengths of both rows, and the lengths of both columns, are 1. They also show that they’re orthogonal to one another, since the products of rows with other rows give a result of 0. Therefore, we can show that U has orthonormal rows and columns, and that there are relationships between a, b, c, and d that can simplify how they are expressed. $`\checkmark`$

#### Exercise I.3.2:

Starting from $`U=\begin{pmatrix}a&b\\c&d\end{pmatrix}`$, where a, b, c, and d are complex numbers, show that U can be expressed as $`U=\begin{pmatrix}a&-e^{i\beta}c^*\\c&e^{i\beta}a^*\end{pmatrix}`$, where $`\beta`$ is a real number (and we refer to the complex value $`e^{i\beta}`$ as a phase)

---

Since $`UU\dagger=I`$, then $`U^{-1}=U^\dagger`$. Therefore:

$$

$$

We can therefore express d and b in terms of a and c:

$$
d=det(U)a^*, b=-det(U)c^*\\
U=\begin{pmatrix}a&-det(U)c^*\\c&det(U)a^*\end{pmatrix}
$$

Since determinants are multiplicative, $`det(UU^\dagger)=det(U)det(U^\dagger)=1`$. The determinant of U must be a complex number with modulus 1. Therefore, $`det(U)=e^{i\beta}`$, and:

$$
U=\begin{pmatrix}a&-e^{i\beta}c^*\\c&e^{i\beta}a^*\end{pmatrix}
$$

#### Exercise I.3.3:

Starting from the expression we obtained for U, show that any unitary matrix can be expressed as the following, where $`\phi,\theta,\omega`$ are real numbers.

$$
U(\phi,\theta,\omega)=\begin{pmatrix}e^{-i(\phi+\omega)/2}cos(\theta/2)&-e^{i(\phi-\omega)/2}sin(\theta/2)\\e^{-i(\phi-\omega)/2}sin(\theta/2)&e^{i(\phi+\omega)/2}cos(\theta/2)\end{pmatrix}
$$

---

We start by expressing coordinates in polar form:

$$

a=re^{i\alpha}, c=se^{i\gamma}

$$

Since U is orthonormal, $`|a|^2+|c|^2=r^2+s^2=1`$, so:

$$
r=cos(\theta/2),s=sin(\theta/2)\\
U=\begin{pmatrix}e^{i\alpha}cos(\theta/2)&-e^{i(\beta-\gamma)}sin(\theta/2)\\e^{i\gamma}sin(\theta/2)&e^{i(\beta-\alpha)}cos(\theta/2)\end{pmatrix}
$$

We can then factor out the complex numbers; specifically, $`e^{i\beta/2}`$, and adjust the signs to match:

$$
U=\begin{pmatrix}e^{i(\alpha-\frac{\beta}{2})}cos(\theta/2)&-e^{i(\frac{\beta}{2}-\gamma)}sin(\theta/2)\\e^{i(\gamma-\frac{\beta}{2})}sin(\theta/2)&e^{i(\frac{\beta}{2}-\alpha)}cos(\theta/2)\end{pmatrix}\\
U=\begin{pmatrix}e^{-i(\frac{\beta}{2}-\alpha)}cos(\theta/2)&-e^{i(\frac{\beta}{2}-\gamma)}sin(\theta/2)\\e^{-i(\frac{\beta}{2}-\gamma)}sin(\theta/2)&e^{i(\frac{\beta}{2}-\alpha)}cos(\theta/2)\end{pmatrix}
$$

To make our substitutions, we need some combination of $`\alpha,\beta,\gamma`$ in each phase. We’ll therefore add and subtract these elements as necessary to each phase:

$$
U=\begin{pmatrix}e^{-i(\frac{\beta}{2}-\frac{\alpha}{2}-\frac{\alpha}{2}-\frac{\gamma}{2}+\frac{\gamma}{2})}cos(\theta/2)&-e^{i(\frac{\beta}{2}-\frac{\gamma}{2}-\frac{\gamma}{2}+\frac{\alpha}{2}-\frac{\alpha}{2})}sin(\theta/2)\\e^{-i(\frac{\beta}{2}-\frac{\gamma}{2}-\frac{\gamma}{2}+\frac{\alpha}{2}-\frac{\alpha}{2})}sin(\theta/2)&e^{i(\frac{\beta}{2}-\frac{\alpha}{2}-\frac{\alpha}{2}-\frac{\gamma}{2}+\frac{\gamma}{2})}cos(\theta/2)\end{pmatrix}
$$

Then, we substitute for clarity:

$$
\phi=\beta-\alpha-\gamma\\
\omega=\gamma-\alpha\\
U(\phi,\theta,\omega)=\begin{pmatrix}e^{-i(\phi+\omega)/2}cos(\theta/2)&-e^{i(\phi-\omega)/2}sin(\theta/2)\\e^{-i(\phi-\omega)/2}sin(\theta/2)&e^{i(\phi+\omega)/2}cos(\theta/2)\end{pmatrix}\checkmark
$$

---

Note that, while this is the most general parameterziation of a unitary matrix, many common single—qubit operations are much simpler than this.

### Codercises

#### Codercise I.3.1-Unitaries in Pennylane

In PennyLane, unitary operations specified by a matrix can be implemented in a quantum circuit via the QubitUnitary operation, a parameterized gate, called via the following function. Complete the quantum function to create a circuit that applies U to the qubit and returns its state.

```python
qml.QubitUnitary(U,wires=wire)
```

<details>
<summary>My Code</summary>

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

#### Codercise I.3.2-Parameterized Unitaries

Unitary matrices can be parameterized via phi, theta, and omega. In PennyLane, this operation is implemented as a gate called Rot, which takes these three parameters.

Aply the Rot operation to a qubit using the input parameters. Then, complete the QNode to return the quantum state vector, using qml.state().

```python
qml.Rot(phi,theta,omega,wires=wire)
```

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_u_as_rot(phi, theta, omega):

    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY A ROT GATE USING THE PROVIDED INPUT PARAMETERS
    qml.Rot(phi,theta,omega,wires=0)

    
    # RETURN THE QUANTUM STATE VECTOR
    return qml.state()

```

</details>

---


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
