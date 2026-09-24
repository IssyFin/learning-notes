# X and H

_Quantum Computing › Single-Qubit Gates_

[Course link](https://pennylane.ai/codebook/single-qubit-gates/x-and-h)

<!-- notion-import -->
## Notes from Notion

_Copied from **[X and H](https://pennylane.ai/codebook/single-qubit-gates/x-and-h)** in [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

### Describing Quantum Operations

We use shorthand to express unitary operations, since it can be annoying to express them in parameterized or matrix form. The action of U can be considered much easier in shorthand since matrix-vector multiplication is linear

$$
U|0⟩=\alpha|0⟩+\beta|1⟩,\\
U|1⟩=\gamma|0⟩+\delta|1⟩\\
U|\psi⟩=U(p|0⟩+q|1⟩)=U(p|0⟩)+U(q|1⟩)=p•U|0⟩+q•U|1⟩
$$

#### Exercise I.4.1

Finish evaluating the above expression to express $`U=|\psi⟩`$ as a linear combination of the two basis states.

---

$$
U|\psi⟩=p*U|0⟩+q*U|1⟩\\
=p(\alpha|0⟩+\beta|1⟩)+q(\gamma|0⟩+\delta|1⟩)\\
=(p\alpha+q\gamma)|0⟩+(p\beta+q\delta)|1⟩\checkmark

$$

Although this restructuring doesn’t appear to help much with this smaller case, when using multiple qubits, such simplification is quite helpful.

We do, however, still use matrix representations to obtain eigenvalues and eigenvectors, and they can help graphically representing functions in a quantum circuit.

### Pauli X (the Pauli X Gate)

The Pauli X gate is essentially a bit flip or a NOT operation, described as $`X=\begin{pmatrix}0&1\\1&0\end{pmatrix}`$ or $`X|0⟩=|1⟩,

X|1⟩ = |0⟩`$.

Visually, the Pauli X gate is represented in one of two ways in a quantum circuit: an X in a box (most common), or a circle and a vertical line (usually when X is involvedi n a 2-qubit operation).

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

#### Exercise I.4.2

Compute the eigenvalues and normalized eigenvectors of the X operation. Express the eigenvectors in ket notation as linear combinations of the basis kets.

---

To find the eigenvalues, we find where, for some nonzero v, $`0=\lambda*I_nv -Av`$, or where the determinant is 0.

$$
\lambda*I_2=\begin{pmatrix}\lambda&0\\0&\lambda\end{pmatrix}\\
det(\begin{pmatrix}0&1\\1&0\end{pmatrix} -\begin{pmatrix}\lambda&0\\0&\lambda\end{pmatrix})\\
=\begin{pmatrix}-\lambda&1\\1&-\lambda\end{pmatrix}\\
=\lambda^2-1\\
\lambda=+1,-1
$$

Then, to get the eigenvectors, we solve and normalize.

$$
\lambda=+1:\\
(\sigma_x-\lambda I)|\psi⟩\\
=\begin{pmatrix}0-1&1\\1&0-1\end{pmatrix}\begin{pmatrix}a\\b\end{pmatrix}\\=\begin{pmatrix}-1&1\\1&-1\end{pmatrix}\begin{pmatrix}a\\b\end{pmatrix}=\begin{pmatrix}0\\0\end{pmatrix}:\\
-a+b=0, a-b=0\\
a=b\\
|a|^2+|b|^2=1
a=b=\frac{1}{\sqrt{2}}

$$

$$
\lambda=-1:\\
(\sigma_x-\lambda I)|\psi⟩\\
=\begin{pmatrix}0-(-1)&1\\1&0-(-1)\end{pmatrix}\begin{pmatrix}a\\b\end{pmatrix}\\=\begin{pmatrix}1&1\\1&1\end{pmatrix}\begin{pmatrix}a\\b\end{pmatrix}=\begin{pmatrix}0\\0\end{pmatrix}:\\
a+b=0, a+b=0\\
a=-b\\
|a|^2+|b|^2=1\\
a=\frac{1}{\sqrt{2}}\\
b=-\frac{1}{\sqrt{2}}

$$

Representing these with basis kets, we get $`v_1`$ for $`\lambda_1=1`$ and $`v_2`$ for $`\lambda_2=-1`$:

$$
|v_1⟩=\frac{1}{\sqrt{2}}(|0⟩+|1⟩)\\
|v_2⟩=\frac{1}{\sqrt{2}}(|0⟩-|1⟩)\\ \checkmark
$$

### Hadamard

The Hadamard gate is one of the most well known in quantum computing. It is denoted by H and represented as a boxed H and considers the matrix $`H=\frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}`$. It essentially creates a uniform superposition of the two states \|0⟩ and \|1⟩.

$$
H|0⟩=\frac{1}{\sqrt{2}}(|0⟩+|1⟩)\\
H|1⟩=\frac{1}{\sqrt{2}}(|0⟩-|1⟩)
$$

Since these two states occur so often, they have special labels based on the sign of the amplitudes:

$$
H|0⟩=|+⟩=\frac{1}{\sqrt{2}}(|0⟩+|1⟩)\\\\
H|1⟩=|-⟩\frac{1}{\sqrt{2}}(|0⟩-|1⟩)\\
$$

#### Exercise I.4.3

What happens when we apply a Hadamard gate twice to an input state (eg HH\|0⟩ or HH\|1⟩)? Work out the result using bra-ket notation.

$$
HH|0⟩ = \frac{1}{\sqrt{2}}(\frac{1}{\sqrt{2}}(|0⟩+|1⟩)+\frac{1}{\sqrt{2}}(|0⟩-|1⟩))\\
=\frac{1}{\sqrt{2}}(\frac{2}{\sqrt{2}}|0⟩\\
=|0⟩
$$

$$
HH|1⟩ = \frac{1}{\sqrt{2}}(\frac{1}{\sqrt{2}}(|0⟩+|1⟩)-\frac{1}{\sqrt{2}}(|0⟩-|1⟩))\\
=\frac{1}{\sqrt{2}}(\frac{2}{\sqrt{2}}|1⟩\\
=|1⟩
$$

Applying a Hadamard gate twice to an input state simply undoes itself, producing the original state. **The Hadamard is its own inverse, and it can both create and destroy uniform superpositions!**

#### Exercise I.4.4

Show that together, \|+⟩ and \|-⟩ constitute an orthonormal basis for single-qubit states (ie they are normalized and orthogonal). This basis is called the Hadamard Basis.

Step 1: Show that each state is normalized (show that the inner product with itself gives us precisely 1)

$$
\langle+|+\rangle=(\frac{1}{\sqrt{2}}(\langle0|+\langle1|))(\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)\\
=\frac{1}{2}((\langle0|+\langle1|)(|0\rangle+|1\rangle))\\
=\frac{1}{2}(\langle0|0\rangle+\langle0|1\rangle+\langle1|0\rangle+\langle1|1\rangle)\\
=\frac{1}{2}(1+0+0+1)\\
=1\checkmark
$$

$$
\langle-|-\rangle=(\frac{1}{\sqrt{2}}(\langle0|-\langle1|))(\frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)\\
=\frac{1}{2}((\langle0|-\langle1|)(|0\rangle-|1\rangle))\\
=\frac{1}{2}(\langle0|0\rangle-\langle0|1\rangle-\langle1|0\rangle+\langle1|1\rangle)\\
=\frac{1}{2}(1-0+0-1)\\
=1\checkmark
$$

<br>Step 2: Show the two states are orthogonal (taking the inner product/dot product results in 0) (order doesn’t matter)

$$
\langle-|+\rangle=(\frac{1}{\sqrt{2}}(\langle0|-\langle1|))(\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle))\\
=\frac{1}{2}((\langle0|-\langle1|)(|0\rangle+|1\rangle))\\
=\frac{1}{2}(\langle0|0\rangle+\langle0|1\rangle-\langle1|0\rangle-\langle1|1\rangle)\\
=\frac{1}{2}(1+0-0-1)=0\checkmark
$$

#### Codercise I.4.1 - Flipping Bits

A common use of the X gate (Pauli X gate) is to initialize the state of a qubit at the beginning of an algorithm. Although we often want to start in state \|0⟩ (the default in pennylane), there are in some cases where we would want to start from \|1⟩ instead. Complete the function by using qml.PauliX to initialize the qubit’s state to \|0⟩ or \|1⟩ based on an input flag, then use qml.QubitUnitary to apply the provided unitary U.

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=1)

U = np.array([[1, 1], [1, -1]]) / np.sqrt(2)

@qml.qnode(dev)
def varied_initial_state(state):
    """Complete the function such that we can apply the operation U to
    either |0> or |1> depending on the input argument flag.

    Args:
        state (int): Either 0 or 1. If 1, prepare the qubit in state |1>,
            otherwise, leave it in state 0.

    Returns:
        np.array[complex]: The state of the qubit after the operations.
    """
    ##################
    # YOUR CODE HERE #
    ##################

    # KEEP THE QUBIT IN |0> OR CHANGE IT TO |1> DEPENDING ON THE state PARAMETER
    if state==1:
        qml.PauliX(wires=0)

    # APPLY U TO THE STATE
    qml.QubitUnitary(U,wires=0)
    
    return qml.state()

```

</details>

#### Codercise I.4.2 - Uniform Superposition

The Hadamard gate, typically represented as H, is implemented in PennyLane as qml.Hadamard. This gate creates a uniform superposition of the two states $`|0\rangle`$ and $`|1\rangle`$. Complete the function below such that it applies a Hadamard gate to the qubit and returns the state of the qubit with qml.state.

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_hadamard():
    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY THE HADAMARD GATE
    qml.Hadamard(wires=0)
    
    # RETURN THE STATE
    return qml.state()

```

</details>

#### Codercise I.4.3 - Combining X and H

Combining code from the 2 previous exercises, apply the Hadamard gate to both $`|0\rangle`$ and $`|1\rangle`$. What do the two different output sttes look like? Do you notice anything special about them?<br><br>Response: The output states look to be the same as the eigenvectors we saw earlier: $`\frac{1}{\sqrt{2}}\begin{pmatrix}1\\1\end{pmatrix}`$ and  $`\frac{1}{\sqrt{2}}\begin{pmatrix}1\\-1\end{pmatrix}`$

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_hadamard():
    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY THE HADAMARD GATE
    qml.Hadamard(wires=0)
    
    # RETURN THE STATE
    return qml.state()

```

</details>

#### Codercise I.4.4 - A QNode with X and H

Let’s combine everything. Create a device with one qubit, write a QNode applying the following circuit, and return the state. Determine the effect on the two basis states; what does this operation do?<br><br>

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

Response: This operation effectively flips $`|0\rangle`$ qubits to $`|1\rangle`$ and $`|1\rangle`$ to $`|0\rangle`$

<details>
<summary>My Code</summary>

```python
##################
# YOUR CODE HERE #
##################

# CREATE A DEVICE
dev=qml.device("default.qubit",wires=1)

@qml.qnode(dev) #create implicitly with a decoration
# CREATE A QNODE CALLED apply_hxh THAT APPLIES THE CIRCUIT ABOVE
def apply_hxh(state):
    if state==1:
        qml.PauliX(wires=0)
    qml.Hadamard(wires=0)
    qml.PauliX(wires=0)
    qml.Hadamard(wires=0)
    return qml.state()

# Print your results
print(apply_hxh(0))
print(apply_hxh(1))
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
