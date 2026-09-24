# It's Just a Phase

_Quantum Computing › Single-Qubit Gates_

[Course link](https://pennylane.ai/codebook/single-qubit-gates/its-just-a-phase)

<!-- notion-import -->
## Notes from Notion

_Copied from **It’s Just a Phase (Qubit rotations and global phases)** in [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

### Global and Relative Phases

By writing an arbitrary quantum state in polar form, we can factor out the real and complex components:

$$
|\psi\rangle = \alpha|0\rangle+\beta|1\rangle\\
=\alpha e^{i\phi}|0\rangle+\beta e^{i\gamma}|1\rangle\\
=e^{i\phi}(\alpha|0\rangle+\beta e^{i(\gamma-\phi)}|1\rangle\\
$$

Since $`e^{i\phi}`$ doesn’t affect the outcome probability, we can effectively ignore it (note - commonly known as a global phase)

$$
e^{i\phi}(\alpha|0\rangle)+\beta e^{i(\gamma-\phi)}|1\rangle)=\alpha|0\rangle+\beta e^{i\theta}|1\rangle
$$

The remaining complex value, $`e^{i\theta}`$, is known as the *relative phase. *Although this phase doesn’t affect the measurement probabilities of $`|0\rangle`$ and $`|1\rangle`$, it can affect other processes.

### The Pauli Z Gate

The Pauli Z gate affects some amplitudes differently than others:

$$
|0\rangle \rightarrow |0\rangle\\
|1\rangle \rightarrow -|1\rangle
$$

Applying Z effectively “flips” the phase of the $`|1\rangle`$ state while leaving the $`|0\rangle`$ state intact.

However, when applied to other states, it will produce a relative phase:

$$
Z|+\rangle=\frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)=|-\rangle\\
Z|-\rangle=\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)=|+\rangle
$$

In other words,

$`Z=HXH`$

### Z Rotations/RZ Gates

The Z rotation is a more general form of the Pauli Z gate; Given an angle $`\omega`$ in radians, a state is rotated:

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle\\
RZ(\omega)|\psi\rangle=\alpha|0\rangle+\beta e^{i\omega}|1\rangle\\
RZ\omega=\begin{pmatrix}e^{-i\frac{\omega}{2}}&0\\0&e^{i\frac{\omega}{2}}\end{pmatrix}
$$

#### Exercise I.5.1:

Evaluate the action of the matrix $`RZ(\omega)`$ on an arbitrary state $`|\psi\rangle=\alpha|0\rangle+\beta|1\rangle`$. Explain why this is equivalent to the matrix representation $`RZ'(\omega)=\begin{pmatrix}1&0\\0&e^{i\frac{\omega}{2}}\end{pmatrix}`$

---

Using vector multiplication:

$$
\begin{pmatrix}e^{-i\frac{\omega}{2}}&0\\0&e^{i\frac{\omega}{2}}\end{pmatrix}\begin{pmatrix}\alpha\\\beta\end{pmatrix}\\
=\begin{pmatrix}\alpha e^{-i\frac{\omega}{2}}\\\beta e^{i\frac{\omega}{2}}\end{pmatrix}\\
=e^{-i\frac{\omega}{2}}\alpha|0\rangle+e^{i\frac{\omega}{2}}\beta|1\rangle\\
=e^{-i\frac{\omega}{2}}(\alpha|0\rangle+e^{i \omega}\beta|1\rangle)\\
=\alpha|0\rangle+e^{i\omega}\beta|1\rangle\\
=\begin{pmatrix}1&0\\0&e^{i\omega}\end{pmatrix}\begin{pmatrix}\alpha\\\beta\end{pmatrix}\checkmark
$$

#### Exercise I.5.2:

What is the inverse/conjugate transpose of $`RZ(\omega)`$? Express the result as a function of $`\omega`$.

---

A matrix is inverted when $`AA^{-1}=I`$:

$$
\begin{pmatrix}1&0\\0&e^{i\omega}\end{pmatrix}\begin{pmatrix}a&b\\c&d\end{pmatrix}=\begin{pmatrix}1&0\\0&1\end{pmatrix}\\
1*a+0*c=1\rightarrow a=1\\1*b+0*d=0 \rightarrow b=0\\0*a+e^{i\omega}*c=0 \rightarrow c=0\\0*b+e^{i\omega}d=1 \rightarrow d=e^{-i\omega}\\

$$

Or more simply put, a matrix is inverted when it’s turned the same degree in the opposite direction $`RZ'(\omega)=RZ(-\omega)\checkmark`$

### S and T

Besides the Pauli Z gate, there are two other special Z gates we use:

- The Phase Gate (S), where $`\omega=\frac{\pi}{2}`$
- The Pi-Over-8-Gate (T), where $`\omega=\frac{\pi}{4}`$

#### Exercise I.5.3:

Determine the matrix representation of S. What are its eigenvalues and eigenvectors? What happens when you apply S twice?

---

$$
S=\begin{pmatrix}1&0\\0&e^{i\frac{\pi}{2}}\end{pmatrix}\\
SS=\begin{pmatrix}1&0\\0&e^{i\frac{\pi}{2}}\end{pmatrix}\begin{pmatrix}1&0\\0&e^{i\frac{\pi}{2}}\end{pmatrix}\\
SS=\begin{pmatrix}1*1+0*0&1*0+0*e^{i\frac{\pi}{2}}\\0*1+e^{i\frac{\pi}{2}}&0*0+e^{i\pi}\end{pmatrix}=\begin{pmatrix}1&0\\0&e^{i\pi}\end{pmatrix}
$$

