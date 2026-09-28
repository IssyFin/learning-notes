# Inspecting Quantum Circuits

_Quantum Computing › PennyLane Fundamentals_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)

## Properties of a Quantum Circuit

Tools that examine circuits are implemented in PennyLane as trasnsforms - generally they take a QNode as an input and return a new function that accepts the same arguments as the original QNode.

For example, given the circuit below, we can use qp.specs() to return detailed information about the circuit, including # of gates, circuit depth, types of gates, and more.

```python
dev = qp.device('default.qubit', wires=4)

@qp.qnode(dev, diff_method='parameter-shift')
def circuit(x, y):
    qp.RX(x[0], wires=0)
    qp.Toffoli(wires=(0, 1, 2))
    qp.CRY(x[1], wires=(0, 1))
    qp.Rot(x[2], x[3], y, wires=0)
    return qp.expval(qp.Z(0)), qp.expval(qp.X(1))
    
x = np.array([0.05, 0.1, 0.2, 0.3], requires_grad=True)
y = np.array(0.4, requires_grad=False)
specs_func = qp.specs(circuit)
>>>specs_func(x, y)
{'resources': Resources(num_wires=3, num_gates=4, gate_types=defaultdict(<class 'int'>, {'RX': 1, 'Toffoli': 1, 'CRY': 1, 'Rot': 1}), depth=4, shots=0),
 'gate_sizes': defaultdict(int, {1: 2, 3: 1, 2: 1}),
 'gate_types': defaultdict(int, {'RX': 1, 'Toffoli': 1, 'CRY': 1, 'Rot': 1}),
 'num_operations': 4,
 'num_observables': 2,
 'num_diagonalizing_gates': 1,
 'num_used_wires': 3,
 'num_trainable_params': 4,
 'depth': 4,
 'num_device_wires': 4,
 'device_name': 'default.qubit',
 'gradient_options': {},
 'interface': 'auto',
 'diff_method': 'parameter-shift',
 'gradient_fn': 'pennylane.gradients.parameter_shift.param_shift',
 'num_gradient_executions': 10}
```

## Visualizing Circuits

PennyLane can also visualize quantum circuits. qp.draw() will produce text-based diagrams and qp.draw_mpl() will produce Matplotlib-based graphical representations.

```python
dev = qp.device('default.qubit')

@qp.qnode(dev)
def circuit(x, z):
    qp.QFT(wires=(0,1,2,3))
    qp.IsingXX(1.234, wires=(0,2))
    qp.Toffoli(wires=(0,1,2))
    mcm = qp.measure(1)
    mcm_out = qp.measure(2)
    qp.CSWAP(wires=(0,2,3))
    qp.RX(x, wires=0)
    qp.cond(mcm, qp.RY)(np.pi / 4, wires=3)
    qp.CRZ(z, wires=(3,0))
    return qp.expval(qp.Z(0)), qp.probs(op=mcm_out)
    
>>> print(qp.draw(circuit)(1.2345,1.2345))
0: ─╭QFT─╭IsingXX(1.23)─╭●───────────╭●─────RX(1.23)─╭RZ(1.23)─┤  <Z>
1: ─├QFT─│──────────────├●──┤↗├──────│───────────────│─────────┤
2: ─├QFT─╰IsingXX(1.23)─╰X───║───┤↗├─├SWAP───────────│─────────┤
3: ─╰QFT─────────────────────║────║──╰SWAP──RY(0.79)─╰●────────┤
                             ╚════║═════════╝
                                  ╚════════════════════════════╡  Probs[MCM]
                                  
fig, ax = qp.draw_mpl(circuit, style='pennylane')(1.2345,1.2345)
fig.show()
```

qp.Snapshot() can also be used to capture a snapshot of the quantum state, including either a ket, a density matrix, or a covariance matrix depending on th esimulator used, or the result of an arbitrary measurement.

```python
dev = qp.device("default.qubit", wires=2)

@qp.qnode(dev, interface=None)
def circuit():
    qp.Snapshot(measurement=qp.expval(qp.Z(0)))
    qp.Hadamard(wires=0)
    qp.Snapshot("very_important_state")
    qp.CNOT(wires=[0, 1])
    qp.Snapshot()
    return qp.expval(qp.X(0))

>>> qp.snapshots(circuit)()
{0: 1.0,
'very_important_state': array([0.707+0.j, 0.+0.j, 0.707+0.j, 0.+0.j]),
2: array([0.707+0.j, 0.+0.j, 0.+0.j, 0.707+0.j]),
'execution_results': 0.0}
```

