# PF: PennyLane Fundamentals

_Quantum Computing_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)


## Unit notes


<!-- notion-import -->
## Notes from Notion

_Copied from [🐣 PF (Pennylane fundamentals)](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)._

### PennyLane Fundamentals: Concept Summary

<details>
<summary>Circuits and QNodes: 5 Steps to Output State</summary>

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

</details>

<details>
<summary>Gradients</summary>

- Use partial derivatives for finding optimizations (gradient descent; easy with PennyLane functions)
- PennyLane offers auto-entangling functions

</details>

### PennyLane Fundamentals: Code Summary

### PennyLane Fundamentals: Code Summary

- Full basic setup (device→qnode→function)
- Subcircuits
    
    Subcircuits inherit the wire definitions by default. For example:
    
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
    
    We can switch this order by using a wire list. For example:
    
    ```python
    def subcircuit_1(angle,wire_list):
    
        qml.RX(angle, wires = 0)
        qml.PauliY(wires = 1)
    
    def subcircuit_2(wire_list):
    
        qml.Hadamard(wires = wire_list[0])
        qml.CNOT(wires = [wire_list[0],wire_list[1]])
    
    def full_circuit(theta, phi):
    
        subcircuit_1(theta,wire_list=[0,1])
        subcircuit_2(wire_list[0,1])
        subcircuit_1(phi,wire_list=[1,0])
    ```
    