Essentially, applying S twice just means you rotate by twice the angle (a quarter turn, applying the Pauli Z)

Eigenvalues:

$$
det(A-\lambda I)=0:\\
\begin{pmatrix}1-\lambda&0\\0&e^{i\frac{\pi}{2}}-\lambda\end{pmatrix}\\
\lambda=1 ,e^{i\frac{\pi}{2}}\\
\lambda=1:\\
\begin{pmatrix}0&0\\0&e^{i\frac{\pi}{2}}\end{pmatrix} \rightarrow \begin{pmatrix}0\\1\end{pmatrix}\\
\lambda=e^{i\frac{\pi}{2}}:\\
\begin{pmatrix}1-e^{i\frac{\pi}{2}}&0\\0&0\end{pmatrix}\rightarrow \begin{pmatrix}1\\0\end{pmatrix}
$$

#### Exercise I.5.4:

How can you compute the adjoint of $`S`$, $`S^\dagger`$, using only S gates? Similarly, the adjoint of $`T`$, $`T^\dagger`$?

Essentially want to just rotate the rest of the way around, sans 1:

$`S^\dagger=SSS`$

$`T^\dagger=TTTTTTT`$

#### Codercise I.5.1: The Pauli Z Gate

The Pauli Z gate is defined where $`|0\rangle\rightarrow |0\rangle, |1\rangle \rightarrow-|1\rangle`$. Use qml.PauliZ to apply the Pauli Z gate to the $`|+\rangle`$ state. What state is it, and how do the measurement prbabilities differ?

<details>
<summary>My Code</summary>

```javascript
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_z_to_plus():
    """Write a circuit that applies PauliZ to the |+> state and returns
    the state.

    Returns:
        np.array[complex]: The state of the qubit after the operations.
    """

    ##################
    # YOUR CODE HERE #
    ##################

    # CREATE THE |+> STATE
    state = np.array([1,0])
    #Apply Hadamard to get |+> from |0>
    qml.Hadamard(wires=0)
    
    # APPLY PAULI Z
    qml.PauliZ(wires=0)
    
    # RETURN THE STATE
    return qml.state()

print(apply_z_to_plus())

```

</details>

#### Codercise I.5.2: The Z Rotation

Given some arbitrary $`|\psi\rangle=\alpha|0\rangle+\beta|1\rangle`$ and angle of rotation $`\omega`$ (in radians), the Z rotation gate RZ acts as follows:

$`RZ(\omega)|\psi\rangle=e^{-i\frac{\omega}{2}}\alpha|0\rangle+\beta e^{i\frac{\omega}{2}}|1\rangle`$

However, this prefactor of $`e^{-i\frac{\omega}{2}}`$ is also a global phase and can thus be factored out. This means that $`RZ(\omega)`$ produces

$`RZ(\omega)|\psi\rangle=e^{-i\frac{\omega}{2}}\alpha|0\rangle+\beta e^{i\frac{\omega}{2}}|1\rangle~\alpha|0\rangle+\beta e^{i\omega}|1\rangle`$.

In Pennylane, this operation is accessible as qml.RZ, which is a parameterized operation, and so we must specify not only a wire, but an angle of rotation:

```python
qml.RZ(angle,wires=wire)
```
Write a QNode that uses qml.RZ to simulate a qml.PauliZ Operation and return the state. Apply it to the $`|+\rangle`$ state to check your work.

<details>
<summary>My Code</summary>

```javascript
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def fake_z():
    """Use RZ to produce the same action as Pauli Z on the |+> state.

    Returns:
        np.array[complex]: The state of the qubit after the operations.
    """

    ##################
    # YOUR CODE HERE #
    ##################

    # CREATE THE |+> STATE
    state = np.array([1,0])
    qml.Hadamard(wires=0)
    
    # APPLY RZ
    qml.RZ(3.14159,wires=0)

    # RETURN THE STATE
    return qml.state()

```

</details>

#### Codercise I.5.3: The S and T Gates

The quarter turn $`RZ(\frac{\pi}{2})`$ and eighth turn $`RZ(\frac{\pi}{4})`$ gates also have their own names: The phase gate (S) and the T gate.

In PennyLane, they are implemented directly as the non-parameterized operations qml.S and qml.T.

Adjoints in PennyLane can be computed by applying the qml.adjoint transform to an operation before specifying its parameters and wires. For example:

```python
qml.adjoint(qml.RZ)(omega,wires=0)
```
performs the same computation as qml.RZ(-omega,wires=0) since $`RZ^\dagger(\omega)=RZ(-\omega)`$.

With the above in mind, implement the circuit below, using adjoints when necesary, and return the quantum state.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

<details>
<summary>My Code</summary>

```javascript
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def many_rotations():
    """Implement the circuit depicted above and return the quantum state.

    Returns:
        np.array[complex]: The state of the qubit after the operations.
    """

    ##################
    # YOUR CODE HERE #
    ##################

    state = np.array([1,0])
    qml.Hadamard(wires=0)
    qml.S(wires=0)
    qml.adjoint(qml.T)(wires=0)
    qml.RZ(0.3,wires=0)
    qml.adjoint(qml.S)(wires=0)
    # IMPLEMENT THE CIRCUIT

    # RETURN THE STATE

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
