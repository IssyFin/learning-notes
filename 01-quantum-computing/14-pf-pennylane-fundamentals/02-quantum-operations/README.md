# Quantum Operations

_Quantum Computing › PennyLane Fundamentals_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)

<!-- notion-import -->
## Notes from Notion

_Copied from **Quantum operations** in [🐣 PF (Pennylane fundamentals)](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)._

### Quantum State Preparation

Qubits in PennyLane start in state $`|0\rangle`$ by default, or $`\begin{pmatrix}1&0\end{pmatrix}`$.

To prepare a qubit in custom state $`|\psi\rangle`$, one can use qml.StatePrep with a normalized 2-dimensional vector:

```python
dev = qml.device('default.qubit', wires=2)

@qml.qnode(dev)
def circuit(state=None):
    qml.StatePrep(state, wires=range(2))
    return qml.state()

state = circuit([1/2, 1/2, 1/2, 1/2])
```
Note that we can have PennyLane normalize the state by adding the option “normalize=True”.

We can also use qml.BasisState to generate an array of binary qubits:

```python
qml.BasisState(np.array([1, 0, 0]), wires=range(3))
```

### Multi-Qubit and Single-Qubit Gates

Although PennyLane provides many single- and multi- qubit gates, it’s possible to generate *any* unitary gate through the use of qml.QubitUnitary. For example, to implement single-qubit unitary U on wire 0 and two-qubit unitary V on wires \[0,1\]:

```python
@qml.qnode(dev)
def qubit_unitaries(U,V):
  qml.QubitUnitary(U, wires = 0)
  qml.QubitUnitary(V, wires = [0,1])
  return qml.state()

U = np.array([[-0.69165024-0.50339329j,  0.28335369-0.43350413j],
       [ 0.1525734 -0.4949106j , -0.82910055-0.2106588j ]])
V = np.array([[-0.01161649+0.12340198j,  0.24202953+0.47179157j,
        -0.66720111+0.23783294j,  0.38909577-0.22439714j],
       [-0.47281374+0.235468j  , -0.51436345+0.28615452j,
         0.34116689+0.18781118j,  0.08677015-0.46405913j],
       [-0.53198348+0.60728927j,  0.34990852-0.34440384j,
         0.00247372-0.07165224j,  0.18828175+0.257979j  ],
       [-0.07641446-0.2190734j ,  0.27910452-0.23115134j,
         0.03829659-0.58309818j,  0.21999088-0.65189823j]])
```

### Controlled Unitaries

Controlled gates are the most common form of multi-qubit gates - they either act on a control qubit with control_value=1 (filled dot) or control_value=0 (empty dot).

As long as the gate U exists as a PennyLane operator, we can use qml.ctrl to apply U as a control, using the following arguments:

