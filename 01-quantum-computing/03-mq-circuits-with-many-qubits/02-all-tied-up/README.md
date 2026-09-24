# All Tied Up

_Quantum Computing › Circuits with Many Qubits_

[Course link](https://pennylane.ai/codebook/circuits-with-many-qubits/all-tied-up)

<!-- notion-import -->
## Notes from Notion

_Copied from **All tied up (Quantum entanglement)** in [💠 MQ (Many Qubits)](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)._

### Mathematics of Entangled States

So far, we’ve looked at separable multi-qubit operations: every qubit gets acted on by single-qubit gates, with no interactions between them. However, quantum systems allow for entanglement, which is used as a resource in many quantum algorithms.

#### Exercise I.12.1

**A: Suppose we have two single-qubit states**

$`|\psi_1\rangle=\alpha|0\rangle+\beta|1\rangle, |\psi_2\rangle=\gamma|0\rangle+\delta|1\rangle`$

Take the tensor product of these two states to express $`|\psi_1\rangle\otimes |\psi_2\rangle`$ as a linear combination of two-qubit computational basis states. Use the distributive property of the tensor product rather than working with explicit vectors.

$$
|\psi_1\rangle\otimes|\psi_2\rangle\\
(\alpha|0\rangle+\beta|1\rangle)\otimes(\gamma|0\rangle+\delta|1\rangle)\\
\alpha\gamma|00\rangle+\beta\delta|11\rangle+\alpha\delta|01\rangle+\beta\gamma|10\rangle
$$

**B: Consider the state**

$`|\psi\rangle=\frac{1}{\sqrt{2}}(|00\rangle+|11\rangle)`$

Using the results of the previous exercise, show that you cannot find two single-qubit states that tensor together to produce this state.

Since $`|01\rangle`$ and $`|10\rangle`$ are not present, we must assume that $`\alpha\delta`$ and $`\beta\gamma`$ are 0. For *that* to be true, either $`\alpha`$ or $`\delta`$, and either $`\beta`$ or $`\gamma`$ must be 0.

*However*, since $`|00\rangle`$ and $`|11\rangle`$ are nonzero, we see that we can’t have any combination of $`\alpha,\beta,\delta,\gamma`$ values that would produce a 0-value $`|01\rangle`$ and $`|10\rangle`$ while producing a non-zero $`|00\rangle`$ and $`|11\rangle`$

**Since we can’t express this state as the tensor product of two single-qubit states, these qubits can be considered entangled! **$`\checkmark`$

A state is considered **entangled** if it cannot be expressed as a tensor product of individual qubit states (in which case, it would be separable). An entangled state can only be described by using the full state.

Note that entanglement generalizes to larger systems as well, consisting of more than two qubits. For example, the Greenberg-Horne-Zeilinger (GHZ) state, $`|GHZ\rangle=\frac{1}{\sqrt{2}}(|000\rangle+|111\rangle)`$ is fully entangled (none of the qubits can be written independently of the others), while the state $`\frac{1}{2}(|000\rangle+|100\rangle+|011\rangle+|111\rangle)=\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)\otimes\frac{1}{\sqrt{2}}(|00\rangle+|11\rangle)`$ have some entangled qubits (second and third) while others (the first) are not (this is considered to be a ***bipartite*** state, since it can be split up into 2 subsystems).

### The CNOT Gate

Entangling gates: Operations that involve interactions between qubits. Transforms some separable state into an entangled state. These cannot be written as a tensor product of individual single-qubit gates.

The controlled-NOT (CNOT) gate is a two-qubit gate that performs an operation on one qubit depending on the state of another.

*[Image in Notion: The first qubit, denoted with a solid dot, is the control qubit. This qubit’s state does not change, but its state determines whether the operation is performed. The second qubit is the Target qubit.](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

$`CNOT=\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&0&1\\0&0&1&0\end{pmatrix}`$

#### Exercise I.12.2

Evaluate the action of CNOT on the computational basis states $`|ab\rangle`$, where the first qubit is the control and the second qubit is the target. How do they change? Can you express how the second bit transforms based on the first using a Boolean function of a and b?

$$
|00\rangle\rightarrow|00\rangle\\
|01\rangle\rightarrow|01\rangle\\
|10\rangle\rightarrow|11\rangle\\
|11\rangle\rightarrow|10\rangle\\
CNOT|ab\rangle=|a(b\oplus a)\rangle \checkmark

$$

#### Exercise I.12.3

Suppose that instead of the first qubit being the control and the second the target (which we denote as $`CNOT_{ab}`$), we exchange their roles. Given a state $`|ab\rangle`$, the state $`|b\rangle`$ determines whether an X is applied to the state $`|a\rangle`$. Determine the action of this operation on the computational basis states; use this to recover the matrix representation of a CNOT, $`CNOT_{ba}`$, acting on the qubits backwards.

The matrix will look quite similar to $`CNOT_{ab}`$, which is

$$
CNOT_{ab}=\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&0&1\\0&0&1&0\end{pmatrix}
$$

The state table for $`CNOT_{ba}`$ is

$$
|00\rangle \rightarrow |00\rangle\\
|01\rangle \rightarrow |11\rangle\\
|10\rangle \rightarrow |10\rangle\\
|11\rangle \rightarrow |01\rangle\\
$$

The matrix for $`CNOT_{ba}`$ is

$$
CNOT_{ba}=\begin{pmatrix}1&0&0&0\\0&0&0&1\\0&0&1&0\\0&1&0&0\end{pmatrix}
$$

### Universal Gate Sets

The universal gate sets for single-qubit operations can be expanded to be a universal gate set for multi-qubit operations with the simple addition of the CNOT gate. Popular universal gate sets for multi-qubit computation are $`{CNOT,H,T}`$ and $`CNOT,RY,RZ`$.

Although we could do everything with just CNOT, other controlled operations can be implemented between qubit states. For example, an arbitrary controlled unitary on two qubits es expressed like so:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

With the matrix representation structured as:

$$
CU=\begin{pmatrix}I_2&0\\0&U\end{pmatrix}=\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&U_{11}&U_{12}\\0&0&U_{21}&U_{22}\end{pmatrix}=|0\rangle\langle0|\otimes I+|0\rangle\langle0|\otimes U
$$

where $`I_2`$ is the 2x2 identity matrix and the 0 are 2x2 zero matrices. Note that this structure can be expanded for additional qubits. If the number of qubits is even (eg 3), we can expand the equation for the matrix representation with the Hadamard gate:

$`|0\rangle\langle0| \otimes H \otimes I \otimes H+|1\rangle\langle1| \otimes H \otimes U \otimes H`$

#### Exercise I.12.4

Determine the action of a controlled-Hadamard gate on the two-qubit computational basis. You can write out the matrix, but try also to work it out using only bra-ket notation and your knowledge of how the Hadamard affects the single-qubit computational basis state.

Recap: Hadamard effectively produces a uniform superposition of the two basis states; $`|0\rangle \rarr \frac{1}{\sqrt{2}}(|0\rangle+|1\rangle), |1\rangle \rarr \frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)`$.

A controlled hadamard does the same thing, but with the operation determined by the control qubit and the sign dependent on the target qubit.

Since the hadamard matrix is $`\frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}`$, we can expect that the controlled Hadamard matrix would be

$$
\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&\frac{1}{\sqrt{2}}&\frac{1}{\sqrt{2}}\\0&0&\frac{1}{\sqrt{2}}&-\frac{1}{\sqrt{2}}\end{pmatrix}
$$

Our state table would be:

$$
|00\rangle \rarr |00\rangle\\
|01\rangle \rarr |01\rangle\\
|10\rangle \rarr \frac{1}{\sqrt{2}}(|10\rangle+|11\rangle)\\
|11\rangle \rarr \frac{1}{\sqrt{2}}(|10\rangle-|11\rangle)\\
$$

Note that Hadamard is only applied when the control qubit is 1; and the sign of the Hadamard function relies on the target qubit.

#### Codercise I.12.1-Entangling Operations

In PennyLane, we apply CNOTs using qml.CNOT, with the following syntax:

```python
def circuit():
qml.CNOT(wires=[control,target])
```
Write a circuit that implements a CNOT gate between two qubits and test it out on all four computational basis states. Express the answer in a dictionary taking the form of a truth table.

Truth table:

$$
|00\rangle \rarr |00\rangle\\
|01\rangle \rarr |01\rangle\\
|10\rangle \rarr |11\rangle\\
|11\rangle \rarr |10\rangle\\

$$

<details>
<summary>My Code</summary>

```python
num_wires = 2
dev = qml.device("default.qubit", wires=num_wires)

@qml.qnode(dev)
def apply_cnot(basis_id):
    """Apply a CNOT to |basis_id>.

    Args:
        basis_id (int): An integer value identifying the basis state to construct.

    Returns:
        np.array[complex]: The resulting state after applying CNOT|basis_id>.
    """

    # Prepare the basis state |basis_id>
    bits = [int(x) for x in np.binary_repr(basis_id, width=num_wires)]
    qml.BasisState(bits, wires=[0, 1])

    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY THE CNOT
    qml.CNOT(wires=[0,1])
    
    return qml.state()

##################
# YOUR CODE HERE #
##################

# REPLACE THE BIT STRINGS VALUES BELOW WITH THE CORRECT ONES
cnot_truth_table = {"00": "00", "01": "01", "10": "11", "11": "10"}

# Run your QNode with various inputs to help fill in your truth table
print(apply_cnot(0))
print(apply_cnot(1))
print(apply_cnot(2))
print(apply_cnot(3))
```

</details>

#### Codercise I.12.2-Separate or Entangled?

Implement the following circuit and inspect the output state. Can you argue why this state is entangled?

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

This state is a special kind of state known as a Bell state. It’s used in many quantum algorithms since it possesses the “maximum amount of entanglement”

On first glance, we can already tell this circuit is entangled; whether or not we flip bit 2 depends on how bit 1 is superposited (if 0, we superposit 0 and +1. If 1, we superposit 0 and -1).

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def apply_h_cnot():
    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY THE OPERATIONS IN THE CIRCUIT
    qml.H(wires=0)

    qml.CNOT(wires=[0,1])

    return qml.state()

print(apply_h_cnot())

##################
# YOUR CODE HERE #
##################

# SET THIS AS 'separable' OR 'entangled' BASED ON YOUR OUTCOME
state_status = "entangled"

```

</details>

#### Codercise I.12.3-Controlled Rotations

Note that the CNOT gate produces an entangled Bell state. However, there are other entangling gates, including controlled operations, which generally produce entangled states.

PennyLane contains several common controlled operations - including qml.CRX, qml.CRY, and qml.CRZ. These implement the appropriate rotation depending on the control qubit. For example,

$$
CRX(\theta)|00\rangle=|00\rangle\\
CRX(\theta)|10\rangle=|1\rangle\otimes(cos(\theta/2)|0\rangle-isin(\theta/2)|1\rangle)
$$

In Pennylane, we can apply these similarly to how we applied CH:

```python
def circuit(theta):
	qml.CRX(theta,wires=[control,target])
```
Write a circuit in Pennylane that implements the following sequence of operations, then return the measurement outcome probabilities.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=3)

@qml.qnode(dev)
def controlled_rotations(theta, phi, omega):
    """Implement the circuit above and return measurement outcome probabilities.

    Args:
        theta (float): A rotation angle
        phi (float): A rotation angle
        omega (float): A rotation angle

    Returns:
        np.array[float]: Measurement outcome probabilities of the 3-qubit
        computational basis states.
    """

    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY THE OPERATIONS IN THE CIRCUIT AND RETURN MEASUREMENT PROBABILITIES
    qml.Hadamard(wires=0)
    qml.CRX(theta,wires=[0,1])
    qml.CRY(phi,wires=[1,2])
    qml.CRZ(omega,wires=[2,0])

    
    return qml.probs(wires=[0,1,2])

theta, phi, omega = 0.1, 0.2, 0.3
print(controlled_rotations(theta, phi, omega))

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
