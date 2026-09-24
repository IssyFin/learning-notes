# Multi-Qubit Systems

_Quantum Computing › Circuits with Many Qubits_

[Course link](https://pennylane.ai/codebook/circuits-with-many-qubits/multi-qubit-systems)

<!-- notion-import -->
## Notes from Notion

_Copied from **Multi-Qubit systems** in [💠 MQ (Many Qubits)](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)._

### Tensor Products

A single-qubit state lives in a Hilbert space (a 2-dimensional vector space spanned by basis vectors $`|0\rangle`$ and $`|1\rangle`$. By using more than one qubit, we enter a more complicated vector space. We combine Hilbert spcaes using a **tensor product** operation.

To compute the tensor product for a pair of two-dimensional vectors (two single-qubit states):

$$
\begin{pmatrix}a\\b\end{pmatrix} \otimes \begin{pmatrix}c\\d\end{pmatrix} = \begin{pmatrix}a\begin{pmatrix}c\\d\end{pmatrix}\\b\begin{pmatrix}c\\d\end{pmatrix}\end{pmatrix} = \begin{pmatrix}ac\\ad\\bc\\bd\end{pmatrix}
$$

And to compute the tensor product for the unitary operations acting on qubits:

$$
\begin{pmatrix}a&b\\c&d\end{pmatrix}\otimes\begin{pmatrix}\alpha&\beta\\\gamma&\delta\end{pmatrix}=\begin{pmatrix}a\begin{pmatrix}\alpha &\beta\\\gamma&\delta\end{pmatrix}&b\begin{pmatrix}\alpha&\beta\\\gamma&\delta\end{pmatrix}\\c\begin{pmatrix}\alpha&\beta\\\gamma&\delta\end{pmatrix}&d\begin{pmatrix}\alpha&\beta\\\gamma&\delta\end{pmatrix}\end{pmatrix}=\begin{pmatrix}a\alpha&a\beta&b\alpha&b\beta\\a\gamma&a\delta&b\gamma&b\delta\\c\alpha&c\beta&d\alpha&d\beta\\c\gamma&c\delta&d\gamma&d\delta\end{pmatrix}
$$

### Multi-Qubit Bases

The multi-qubit computational basis is the set of multi-qubit states encompassing all possible combinations of $`|0\rangle`$ and $`|1\rangle`$. This can get complicated fast in multiple-qubit spaces:

2-qubit case: $`|00\rangle,|01\rangle,|10\rangle,|11\rangle`$ (can also be written in integer form, as if it was binary): $`|10\rangle \rightarrow |2\rangle, |111\rangle \rightarrow|7\rangle`$

### Exercise I.11.1:

The two-qubit computational basis consists of 4 vectors:

$`|0\rangle \otimes|0\rangle, |1\rangle \otimes|0\rangle, |0\rangle \otimes|1\rangle, |1\rangle \otimes|1\rangle`$

These vectors constitute every possible pairing of 2 qubits in the 2 possible single-qubit basis states.

Apply the tensor product to evaluate the 4-dimensional basis vectors. What do you notice about them?

Note that, notationally, $`|0\rangle\otimes|1\rangle\otimes|1\rangle\otimes|0\rangle`$ can be written as $`|0110\rangle`$.

$$
|00\rangle:\begin{pmatrix}1\\0\end{pmatrix}\otimes\begin{pmatrix}1\\0\end{pmatrix}=\begin{pmatrix}1\\0\\0\\0\end{pmatrix}\\
|01\rangle:\begin{pmatrix}1\\0\end{pmatrix}\otimes\begin{pmatrix}0\\1\end{pmatrix}=\begin{pmatrix}0\\1\\0\\0\end{pmatrix}\\
|10\rangle:\begin{pmatrix}0\\1\end{pmatrix}\otimes\begin{pmatrix}1\\0\end{pmatrix}=\begin{pmatrix}0\\0\\1\\0\end{pmatrix}\\
|11\rangle:\begin{pmatrix}0\\1\end{pmatrix}\otimes\begin{pmatrix}0\\1\end{pmatrix}=\begin{pmatrix}0\\0\\0\\1\end{pmatrix}\\
$$

We can notice that the position the sole 1 exists in is the same as the binary representation of the vector.

### Exercise I.11.2

Considering the number and size of the vectors in the 2-qubit computational basis, how many vectors will be in the 3-qubit computational basis (and what size will they be)? Generalize the result to an n-qubit system.

1-qubit computational basis: 2 vectors (2\^1) of size 2 (2\^1)

2-qubit computational basis: 4 vectors (2\^2) of size 4 (2\^2)

3-qubit computational basis: 8  vectors (2\^3) of size 8 (2\^3)

**n-qubit computational basis: 2\^n vectors of size 2\^n**

Note that this indicates why we need to *build* quantum computers instead of just simulating - the size of the space explodes!

### Exercise I.11.3

Use the tensor product to compute the state vector of a two-qubit system where the first qubit is in state $`|+\rangle`$ and the second is in state $`|1\rangle`$.

Express this state as a linear combination of the two-qubit computational basis vectors, and verify that the state is still normalized.

First, let’s consider the vector for each of these:

$$
|+\rangle=\begin{pmatrix}\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}\end{pmatrix}\\
|1\rangle=\begin{pmatrix}0\\1\end{pmatrix}
$$

Next, compute:

$$
|+\rangle\otimes|1\rangle=\frac{1}{\sqrt{2}}\begin{pmatrix}1\\1\end{pmatrix}\otimes\begin{pmatrix}0\\1\end{pmatrix}=\frac{1}{\sqrt{2}}\begin{pmatrix}0\\1\\0\\1\end{pmatrix}
$$

Expressed as a linear combination of the two-qubit computational basis vectors, we have:

$$
\frac{1}{\sqrt{2}}(|01\rangle+|11\rangle)
$$

To make sure it’s normalized, we sum the squares of the vector entries:

$`(\frac{1}{\sqrt{2}}*0)^2+(\frac{1}{\sqrt{2}}*1)^2+(\frac{1}{\sqrt{2}}*0)^2+(\frac{1}{\sqrt{2}}*1)^2=1/2+1/2=1 \checkmark`$

---

Note that we can do this much  more simply using the distributive property of the tensor product to obtain the linear combination directly:

$$
|+\rangle\otimes|1\rangle=\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)\otimes|1\rangle=\frac{1}{\sqrt{2}}(|01\rangle+|11\rangle) \checkmark
$$