!image.png

## Interactive Debugging

PennyLane Debugger (PLDB), based on Python debugger (PDB) gives an interface for debugging, with commands including list, longlist, next, continue, and quit. To enter, insert breakpoints with qp.breakpoint().

```python
dev = qp.device("default.qubit", wires=2)

@qp.qnode(dev)
def circuit(x):
    qp.breakpoint()

    qp.RX(x, wires=0)
    qp.Hadamard(wires=1)

    qp.breakpoint()

    qp.CNOT(wires=[0, 1])
    return qp.expval(qp.Z(0))

circuit(1.23)

> /Users/your/path/to/script.py(8)circuit()
-> qp.RX(x, wires=0)
[pldb] list
  3
  4         @qp.qnode(dev)
  5         def circuit(x):
  6             qp.breakpoint()
  7
  8  ->         qp.RX(x, wires=0)
  9             qp.Hadamard(wires=1)
 10
 11             qp.breakpoint()
 12
 13             qp.CNOT(wires=[0, 1])
[pldb] next
> /Users/your/path/to/script.py(9)circuit()
-> qp.Hadamard(wires=1)

> /Users/your/path/to/script.py(10)circuit()
-> qp.RX(x, wires=0)
[pldb] qp.debug_state()
array([1.+0.j, 0.+0.j, 0.+0.j, 0.+0.j])
[pldb] continue
> /Users/your/path/to/script.py(15)circuit()
-> qp.CNOT(wires=[0, 1])
[pldb] qp.debug_probs()
array([0.33355943, 0.33355943, 0.16644057, 0.16644057])
```

The debug tools can also provide a visual representation of the circuit structure at a breakpoint, using qp.debug_tap():

```python
[pldb] qtape = qp.debug_tape()
[pldb] print(qtape.draw())
0: ──RX─┤  
1: ──H──┤  
```

<aside>

## Codercise PF.6.1 - Draw!

Practice drawing quantum circuits. Feel free to play around with the circuit itself.

```python
dev = qp.device("default.qubit", wires=3)

@qp.qnode(dev)
def circuit():
    """
    Implements a circuit and returns the state
    """
    qp.Hadamard(wires=0)
    qp.CRY(np.pi / 4, wires=(0, 1))
    qp.CRX(np.pi / 3, wires=(1, 2))
    qp.S(wires=1)
    qp.T(wires=2)
    qp.Toffoli(wires=(0, 1, 2))
    qp.SWAP(wires=(0, 2))
    return qp.state()

print(qp.draw(circuit)()) # use qp.draw()

0: ──H─╭●─────────────────────╭●─╭SWAP─┤  State
1: ────╰RY(0.79)─╭●─────────S─├●─│─────┤  State
2: ──────────────╰RX(1.05)──T─╰X─╰SWAP─┤  State

```

</aside>

<aside>

## Codercise PF.6.2 - Lights, Camera, Snapshot

Practice taking and reviewing snapshots. Play around with the number and location of the snapshots. For example, start by implementing a new qp.Snapshot("foo") before the Toffoli gate in the following circuit.

Feel free to modify the circuit itself as well.

```python

dev = qp.device("default.qubit", wires=3)

@qp.qnode(dev)
def circuit():
    """
    Implements a circuit and returns the state
    """
    qp.Hadamard(wires=0)
    qp.CRY(np.pi / 4, wires=(0, 1))
    qp.CRX(np.pi / 3, wires=(1, 2))
    qp.Snapshot("very_important_state")
    qp.S(wires=1)
    qp.T(wires=2)
    qp.Snapshot("before Toffoli")
    qp.Toffoli(wires=(0, 1, 2))
    qp.Snapshot(measurement=qp.expval(qp.Z(0)))
    qp.SWAP(wires=(0, 2))
    return qp.state()

for key, val in qp.snapshots(circuit)().items(): print(key, val)

very_important_state [0.70710678+0.j         0.        +0.j         0.        +0.j
 0.        +0.j         0.65328148+0.j         0.        +0.j
 0.23434479+0.j         0.        -0.13529903j]
before Toffoli [0.70710678+0.j         0.        +0.j         0.        +0.j
 0.        +0.j         0.65328148+0.j         0.        +0.j
 0.        +0.23434479j 0.09567086+0.09567086j]
2 5.551115123125783e-17
execution_results [0.70710678+0.j         0.65328148+0.j         0.        +0.j
 0.09567086+0.09567086j 0.        +0.j         0.        +0.j
 0.        +0.j         0.        +0.23434479j]
```

</aside>
