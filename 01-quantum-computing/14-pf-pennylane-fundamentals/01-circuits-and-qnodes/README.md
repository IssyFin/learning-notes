# Circuits and QNodes

_Quantum Computing › PennyLane Fundamentals_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)

<!-- notion-import -->
## Notes from Notion

_Copied from **Circuits and QNodes** in [🐣 PF (Pennylane fundamentals)](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)._

#### Summary

5 steps for getting an output state of a quantum computation:

1. Define device

```python
dev=qml.device("default.qubit",wires=2)
```
2. Write the circuit as a quantum circuit including all of the necessary gates, in order of application

```python
def quantum_circuit(angle):
    qml.RX(angle, wires = 0)
    qml.PauliY(wires = 1)
```
3. Return the quantum state at the end of the function

```python
def quantum_circuit(angle):
    qml.RX(angle, wires = 0)
    qml.PauliY(wires = 1)
    
    qml.state()
```
4. Link the device to the circuit via a qnode

```python
@qml.qnode(dev)
def quantum_circuit(angle):
    qml.RX(angle, wires = 0)
    qml.PauliY(wires = 1)

    qml.state()
```
5. Link everything in the same codeblock

```python
dev = qml.device("default.qubit", wires = 2)

@qml.qnode(dev)
def quantum_circuit(angle):
    qml.RX(angle, wires = 0)
    qml.PauliY(wires = 1)

    qml.state()
```

### PennyLane Overview

PennyLane is an open-source python library supporting quantum simulations. It can be used to run algorithms both locally in CPUs, or externally in GPUs, quantum simulators, and quantum devices. Because PennyLane is open-source and community-based, anyone can contribute to its growth!

However, quantum computing does require some technical knowledge prerequisites, and the PennyLane course presumes a background in the following:

- Linear Algebra, Complex Numbers, and Multivariable Calculus
- Elemental Python programming language (variables, lists, conditional statements, loops, numpy, functions)
- Quantum state representations as vectors in a Hilbert Space
- Unitary Operators (including common gates like Hadamard, Pauli Operators, Rotations, Controlled NOTs, concept of controlled gates)
- Quantum measurements (observables, Born’s rule for probabilities, expectation values)
- Representation of quantum state preparations, operations, and measurements as quantum circuits

#### Installing Pennylane

PennyLane can be installed via pip:

```python
pip install pennylane
```
And can be included in a python IDE via the following:

```python
import pennylane as qml
from pennylane import numpy as np
```

### Quantum Functions

A quantum function, or quantum circuit, is a Python function that includes a list of gates which may depend on parameters and act on some wires (qubits). In PennyLane, all wires/qubits are initialized as $`|0\rangle`$ by default, and wires are numbered from 0 on. Functions in PennyLane generally return a state, called via “qml.state()”.

Note that some quantum functions don’t return anything; these can be used as quantum subcircuits, or elements of a larger circuit:

```python
def subcircuit_1(angle):

    qml.RX(angle, wires = 0)
    qml.PauliY(wires = 1)
    
def subcircuit_2():

    qml.Hadamard(wires = 0)
    qml.CNOT(wires = [0,1])
    
def full_circuit(theta, phi):

    subcircuit_1(theta)
    subcircuit_2()
    subcircuit_1(phi)
```
To instantiate a quantum function in PennyLane, we need to define the function, along with its parameters and constituent gates, initialize a qnode, and apply it to a device.

#### Devices

Common devices include:

  - default.qubit (for circuits without noise. Not optimized for performance, but will generally be the first device to receive updates through PennyLane)
  - lightning.qubit (fast and noiseless, optimized for c++ backend, but won’t be updated as quickly as the default.qubit device)
  - default.mixed (allows noisy gates, and uses the density operator representation of quantum states)
Generally, we work with default.qubit, but may work with lightning.qubit for larger circuits. To define the device, we use qml.device() and specify which device and the number of wires. Note that, for the default qubit, the number of wires is optional since the backend can infer it from the circuit.

```python
dev=qml.device("default.qubit",wires=2)
```
We can also define our wire names at this point, if we’d prefer to not use the default naming system (eg 0,1,2), and can even use nonnumeric names this way:

```python
dev=qml.device("default.qubit",wires=[0,1,2])
```

```python
dev=qml.device("default.qubit",wires=["target","control"])
```

#### QNodes

QNodes pair a device with a quantum circuit. QNodes are most easily set up via a decorator, placed above a quantum function, and specified with the device name:

```python
dev=qml.device("default.qubit",wires=2)

@qml.qnode(dev)
def my_quantum_function(theta):
	qml.RX(theta,wires=0)
	qml.PauliY(wires=1)
	qml.Hadamard(wires=0)
	qml.Hadamard(wires=1)
	
	return qml.state()
```
We can also distinguish the qnode separately from the quantum function via the following:

```python
my_first_qnode=qml.QNode(my_first_quantum_function,dev)
```

#### Outputs

When a state is returned, as in the case of qml.state(), we see the amplitudes for the basis states. For example, in a 2-wire system (2 qubits), we may see an output of

\[0.191+0.462j -0.191-0.462j -0.191+0.462j 0.191-0.462j\]