### Separable Operations

Note that, in a multi-qubit system, multiple single-qubit operations can be performed in parallel. These operations are considered *separable* because they can be expressed as tensor products of individual qubit systems.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

Each “layer” is considered a single multi-qubit operation. In the diagram above, the first step is to apply a Hadamard to each qubit, which corresponds to applying $`H\otimes H\otimes H`$, the second applies $`S \otimes T \otimes T^\dagger`$ and so on. Reading left to right, the matrices are applied to the states from right to left. (eg, applying these first two layers to state $`|\psi\rangle`$ would look like $`(S\otimes T \otimes T^\dagger)(H \otimes H \otimes H)|\psi\rangle`$.

### Exercise I.11.4

Assuming we have two tensor product operations, $`A \otimes B`$ and $`C \otimes D`$, we can apply the multiplication on each side of the tensor product independently $`(A\otimes B) \cdot (C\otimes D)=(AC)\otimes(BD)`$. See if you can understand this creatively.

Since the “top half” and the “bottom half” of the first operator are combined with the “left half” and the “right half” of the second operator, we can combine these visually - A and C both have their top and bottom halves combined with the right and left halves of B and D (there should be no “muddying” there) - so we can combine those attributes right away.

Put graphically, if we have the quantum circuit described in the equation, we can note that the operations are separable and independent; we can conceptually group them together.Note that we have to adjust the notation: C→A becomes AC.

$$
--|C|-|A|--\\
--|D|-|B--\\
\rightarrow\\
--|AC|--\\
--|BD|--\\
\rightarrow\\
(AC)\otimes(BD) \checkmark
$$

### Codercise I.11.1- Preparing Basis State

In PennyLane, qubits are indexed<br>numerically from left to right. Therefore, a state such as $`|10100\rangle`$ indicates that the first and third qubit (or, wires 0 and 2) are in state $`|1\rangle`$ and the second, fourth, and fifth qubit are in state $`|0\rangle`$.

When drawing quantum circuits, our convention is that the leftmost (first) qubit is at the *top *of the circuit.

Write a circuit in PennyLane that accepts an integer value, then prepares and returns the corresponding computational basis state vector $`|n\rangle`$.

<details>
<summary>My Code</summary>

```python
num_wires = 3
dev = qml.device("default.qubit", wires=num_wires)

@qml.qnode(dev)
def make_basis_state(basis_id):
    """Produce the 3-qubit basis state corresponding to |basis_id>.

    Note that the system starts in |000>.

    Args:
        basis_id (int): An integer value identifying the basis state to construct.

    Returns:
        np.array[complex]: The computational basis state |basis_id>.
    """

    ##################
    # YOUR CODE HERE #
    ##################

    myseries = np.binary_repr(basis_id,width=3)

    if myseries[0]=="1":
        qml.PauliX(wires=0)

    if myseries[1]=="1":
        qml.PauliX(wires=1)

    if myseries[2]=="1":
        qml.PauliX(wires=2)

    return qml.state()

basis_id = 3
print(f"Output state = {make_basis_state(basis_id)}")

```

</details>

### Codercise I.11.2- Separable Operations:

Use PennyLane to create the state $`|+1\rangle=|+\rangle\otimes|1\rangle`$. Then, return two measurements:

- The expectation value of Y on the first qubit
- The expectation value of Z on the second qubit
Note that, in PennyLane, you can return measurements of multiple observables as a tuple, as long as they don’t share wires.

<details>
<summary>My Code</summary>

```python
# Creates a device with *two* qubits
dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def two_qubit_circuit():
    ##################
    # YOUR CODE HERE #
    ##################
    
    # |+> is |0> w/ hadamard operation
    qml.Hadamard(wires=0)

    # |1> is |0> w/ the paulix gate
    qml.PauliX(wires=1)
    

    # RETURN TWO EXPECTATION VALUES, Y ON FIRST QUBIT, Z ON SECOND QUBIT

    return (qml.expval(qml.PauliY(wires=0)),qml.expval(qml.PauliZ(wires=1)))

```

</details>

### Codercise I.11.3- Expectation Value of 2-Qubit Observable

Now, write a PennyLane circuit that creates the state $`|1-\rangle=|1\rangle\otimes|-\rangle`$. Then, measure the expectation value of the *two-qubit observable *$`Z\otimes X`$*. *

In PennyLane, you can combine observables using the @ symbol to represent the tensor product (eg qml.PauliZ(0)@qml.PauliZ(1))

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def create_one_minus():
    ##################
    # YOUR CODE HERE #
    ##################

    # PREPARE |1>|->

    # Prepare |1> (just PauliX on state |0>)
    qml.PauliX(wires=0)

    # Prepare |-> (Pauli X, then Hadamard on state |0>)
    qml.PauliX(wires=1)
    qml.Hadamard(wires=1)

    # RETURN A SINGLE EXPECTATION VALUE Z \otimes X

    return qml.expval(qml.PauliZ(wires=0)@qml.PauliX(wires=1))

print(create_one_minus())

```

</details>

### Codercise I.11.4- Double Trouble

Implement the following circuit twice.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

For one version, measure the observables Z on the first qubit (eg $`Z \otimes I`$ on the first qubit and $`I \otimes Z`$ on the second qubit)

For the second version, measure the observable $`Z\otimes Z`$.

See how they compare? Plot results as a function of $`\theta`$

Note that, in pennylane, you don’t need to specify the identity portion of observables; $`I \otimes Z`$ is simply qml.PauliZ(1) not qml.Identity(0)@qml.PauliZ(1)

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def circuit_1(theta):
    """Implement the circuit and measure Z I and I Z.

    Args:
        theta (float): a rotation angle.

    Returns:
        float, float: The expectation values of the observables Z I, and I Z
    """
    qml.RX(theta,wires=0)
    qml.RY(theta*2,wires=1)

    return (qml.expval(qml.PauliZ(wires=0)),qml.expval(qml.PauliZ(wires=1)))

@qml.qnode(dev)
def circuit_2(theta):
    """Implement the circuit and measure Z Z.

    Args:
        theta (float): a rotation angle.

    Returns:
        float: The expectation value of the observable Z Z
    """
    qml.RX(theta,wires=0)
    qml.RX(theta*2,wires=1)

    return qml.expval(qml.PauliZ(wires=0)@qml.PauliZ(wires=1))

def zi_iz_combination(ZI_results, IZ_results):
    """Implement a function that acts on the ZI and IZ results to
    produce the ZZ results. How do you think they should combine?

    Args:
        ZI_results (np.array[float]): Results from the expectation value of
            ZI in circuit_1.
        IZ_results (np.array[float]): Results from the expectation value of
            IZ in circuit_2.

    Returns:
        np.array[float]: A combination of ZI_results and IZ_results that
        produces results equivalent to measuring ZZ.
    """

    combined_results = np.zeros(len(ZI_results))

    combined=[]

    # Note - ZZ seems to be a measure of how in-sync the two signals (ZI and IZ) are - can we use that?
    # Should be 1- the difference in some way or other
    # Note that if either ZI or IZ is 0, ZIxIZ will be 0
    # If both ZI and IZ are 1, ZIxIZ is 1
    # If ZI = -IZ, then ZIxIZ is -1
    # We're effectively multiplying the two? (1x1=1, 0xX=0, -1x1=-1, -1x-1=1)
    for i in range(0,len(ZI_results)):
        combined.append(ZI_results[i]*IZ_results[i])

    return np.array(combined)

theta = np.linspace(0, 2 * np.pi, 100)

# Run circuit 1, and process the results
circuit_1_results = np.array([circuit_1(t) for t in theta])

ZI_results = circuit_1_results[:, 0]
IZ_results = circuit_1_results[:, 1]
combined_results = zi_iz_combination(ZI_results, IZ_results)

# Run circuit 2
ZZ_results = np.array([circuit_2(t) for t in theta])

# Plot your results
plot = plotter(theta, ZI_results, IZ_results, ZZ_results, combined_results)

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