- Support functions
    - State Preparation:
        
        Use qml.StatePrep for most cases
        
        ```python
        qml.StatePrep(state, wires=range(2),normalize=True)
        ```
        
        Use qml.BasisState for simple cases (|0> and |1>) (below makes the basis state (|100>)
        
        ```python
        qml.BasisState(np.array([1, 0, 0]), wires=range(3))
        ```
        
        Using qml.StatePrep for multiple qubits: Each possible state gets passed in (order is |000> → |001> → |010>, etc) (below example makes state $\alpha|001\rangle+\beta|010\rangle$)
        
        ```python
          qml.StatePrep(state=[0,alpha,beta,0,0,0,0,0],normalize=True,wires=[0,1,2])
        ```
        
    - Adjoint
        
        qml.adjoint can be used for both qml functions and subcircuits made by the user:
        
        ```python
        qml.adjoint(qml.RX)(0.5,wires=0)
        qml.adjoint(q_function)(theta,phi,omega)
        ```
        
    - Ctrl and Controlled Unitaries
        
        qml.ctrl can take a function (qml or user-defined subcircuit) and apply it as a controlled gate.
        
        ```python
           qml.ctrl(qml.RX, control = (0), control_values=(1))(angle, wires=[1])
        ```
        
        Likewise, qml.QubitUnitary can apply a single- (U) or multi-qubit (V) unitary matrix, and qml.ControlledQubitUnitary can do so as a controlled operation.
        
        ```python
        
          qml.QubitUnitary(U, wires = 0)
          qml.QubitUnitary(V, wires = [0,1])
          qml.ControlledQubitUnitary(U, wires = [0, 1, 2], control_values = (0,1))
        ```
        
    - Drawing a circuit:
        
        ```jsx
        In[1]: print(qp.draw(circuit, level = "device")([[0.1,0.2,0.3,0.4],[0.5,0.6,0.7, 0.8]]))
        
        Out[1]:
        0: ──RX(0.10)─╭●───────╭X──RX(0.50)─╭●───────╭X─┤  <Z>
        1: ──RX(0.20)─╰X─╭●────│───RX(0.60)─╰X─╭●────│──┤     
        2: ──RX(0.30)────╰X─╭●─│───RX(0.70)────╰X─╭●─│──┤     
        3: ──RX(0.40)───────╰X─╰●──RX(0.80)───────╰X─╰●─┤     
        ```
        
- Measurements
    
    
    | **Measurement** | **PennyLane** | **Output**  | **Needs shots>0** | **Differentiable** | **Needs observable** | **Output Example** |
    | --- | --- | --- | --- | --- | --- | --- |
    | Sample | qp.sample | Array | Yes | No | No | Out:  array([0, 1]) |
    | Counts | qp.counts | Dict | Yes | No | No | Out:  {'01': 1} |
    | Probability | qp.probs | Array | No | Yes | No | Out:  array([0., 1., 0., 0.]) |
    | Expectation Value | qp.expval | Float | No | Yes | Yes | qp.expval(qp.Z(wires=0)), qp.expval(qp.Z(wires=1))
    
    Out: (1.0, -1.0) |
- Entangling and Higher-Order Derivatives
    - Basic Entangling (CNOTs across the board)
        
        ```jsx
        def entangler_circuit(weights):
          qp.BasicEntanglerLayers(weights, wires = range(n_wires))
          return qp.expval(qp.PauliZ(0))
        ```
        
        - Note that stronglyentangledlayers will also rotate each of the wires in 3d space
    - Jacobian Matrix
        - The transformation matrix of the derivatives
        
        ```jsx
        
        @qp.qnode(dev,interface="autograd", diff_method="parameter-shift")
        def entangler(weights):
            qp.BasicEntanglerLayers(weights, wires = range(n_wires))
            return qp.expval(qp.PauliZ(0))
            
        In[2]: test_weights = np.array([[0.1,0.2,0.3,0.4]], requires_grad = True)
        
        print(qp.jacobian(entangler)(test_weights))
        
        ```
        
    - Hessian Matrix (higher-order derivatives; nested Jacobians)
        
        Since the Jacobian of a circuit is differentiable, we can find the Hessian of a scalar-valued function by nesting 2 qp.jacobians. Note that we set the differentiation method as parameter shift, and set max_diff to 2 to indicate we want the QNode to be differentiated twice (default value is 1)
        
        ```jsx
        dev = qp.device("default.qubit", wires = 2)
        
        @qp.qnode(dev, diff_method = "parameter-shift", max_diff = 2)
        def scalar_valued_circuit(params):
          qp.RX(params[0], wires = 0)
          qp.CNOT(wires=[0,1])
          qp.RY(params[1], wires = 0)
          return qp.expval(qp.PauliZ(0))
        
        test_params = np.array([0.7,0.3], requires_grad = True)
        qp.jacobian(qp.jacobian(circuit))(test_params)
        
        Out[5]:
        [[-0.73068165  0.19037934]
         [ 0.19037934 -0.73068165]]
        ```
        
- Optimization
    - Creating a cost function
        
        Given a circuit that returns an expectation value, and a polynomial cost function, it’s simple to implement a cost function which will return the resulting cost:
        
        ```jsx
        ## cost_function = x^3 - 0.5(x^2)+x
            x=circuit_as_function(params)
            return x**3-(0.5)*(x**2)+x# Return the value of the cost function
        ```
        
    - Creating an optimization
        
        By using gradient descent and cost functions, we can find the low/optimal values for a circuit:
        
        ```jsx
        def optimize(cost_function, init_params, *steps):
        
            opt = qp.GradientDescentOptimizer(stepsize = 0.4)
            steps = 100
            params = init_params
        
            for i in range(steps):
              params = opt.step(cost_function, params)
        
            return params, cost_function(params)
        ```
        
- Dynamic Circuits
    - qp.measure(x,reset=True) to reset qubit after measurement
    - qp.measure(x,postselect=True) to maintain qubit after measurement
    - qp.cond(measure1==0,qp.Hadamard)(0) to split the bit given a particular measurement
- Drawing and Snapshots
    - qp.draw(circuit) to produce a text-style circuit drawing
    - qp.draw_mpl(circuit) to produce a matplotlib style drawing of the circuit
    - qp.Snapshot(”state name”) to produce a snapshot for later debugging
    - qp.breakpoint() to add a breakpoint for debugging
    - qp.debug_tape() to visuallize a circuit at a breakpoint
    - 
## Lessons

1. [Circuits and QNodes](./01-circuits-and-qnodes/)
2. [Quantum Operations](./02-quantum-operations/)
3. [Measurements in PennyLane](./03-measurements-in-pennylane/)
4. [Gradients and Optimization](./04-gradients-and-optimization/)
5. [Dynamic Circuits](./05-dynamic-circuits/)
6. [Inspecting Quantum Circuits](./06-inspecting-quantum-circuits/)