which would translate to:

$$
|\psi\rangle=(0.191+0.462i)|00\rangle+(-0.191-0.462i)|01\rangle+(-0.191+0.462i)|10\rangle+(0.191-0.462i)|11\rangle
$$

In general, these states will be represented in “binary counting” order.

#### Codercise PF.1.1a - Writing a Quantum Function

Consider the following quantum circuit:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

Complete the circuit function that represents this quantum circuit. You will need to write the gates contained in this circuit. Note that the RY gate depends on an angle parameter.

```python
def circuit(theta,wires=[0,1]): # Write any arguments you need here
    """
    This quantum function implements the circuit shown above
    and returns the output quantum state
    """
    qml.Hadamard(wires=0)
    qml.PauliX(wires=1)
    qml.CNOT(wires=[0,1])
    qml.RY(theta,wires=1)

    ####################
    ###YOUR CODE HERE###
    ####################

    return qml.state()

```

#### Codercise PF.1.1b - Creating Devices

In the code below, define 3 devices as follows:

- dev_qubit: A “default.qubit” device with wires named “alice” and “bob”
- dev_mixed: A “default.mixed” device with two wires, with no specific names
- Turn example_circuit into a QNode by choosing a device and inserting the wires corresponding to the device of your choice. Feel free to switch devices so you can see how the output changes.

```python
dev_qubit = qml.device("default.qubit",wires=["alice","bob"]) # Define the device here
dev_mixed = qml.device("default.mixed",wires=2)# Define the device here

@qml.qnode(dev_qubit) # Choose the device you want
def example_circuit(theta):
    
    qml.RX(theta, wires = "alice" ) # Complete with wires in the device
    qml.CNOT(wires = ["alice","bob"] ) # Complete with wires in the device
    
    return qml.state()

print(example_circuit(0.3))
```

#### Codercise PF.1.1c - Circuit into QNode

Consider the circuit that you defined in PF.1.1a. Define a 2-wire “default.qubit” device and use it to define a QNode named circuit_qnode that applies the circuit and returns the state.

Note that since circuit is already defined in the backend, you don’t need to write it again.

```python
dev = qml.device("default.qubit",wires=2)# Define the device

circuit_qnode = qml.QNode(circuit,dev)# Assign a QNode to circuit"

print(circuit_qnode(0.3))

```

#### Codercise PF.1.2a - Defining Subcircuits

In this codercise, we will define the quantum circuits subcircuit_1 and subcircuit_2. Use those two subcircuits to create the full_circuit QNode shown below.

*[Image in Notion: subcircuit_1](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

*[Image in Notion: subcircuit_2](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

*[Image in Notion: fulHal_circuit](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```python
def subcircuit_1(angle):

    ####################
    ###YOUR CODE HERE###
    ####################
    qml.RX(angle,wires=0)
    qml.PauliY(wires=1)

    # No need to return anything

def subcircuit_2():

    ####################
    ###YOUR CODE HERE###
    ####################
    qml.Hadamard(wires=0)
    qml.CNOT(wires=[0,1])

    # No need to return anything

dev = qml.device('default.qubit', wires = 2)

# Decorate this function to create a QNode
@qml.qnode(dev)
def full_circuit(theta, phi):

    ####################
    ###YOUR CODE HERE###
    ####################   
    subcircuit_1(theta)
    subcircuit_2()
    subcircuit_1(phi)

    return qml.state()

    # Return the quantum state
```

#### Codercise PF.1.2b - Jumbled Wires

Based on the previous codercise, let’s use subcircuit_1 and subcircuit_2 to build the following full_circuit.

*[Image in Notion: subcircuit_1](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

*[Image in Notion: subcircuit_2](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

*[Image in Notion: full_circuit](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

One way to do this is to let the subcircuits depend on a list wire_list of wires, so that we can switch the order of the wires when we call the subcircuits. Build the QNode full_circuit using this strategy.

```python
def subcircuit_1(angle, wire_list):
    """
    Implements the first subcircuit as a function of the RX gate angle
    and the list of wires wire_list on which the gates are applied
    """
    print("starting sub1")
    qml.RX(angle,wires=wire_list[0])
    qml.PauliY(wires=wire_list[1])
    print("ending sub1")

    
    ####################
    ###YOUR CODE HERE###
    ####################

def subcircuit_2(wire_list):
    """
    Implements the second subcircuit as a function of the list of wires 
    wire_list on which the gates are applied
    """
    
    print("starting sub2")
    qml.Hadamard(wires=wire_list[0])
    qml.CNOT(wires=[wire_list[0],wire_list[1]])
    print("ending sub2")
    ####################
    ###YOUR CODE HERE###
    ####################

dev = qml.device("default.qubit", wires = [0,1])

@qml.qnode(dev)
def full_circuit(theta, phi):
    """
    Builds the full quantum circuit given the input parameters
    """
    subcircuit_1(theta,wire_list=[0,1])
    subcircuit_2(wire_list=[0,1])
    subcircuit_1(phi,wire_list=[1,0])

    ####################
    ###YOUR CODE HERE###
    ####################

    return qml.state()

```

---


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
