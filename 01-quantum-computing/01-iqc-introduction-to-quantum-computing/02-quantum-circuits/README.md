# Quantum Circuits

_Quantum Computing › Introduction to Quantum Computing_

[Course link](https://pennylane.ai/codebook/introduction-to-quantum-computing/quantum-circuits)

<!-- notion-import -->
## Notes from Notion

_Copied from **Quantum Circuits** in [🔆 IQC (introduction to quantum computing)](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)._

### Visualizing Quantum Algorithms

We represent sequences of operation via ***quantum circuits***.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

#### Wires and Registers

Circuits start with a set of ***wires*** representing a set of qubits (one wire per qubit). Each qubit is initialized, typically in state \|0⟩, but not always. Initial states should always be indicated, though if they aren’t, it is typically safe to assume it is \|0⟩.

\*Note: A set of qubits is called a ***quantum register***

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

#### Gates and Operations

Similarly to digital circuits, operations on qubits are often called gates, which may affect one or more qubits.

Circuits are read left to right, with gates affecting the qubits at its left side, producing the output on its right side. Operations acting on separate qubits can be applied in *parallel, *so long as the order of operations and dependencies are maintained*.*

*[Image in Notion: Note that these two quantum circuits are equivalent; the pentagon on qubit 0 can be “pushed” to the left and applied at the same time as the rectangle on qubits 1 and 2](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

#### Circuit Depth

Though we can track number of gates and types of gates, the truly important metric is ***circuit depth***, or the number of time steps it takes for a circuit to run, if operations are done in parallel. Essentially, it is the number of layers in a circuit.

*[Image in Notion: This example qubit circuit, modeled as lego bricks (one gate per block) has a depth of 6.](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

#### Measurements

Following the gates and qubit manipulation, the output must be measured. A measurement is depicted in a circuit as a box with a dial, as shown below. Note that measurement is not counted in the calculation of depth.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

#### Exercise:

Draw the circuit diagram for a 4-qubit circuit from the following set of instructions, then identify the depth of the circuit:

- Initialize all the qubits in \|0⟩
- Apply a circle operation to qubit 0
- Apply a circle operation to qubit 2
- Apply a triangle operation to qubit 2
- Apply a triangle operation to qubit 3
- Apply a rectangle operation between qubits 0 and 1
- Apply a rectangle operation between qubits 1 and 2
- Apply a rectangle operation between qubits 2 and 3
- Measure all the qubits

---

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

Depth is 4

### Quantum Circuits in Pennylane

In Pennylane, quantum circuits are represented by quantum functions (regular Python functions with special properties). These functions must apply at least one quantum operation and return at least one quantum measurement.

\*Note that qubits (wires) are ordered numerically starting from 0)

To write a quantum function with input parameters, we create a gate and specify which wire(s) to use, then return the measurement of the wires. We can also include parameters.

#### Example:

Represent this quantum circuit in a quantum function:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

Note that this circuit includes 2 qubits and relies on parameter theta.

---

```python
def my_first_circuit(theta):
	qml.Hadamard(wires = 0)
	qml.CNOT(wires = [0,1])
	qml.RZ(theta, wires = 0)
	
	return qml.probs(wires=[0,1])
```

#### Example:

Execute the quantum function you just made in PennyLane. This requires 2 extra parts:

- A device to run the circuit on
  - Typically, we’ll use ‘default.qubit’
  - Wires/qubits can be given string labels
- a QNode, which binds the circuit to the device and executes it.
For Pennylane exercises, we will run with quantum simulators, but PennyLane provides plugins to run on real quantum hardware as well.

---

Step 1: Define the device to run the circuit on

(follow the template* dev = qml.device('device.name',wires=num_qubits)*)

```python
dev = qml.device('default.qubit',wires=["wire_a","wire_b")
```
Step 2: Construct a QNode (the main unit of quantum computation in Pennylane). There are 2 methods to do this:

- Use the qml.QNode function
  - Example:

```python
my_qnode = qml.QNode(my_circuit,my_device)
```

```python
dev = qml.device('default.qubit,wires=[0,1])

dev my_first_circuit(theta):
	qml.Hadamard(wires=0)
	qml.CNOT(wires=[0,1])
	qml.RZ(theta,wires=0)

return qml.probs(wires=[0,1])

my_first_QNode = qml.QNode(my_first_circuit,dev)
```
- The second is to use a decorator; using @qml.qnode(dev) will automatically produce a QNode with the same name as the function that can be run on the device dev.

```python
dev = qml.device('default.qubit,wires=[0,1])

@qml.qnode(dev)
def my_first_circuit(theta):
	qml.Hadamard(wires=0)
	qml.CNOT(wires=[0,1])
	qml.RZ(theta,wires=0)

return qml.probs(wires=[0,1])
```

Note that this circuit includes 2 qubits and relies on parameter theta.

---

```python
def my_first_circuit(theta):
	qml.Hadamard(wires = 0)
	qml.CNOT(wires = [0,1])
	qml.RZ(theta, wires = 0)
	
	return qml.probs(wires=[0,1])
```

### Codercises

#### Codercise I.2.1-Order of Operations

Rearrange the lines of the function to match the order of operations in the circuit:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

<details>
<summary>My Code</summary>

```python
def my_circuit(theta, phi):
    ##################
    # YOUR CODE HERE #
    ##################

    # REORDER THESE 5 GATES TO MATCH THE CIRCUIT IN THE PICTURE

    qml.CNOT(wires=[0, 1])
    qml.RX(theta, wires=2)
    qml.Hadamard(wires=0)
    qml.CNOT(wires=[2, 0])
    qml.RY(phi, wires=1)

    # This is the measurement; we return the probabilities of all possible output states
    # You'll learn more about what types of measurements are available in a later node
    return qml.probs(wires=[0, 1, 2])

```

</details>

#### Codercise I.2.2-Building a QNode

Build a QNode function based on the circuit below.

<details>
<summary>Full exercise</summary>

Recall that one way in which we can turn our quantum circuits into QNodes is via the `qml.QNode` function:

```python
    my_qnode = qml.QNode(my_circuit, my_device)
```
Once a QNode is created, it can be called like<br> a function using the same parameters as the quantum function upon which<br> it's built.

Complete the quantum function in the PennyLane code below<br>to implement the following quantum circuit. Then, construct a QNode using `qml.QNode` and<br>run the circuit on the provided device.

</details>

*[Image in Notion: image](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)*

<details>
<summary>My Code</summary>

```python
# This creates a device with three wires on which PennyLane can run computations
dev = qml.device("default.qubit", wires=3)

def my_circuit(theta, phi, omega):

    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT THE CIRCUIT BY ADDING THE GATES
    qml.RX(theta,wires=0)
    qml.RY(phi,wires=1)
    qml.RZ(omega,wires=2)
    qml.CNOT(wires=[0,1])
    qml.CNOT(wires=[1,2])
    qml.CNOT(wires=[2,0])
    

    # Here are two examples, so you can see the format:
    # qml.CNOT(wires=[0, 1])
    # qml.RX(theta, wires=0)

    return qml.probs(wires=[0, 1, 2])

# This creates a QNode, binding the function and device
my_qnode = qml.QNode(my_circuit, dev)

# We set up some values for the input parameters
theta, phi, omega = 0.1, 0.2, 0.3

# Now we can execute the QNode by calling it like we would a regular function
my_qnode(theta, phi, omega)

```

</details>

#### Codercise I.2.3-The QNode Decorator

Apply a decorator to the quantum function to construct a QNode

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=3)

##################
# YOUR CODE HERE #
##################

# DECORATE THE FUNCTION BELOW TO TURN IT INTO A QNODE

@qml.qnode(dev)
def my_circuit(theta, phi, omega):
    qml.RX(theta, wires=0)
    qml.RY(phi, wires=1)
    qml.RZ(omega, wires=2)
    qml.CNOT(wires=[0, 1])
    qml.CNOT(wires=[1, 2])
    qml.CNOT(wires=[2, 0])
    return qml.probs(wires=[0, 1, 2])

theta, phi, omega = 0.1, 0.2, 0.3

##################
# YOUR CODE HERE #
##################

# RUN THE QNODE WITH THE PROVIDED PARAMETERS

```

</details>

#### Codercise I.2.4-Circuit Depth

What is the depth of the circuit in this exercise?

My answer: 4

---


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
