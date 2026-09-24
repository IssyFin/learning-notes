# We've Got It Under Control

_Quantum Computing › Circuits with Many Qubits_

[Course link](https://pennylane.ai/codebook/circuits-with-many-qubits/weve-got-it-under-control)

<!-- notion-import -->
## Notes from Notion

_Copied from **We’ve got it under control (Expand repertoire w/ controlled gates)** in [💠 MQ (Many Qubits)](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)._

### Controlled Z (Controlled Phase)

Like the controlled X gate, we can apply other gates to a target bit based on the state of a control bit. For example, the controlled-Z gate has a representation of:

$$
CZ=\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&1&0\\0&0&0&-1\end{pmatrix}
$$

Note that the CZ gate is fairly commonly used and therefore has multiple circuit diagrams:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

#### Exercise I.13.I

As we did for the CNOT gate, compute the action of the controlled-Z on the computational basis states. Do this for two cases: first qubit control/second qubit target, and second qubit control/first qubit target. What do we notice about those two cases?

Case 1: First bit control

$$
|00\rangle\rightarrow|00\rangle\\
|01\rangle\rightarrow|01\rangle\\
|10\rangle\rightarrow|1(-0)\rangle\rightarrow|10\rangle\\
|11\rangle\rightarrow|1(-1)\rangle\rightarrow-|11\rangle\\
$$

Case 2: Second bit control

$$
|00\rangle\rightarrow|00\rangle\\
|01\rangle\rightarrow|(-0)1\rangle\\
|10\rangle\rightarrow|10\rangle\rightarrow|10\rangle\\
|11\rangle\rightarrow|(-1)1\rangle\rightarrow-|11\rangle\\
$$

We can tell that, no matter what state is the control bit, we get the same output. The CZ gate is symmetric.

#### Exercise I.13.2

Earlier, we worked out that Z=HXH. Using this knowledge, can you express CZ in terms of H and CNOT?

Thinking about it visually, a CZ gate is *essentially* a 180 phase shift, if both states are 1. H creates a uniform superposition, and X is a not gate. Since H “doesn’t care” about the control bit, we can effectively just include the CNOT gate instead of the X gate to produce a CZ instead of a Z. Note that H doesn’t rely on a control bit - we can apply H just to the target bit.

$$
Z=HXH\\
CZ=HCXH
$$

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

### The SWAP Gate

The SWAP gate simply exchanges the states of two qubits and is denoted by a set of Xs:

$$
SWAP(|\psi\rangle \otimes |\gamma\rangle)=|\gamma\rangle \otimes |\psi\rangle
$$

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

#### Exercise I.13.3:

Denote the matrix form of the SWAP gate by considering how it acts on the computational basis states.

Intuitively, we should expect to see what is essentially a reversal of the identity matrix. However, \|00\> and \|11\> are unchanged while \|01\> and \|10\> are exchanged. Therefore, we see the Identity matrix for those “outer” cases and the reverse identity for the “inner” cases.

$$
SWAP(|\psi\rangle \otimes |\gamma\rangle)=\begin{pmatrix}1&0&0&0\\0&0&1&0\\0&1&0&0\\0&0&0&1\end{pmatrix}
$$

Note that a SWAP can be implemented via 3 CNOT gates. We also note that since SWAP is symmetric, we can change the direction of all the CNOTS without affecting the outcome.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

### The Toffoli Gate (reversible AND/Controlled-Controlled-NOT)

The Toffoli gate is the most common gate using more than 2 qubits, and is considered a universal gate in the realm of classical reversible computing. (note- reversible computing means operations can be run both forwards and backwards. quantum computing is inherently reversible since the operations are unitary)

The Toffoli gate is, essentially, a reversible AND. It keeps the two bits that are AND’d intact and adds the results modulo 2 to a third bit, c.

$$
TOF(abc)=ab(c\oplus ab)\\
TOF|abc\rangle=|ab(c\oplus ab)\rangle
$$

The AND operation only produces a non-zero result when the first two bits (a and b) are 1; therefore, when a and b are 1, we add 1 bit to c, which is equivalent to performing a NOT gate (flipping the bit), and is therefore called a controlled-controlled-NOT.

$$
TOF=\begin{pmatrix}
1&0&0&0&0&0&0&0\\
0&1&0&0&0&0&0&0\\
0&0&1&0&0&0&0&0\\
0&0&0&1&0&0&0&0\\
0&0&0&0&1&0&0&0\\
0&0&0&0&0&1&0&0\\
0&0&0&0&0&0&0&1\\
0&0&0&0&0&0&1&0\\
\end{pmatrix}
$$

#### Exercise I.13.4-

Determine the action of the Toffoli gate on the 3-qubit computational basis states. Look closely at the structure of the qubit states; can you find a mathematical relationship between the first two control bits and the target bit after the operation?

$$
|000\rangle\rightarrow|000\rangle\\
|001\rangle\rightarrow|001\rangle\\
|010\rangle\rightarrow|010\rangle\\
|011\rangle\rightarrow|011\rangle\\
|100\rangle\rightarrow|100\rangle\\
|101\rangle\rightarrow|101\rangle\\
|110\rangle\rightarrow|111\rangle\\
|111\rangle\rightarrow|110\rangle\\

$$

Note that $`\{CNOT, H,T\}`$ is a universal gate set. Therefore, gates like the Toffoli, which process more than 2 qubits, can be broken down into 1- and 2- qubit gates. There are often multiple ways to do so. The following two circuits are both decompositions of the Toffoli. Both require the same number of T gates, but the T gate depth is different, and there are a different number of CNOT gates. If an implementation is difficult, you’d typically want to use the circuit with fewer CNOT gates. However, in a noisy application (which means qubits have a lower coherence time), you’d want to use a lower-depth circuit.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

#### Exercise I.13.5-

Another common 3-qubit operation is the controlled-controlled-Z $`(CCZ)`$. Using what you know about controlled operations,  Z, and the Toffoli gate, express CCZ as a series of 1- and 2- qubit operations.

We can recall that CZ = HCNOT. And we can extrapolate this further. We can take the Toffoli and add an H to either side. Since H is the inverse of itself, the H at the start and end of the circuit cancel each other out.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

#### Codercise I.13.1- The Imposter CZ

Earlier, we learned how to create a Z gate with H and X. A similar circuit can be constructed for CZ using controlled-X (CNOT) and H.

In PennyLane, controlled-Z is available as [qml.CZ](http://qml.CZ) and can be called in the same way as qml.CNOT.

Complete the function imposter_cz to reveal the relationship.

```python
dev = qml.device("default.qubit", wires=2)

# Prepare a two-qubit state; change up the angles if you like
phi, theta, omega = 1.2, 2.3, 3.4

@qml.qnode(device=dev)
def true_cz(phi, theta, omega):
    prepare_states(phi, theta, omega)

    ##################
    # YOUR CODE HERE #
    ##################

    qml.CZ(wires=[0,1])
    # IMPLEMENT THE REGULAR CZ GATE HERE

    return qml.state()

@qml.qnode(dev)
def imposter_cz(phi, theta, omega):
    prepare_states(phi, theta, omega)

    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT CZ USING ONLY H AND CNOT
    qml.Hadamard(wires=1)
    qml.CNOT(wires=[0,1])
    qml.Hadamard(wires=1)
    return qml.state()

print(f"True CZ output state {true_cz(phi, theta, omega)}")
print(f"Imposter CZ output state {imposter_cz(phi, theta, omega)}")

```

#### Codercise I.13.2- The SWAP Gate

The SWAP Gate (qml.SWAP) exchanges the state of two qubits. The swap gate can be implemented via two CNOT gates. In the code below, try to find the sequence of CNOT gates to match the output state produced by a swap.

```python
dev = qml.device("default.qubit", wires=2)

# Prepare a two-qubit state; change up the angles if you like
phi, theta, omega = 1.2, 2.3, 3.4

@qml.qnode(dev)
def apply_swap(phi, theta, omega):
    prepare_states(phi, theta, omega)

    ##################
    # YOUR CODE HERE #
    ##################
    # IMPLEMENT THE REGULAR SWAP GATE HERE
    qml.SWAP(wires=[0,1])

    return qml.state()

@qml.qnode(dev)
def apply_swap_with_cnots(phi, theta, omega):
    prepare_states(phi, theta, omega)

    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT THE SWAP GATE USING A SEQUENCE OF CNOTS
    qml.CNOT(wires=[0,1])
    qml.CNOT(wires=[1,0])
    qml.CNOT(wires=[0,1])

    return qml.state()

print(f"Regular SWAP state = {apply_swap(phi, theta, omega)}")
print(f"CNOT SWAP state = {apply_swap_with_cnots(phi, theta, omega)}")

```

#### Codercise I.13.3- The Toffoli Gate

In PennyLane, the Toffoli gate is applied using the following syntax: qml.Toffoli(wires=\[control1, control2, target\]). This is similar to 2-qubit gates, but all 3 wires need to be specified in the correct order.

Use the Toffoli gate to construct a controlled SWAP operation.

```python
dev = qml.device("default.qubit", wires=3)

# Prepare first qubit in |1>, and arbitrary states on the second two qubits
phi, theta, omega = 1.2, 2.3, 3.4

# A helper function just so you can visualize the initial state
# before the controlled SWAP occurs.
@qml.qnode(dev)
def no_swap(phi, theta, omega):
    prepare_states(phi, theta, omega)
    return qml.state()

@qml.qnode(dev)
def controlled_swap(phi, theta, omega):
    prepare_states(phi, theta, omega)

    ##################
    # YOUR CODE HERE #
    ##################

    # PERFORM A CONTROLLED SWAP USING A SEQUENCE OF TOFFOLIS

    # A regular swap can be executed with three CNOTs. We can control it via 3 Toffolis (controlled-controlled-nots):
    qml.Toffoli(wires=[0,1,2])
    qml.Toffoli(wires=[0,2,1])
    qml.Toffoli(wires=[0,1,2])
    # qml.CNOT(wires=[0,1])
    
    return qml.state()

print(no_swap(phi, theta, omega))
print(controlled_swap(phi, theta, omega))

```

#### Codercise I.13.4- Mixed Control Gates

The idea of a controlled-controlled-NOT generalizes to an arbitrary number of controls. Furthermore, there are many applications in quantum computing where the "polarities" of the control gates are mixed, i.e., on some qubits you may want to control on a qubit being in the state \|0\> rather than \|1\>.

<br>In PennyLane, mixed-polarity multi-controlled Toffoli gates can be easily implemented using the [`MultiControlledX`](https://pennylane.readthedocs.io/en/latest/code/api/pennylane.MultiControlledX.html)` `operation. With this gate, a list `wires` containing the control wires followed by a single target wire, and a list of control bits, `control_values`, are specified as input arguments, like the example below:<br><br>` qml.MultiControlledX(wires=[0, 1, 2, 3], control_values=[1, 0, 1])`

Write a 4-qubit PennyLane circuit that applies a Hadamard to the control qubits, then applies a MultiControlledX on the fourth<br>qubit, controlled on the first 3 qubits being in the state. This is depicted in the circuit below: "control on 0" is denoted by<br>an open circle on the control qubits, rather than a filled circle. What do you expect will happen to the target qubit?

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

```python
dev = qml.device("default.qubit", wires=4)

@qml.qnode(dev)
def four_qubit_mcx():
    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT THE CIRCUIT ABOVE USING A 4-QUBIT MULTI-CONTROLLED X
    qml.Hadamard(wires=0)
    qml.Hadamard(wires=1)
    qml.Hadamard(wires=2)
    qml.MultiControlledX(wires=[0,1,2,3],control_values=[0,0,1])
    return qml.state()

print(four_qubit_mcx())

```

#### Codercise I.13.5- The 3 Controlled Not

This circuit performs something interesting. The set of three Hadamards serves to put the first three qubits in a linear superposition of all 3-qubit basis states:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

Together with the fourth qubit, the state before the Multi-controlled gate is

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

The multi-controlled gate only triggers in the case where the first three qubits are in the state \|001\>. Therefore, the state of the fourth qubit<br>will only be flipped for that particular term in the superposition:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

Since you can apply any gate in a multi-controlled fashion like this, this is a nice trick for applying operations to only certain parts of a superposition. Furthermore, different controlled operations can be applied to different terms simply by tinkering with the control values.

Consider the -controlled-NOT below. Can you implement this gate using only Toffolis? You'll need one extra qubit to do so; this is<br>called an *auxiliary* qubit, and note that it both starts and ends in the state \|0\>

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

```python
# Wires 0, 1, 2 are the control qubits
# Wire 3 is the auxiliary qubit
# Wire 4 is the target
dev = qml.device("default.qubit", wires=5)

@qml.qnode(dev)
def four_qubit_mcx_only_tofs():
    # We will initialize the control qubits in state |1> so you can see
    # how the output state gets changed.
    qml.PauliX(wires=0)
    qml.PauliX(wires=1)
    qml.PauliX(wires=2)

    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT A 3-CONTROLLED NOT WITH TOFFOLIS
    # Note that we need to use the auxiliary qubit, and we only need 3 toffolis

    qml.Toffoli(wires=[0,1,3])
    qml.Toffoli(wires=[2,3,4])
    qml.Toffoli(wires=[0,1,3])

    return qml.state()

# print(four_qubit_mcx_only_tofs())

```
*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

---


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
