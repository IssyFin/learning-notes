# Multi-Qubit Gate Challenge

_Quantum Computing › Circuits with Many Qubits_

[Course link](https://pennylane.ai/codebook/circuits-with-many-qubits/multi-qubit-challenge)

<!-- notion-import -->
## Notes from Notion

_Copied from **Multi-Qubit gate challenge** in [💠 MQ (Many Qubits)](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)._

### Multi-Qubit Gate Reference

*[Image in Notion: CNOT: qml.CNOT; CZ: qml.CZ; CRZ: qml.CRZ](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

*[Image in Notion: TOF: qml.Toffoli; CCZ: qml.CCZ](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

### Single-Qubit Gate Reference

*[Image in Notion: Note:; Z = HXH, Y=StXS (St=S adjoint); RX: qml.RX, RY: qml.RY, Z: qml.RZ](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

*[Image in Notion: X: qml.X or qml.PauliX; H: qml.Hadamard or qml.H; Y: qml.Y or qml.PauliY; Z: qml.Z or qml.PauliZ; S: qml.S; T: qml.T;](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

### Codercise I.14.1 - The Bell States

Consider again the entangled state that we saw earlier, $`|\psi_+\rangle=\frac{1}{\sqrt{2}}(|00\rangle+|11\rangle)`$.

This state is called a Bell State, and it has 3 siblings:

$$
|\psi_-\rangle=\frac{1}{\sqrt{2}}(|00\rangle-|11\rangle)\\

|\phi_+\rangle=\frac{1}{\sqrt{2}}(|01\rangle+|10\rangle)\\

|\phi_-\rangle=\frac{1}{\sqrt{2}}(|01\rangle-|10\rangle)
$$

Together, these states form the Bell Basis. Write a set of 4 circuits that prepare and return each of the four Bell States.

We know that, in a single-qubit case, H gives us $`H|0\rangle=\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)`$ and $`H|1\rangle=\frac{1}{\sqrt{2}}(|0\rangle-|1\rangle).`$ We’ll use a combination of Hadamard, NOTs, and Controlled NOTs to get our resulting states.

```python
dev = qml.device("default.qubit", wires=2)

# Starting from the state |00>, implement a PennyLane circuit
# to construct each of the Bell basis states.

# To make |11> from |00>: PauliX each bit
# To make |01> from |00>: Pauli X second bit
# To make |10> from |00>: Pauli X first bit

@qml.qnode(dev)
def prepare_psi_plus():
    ##################
    # YOUR CODE HERE #
    ##################

    # PREPARE (1/sqrt(2)) (|00> + |11>)
    # Note, this is a uniform superposition for each of the states. Hadamard both, then use a controlled not to get rid of the swaps?
    # first line gives us a uniform superposition of each; we get rid of the swap bits by removing the parts that are neither 00 nor 11
    
    qml.Hadamard(wires=0)
    qml.CNOT(wires=[0,1])

    return qml.state()

@qml.qnode(dev)
def prepare_psi_minus():
    ##################
    # YOUR CODE HERE #
    ##################

    # PREPARE (1/sqrt(2)) (|00> - |11>)
    #We do the same as before, but we apply a not first so that we get a negative result for |11>
    qml.PauliX(wires=0)
    qml.Hadamard(wires=0)
    qml.CNOT(wires=[0,1])

    return qml.state()

@qml.qnode(dev)
def prepare_phi_plus():
    ##################
    # YOUR CODE HERE #
    ##################

    # PREPARE  (1/sqrt(2)) (|01> + |10>)
    #Again, we can do the same thing as our first state, but we'll use a NOT on one wire first, then undo it after.
    qml.Hadamard(wires=0)
    qml.PauliX(wires=0)
    qml.CNOT(wires=[0,1])
    qml.PauliX(wires=0)

    return qml.state()

@qml.qnode(dev)
def prepare_phi_minus():
    ##################
    # YOUR CODE HERE #
    ##################

    # PREPARE  (1/sqrt(2)) (|01> - |10>)
    #And, we'll combine the logic from our previous examples for one last iteration
    qml.PauliX(wires=0)
    qml.Hadamard(wires=0)
    qml.PauliX(wires=0)
    qml.CNOT(wires=[0,1])
    qml.PauliX(wires=0)

    return qml.state()

psi_plus = prepare_psi_plus()
psi_minus = prepare_psi_minus()
phi_plus = prepare_phi_plus()
phi_minus = prepare_phi_minus()

# Uncomment to print results
print(f"|ψ_+> = {psi_plus}")
print(f"|ψ_-> = {psi_minus}")
print(f"|ϕ_+> = {phi_plus}")
print(f"|ϕ_-> = {phi_minus}")

```

### Codercise I.14.2 - Quantum Multiplexer

Implement a 3-qubit circuit in PennyLane that can perform the following:

- If the first two qubits are both \|0\>, do nothing
- If the first qubit is \|0\> and the second is \|1\>, apply PauliX to the third qubit
- If the first qubit is \|1\> and the second is \|0\>, apply PauliZ to the third qubit
- If the first two qubits are both \|1\>, apply a PauliY to the third qubit.
The circuit must produce the exact state that would be obtained by applying<br>these operations, i.e., not just up to a global phase.

There is no need to use any `if` statements in your part of the quantum<br>function; it can all be implemented using quantum operations alone!

*Tip*. This type of operation is called a **quantum multiplexer**. When all 2\^n<br> possible cases of n control qubits are implemented, and the target<br>operation is a single-qubit rotation, it is called a **uniformly controlled<br>rotation**.

```python
dev = qml.device("default.qubit", wires=3)

# State of first 2 qubits
state = [0, 1]

@qml.qnode(device=dev)
def apply_control_sequence(state):
    # Set up initial state of the first two qubits
    if state[0] == 1:
        qml.PauliX(wires=0)
    if state[1] == 1:
        qml.PauliX(wires=1)

    # Set up initial state of the third qubit - use |->
    # so we can see the effect on the output
    qml.PauliX(wires=2)
    qml.Hadamard(wires=2)

    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT THE MULTIPLEXER 
    # 
    
    # IF STATE OF FIRST TWO QUBITS IS 01, APPLY X TO THIRD QUBIT
    # So apply pauliX to wire 0, then use the Toffoli gate
    qml.PauliX(wires=0)
    qml.Toffoli(wires=[0,1,2])
    
    # IF STATE OF FIRST TWO QUBITS IS 10, APPLY Z TO THIRD QUBIT
    # Z=HXH
    #first, undo the previous pauliX
    qml.PauliX(wires=0)
    # then flip the wire1 bit so we can exercise on 10
    qml.PauliX(wires=1)
    qml.Hadamard(wires=2)
    qml.Toffoli(wires=[0,1,2])
    qml.Hadamard(wires=2)

    # IF STATE OF FIRST TWO QUBITS IS 11, APPLY Y TO THIRD QUBIT
    # Y=iXZ=iXHXH
    #flip wire1  back so we can exercise on 11
    qml.PauliX(wires=1)
    #Similar process for Y: Y=StXS
    qml.adjoint(qml.S(wires=2))
    qml.Toffoli(wires=[0,1,2])
    qml.S(wires=2)

    return qml.state()

print(apply_control_sequence(state))

```


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