- op: the operation; U
  - Note, this can either be a PennyLane operator of the form qml.\*\*\* or as a quantum function, or a user-defined quantum function, defined as matrix U applied with qml.ControlledQubitUnitary [(see notes below)\*](/2b544de2a6568084b8c2c3aa39b0e9a0?pvs=25#34944de2a65680e592e5fc7d0159f30c)
- control: the list of wires that act as controls
- control_values: a list containing the control values assigned to each of the control_wires
The qml.ctrl function can be applied as follows:

```python
dev = qml.device("default.qubit", wires = 2)

@qml.qnode(dev)
def controlled_gate_circuit(angle):
   qml.PauliX(0) #Flip to |1> for RX to act on the second wire
   qml.ctrl(qml.RX, control = (0), control_values=(1))(angle, wires=[1])
   return qml.state()
```

#### Applying a user-defined unitary matrix

To use matrix U (the controlled unitary to be appleid), we use qml.ControlledQubitUnitary with the two mandatory arguments:

- U: The matrix (np.array) of unitary U
- wires: The list of wires on which the controlled operator U acts, consisting of the control wire(s) followed by the target wires.
We can also specify the control_values, a list of the control values for the control wires.

As an example, we can apply unitary $`U=[[0.94877859, 0.31594146],[-0.31594146,0.94877869]]`$ on wires 2, controlled on wires 0 (0 control) and 1 (1 control)

```python
dev = qml.device("default.qubit", wires = 3)

U =[[ 0.94877869,  0.31594146], [-0.31594146,  0.94877869]]

@qml.qnode(dev)
def circuit_controlled_unitary():

  qml.PauliX(wires = 1) #Flip to apply the controlled gate
  qml.ControlledQubitUnitary(U, wires = [0, 1, 2], control_values = (0,1))
  return qml.state()
```

### Inverse Operations

Many quantum algorithms will require applying the inverse of an operator. Note that, if U is unitary, $`U^{-1}=U^\dagger`$, and can simply apply the qml.adjoint function.

For example, to apply qml.adjoint to a gate RX, we can use the function as a wrapper. In other words, the two lines below are equivalent.

```python
qml.adjoint(qml.RX(0.5,wires=0))
qml.adjoint(qml.RX)(0.5,wires=0)
```
qml.adjoint can also apply the adjoint of a quantum function. The example below shows how we can find the adjoint of a circuit:

```python
def q_function(theta, phi, omega):
  qml.RX(theta, wires = 0)
  qml.RY(phi, wires = 1)
  qml.RZ(omega, wires = 2)
 
qml.adjoint(q_function)(theta,phi,omega)
```

#### Codercise PF.2.1 - Quantum State Preparation

Using qml.StatePrep, write the state_preparation QNode that prepares a quantum state proportional to $`|\psi\rangle=\alpha|001\rangle+\beta|010\rangle+\gamma|100\rangle`$.

Note that state_preparation takes the complex numbers $`\alpha,\beta,\gamma`$ as arguments. Do not assume that the state is normalized (ie, it might be the case that $`|\alpha|^2+|\beta|^2+|\gamma|^2 \neq1`$

```python
dev = qml.device("default.qubit", wires = 3)

@qml.qnode(dev)
def prep_circuit(alpha, beta, gamma):
    """
    Prepares the state alpha|001> + beta|010> + gamma|100>.
    Args:
    alpha, beta, gamma (np.complex): The coefficients of the quantum state
    to prepare.
    Returns:
    (np.array): The quantum state
    """

    ####################
    ###YOUR CODE HERE###
    ####################
    
    qml.StatePrep(state=[0,alpha,beta,0,gamma,0,0,0],normalize=True,wires=[0,1,2])
    
    return qml.state()

alpha, beta, gamma = 1/np.sqrt(3), 1/np.sqrt(3), 1/np.sqrt(3),

print("The prepared state is", prep_circuit(alpha, beta, gamma))

```

#### Codercise PF.2.2 - A circuit with single-qubit gates

Complete the single_qubit_gates QNode so that it applies the circuit below and returns the quantum state. Note that the single_qubit_gates circuit depends on the parameters theta for the RX gate and phi for the RZ gate.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```python
dev = qml.device("default.qubit", wires = 2)

@qml.qnode(dev)
def single_qubit_gates(theta, phi):
    """
    Implements the quantum circuit shown in the statement
    Args:
    - theta, phi (float): The arguments for the RX and RZ gates, respectively
    Returns:
    - (np.array): The output quantum state.
    
    """

    ####################
    ###YOUR CODE HERE###
    ####################
    qml.H(wires=0)
    qml.H(wires=1)
    qml.T(wires=0)
    qml.S(wires=1)
    qml.RX(theta,wires=0)
    qml.RZ(phi,wires=1)
    
    return qml.state()

theta, phi = np.pi/3, np.pi/4
print("The output state of the circuit is: ", single_qubit_gates(theta, phi))
```

#### Codercise PF.2.3 - A circuit with multi-qubit gates

Complete the multi_qubit_gates QNode so that it applies the circuit below and returns the quantum state. Note that multi_qubit_gates circuit depends on the parameters theta for CRX and phi for CRY.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```python
dev = qml.device("default.qubit", wires = 3)

@qml.qnode(dev)
def multi_qubit_gates(theta,phi):
    """
    Applies the circuit shown the figure above
    Args:
    theta, phi (float): parameters of the CRX and CRY gates, in that order.
    Returns:
    - (np.array): the quantum state
    """
    
    #####################
    ###YOUR CODE HERE####
    #####################
    qml.H(wires=0)
    qml.CRY(phi,wires=[0,1])
    qml.CRX(theta,wires=[1,2])
    qml.S(wires=1)
    qml.T(wires=2)
    qml.Toffoli(wires=[0,1,2])
    qml.SWAP(wires=[0,2])
    return qml.state()

theta, phi = np.pi/3, np.pi/4
print("The output state is: \n", multi_qubit_gates(theta, phi))
```

#### Codercise PF.2.4 - Gates under control

Complete the following QNode to implement the following circuit containing controlled native PennyLane gates. Remember that you can use qml.ctrl. Note that the multi_qubit_gates circuit depends on the parameters theta (for RX) and phi (for RY)

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```python
dev = qml.device("default.qubit", wires = 3)

@qml.qnode(dev)
def ctrl_circuit(theta,phi):
    """Implements the circuit shown in the Codercise statement
    Args:
        theta (float): Rotation angle for RX
        phi (float): Rotation angle for RY
    Returns:
        (numpy.array): The output state of the QNode
    """

    ####################
    ###YOUR CODE HERE###
    ####################
    qml.RY(phi,wires=0)
    qml.H(wires=1)
    qml.RX(theta,wires=2)
    qml.ctrl(qml.S,control=(0),control_values=(1))(wires=1)
    qml.ctrl(qml.T,control=(1),control_values=(0))(wires=2)
    qml.ctrl(qml.H,control=(2),control_values=(1))(wires=0)
    
    
    return qml.state()
```

#### Codercise PF.2.5 - Kicking U Back

The phase kickback routine, shown below, is commonly used in quantum circuits. Write the phase_kickback QNode, which applies the phase kickback algorithm to a single-qubit operator, represented by a given matrix. You may need qml.ControlledQubitUnitary.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```python
dev = qml.device("default.qubit", wires = 2)

@qml.qnode(dev)
def phase_kickback(matrix):
    """Applies phase kickback to a single-qubit operator whose matrix is known
    Args:S
    - matrix (numpy.array): A 2x2 matrix
    Returns:
    - (numpy.array): The output state after applying phase kickback
    """

    ####################
    ###YOUR CODE HERE###
    ####################
    qml.H(wires=0)
    qml.ControlledQubitUnitary(matrix,wires=[0,1])
    qml.H(wires=0)

    return qml.state()

matrix = np.array([[-0.69165024-0.50339329j,  0.28335369-0.43350413j],
    [ 0.1525734 -0.4949106j , -0.82910055-0.2106588j ]])

print("The state after phase kickback is: \n" , phase_kickback(matrix))
```

#### Codercise PF.2.6 - Do, apply, undo

Many quantum algorithms follow a pattern shown in the figure below. We apply an operator V, then a controlled operation U, and then finally the inverse V+. In the code below, the single-qubit operation V is given to you as the subcircuit do, which depends on parameters k. It prepares a state proportional to \|0\>+k\|1\>.

You are also provided U as the apply subcircuit, which simply applies the theta dependent gate lsingX.

Complete the do_apply_undo circuit below that applies the pattern discussed above, with V=do(k) and U=apply(theta).

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```python
dev = qml.device("default.qubit")

def do(k):

    qml.StatePrep([1,k], wires = [0], normalize = True)

def apply(theta):

    qml.IsingXX(theta, wires = [1,2])

@qml.qnode(dev)
def do_apply_undo(k,theta):
    """
    Applies V, controlled-U, and the inverse of V
    Args: 
    - k, theta: The parameters for do and apply (V and U) respectively
    Returns:
    - (np.array): The output state of the routine
    """

    ####################
    ###YOUR CODE HERE###
    ####################
    do(k)
    qml.ctrl(apply,control=(0),control_values=(1))(theta)
    qml.adjoint(do)(k)

    return qml.state()

k, theta = 0.5, 0.8

print("The output state is: \n", do_apply_undo(k, theta))
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
