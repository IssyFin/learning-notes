# Measurements in PennyLane

_Quantum Computing › PennyLane Fundamentals_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)

<!-- notion-import -->
## Notes from Notion

_Copied from **Measurements in PennyLane** in [🐣 PF (Pennylane fundamentals)](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)._

### Measurements Cheatsheet

PennyLane measurements can work on both simulations and quantum hardware. They can also be customized as shown below:

| **Measurement** | **PennyLane** | **Output ** | **Needs shots\>0** | **Differentiable** | **Needs observable** | **Output Example** |
|---|---|---|---|---|---|---|
| Sample | qp.sample | Array | Yes | No | No | Out:  array(\[0, 1\]) |
| Counts | qp.counts | Dict | Yes | No | No | Out:  \{'01': 1\} |
| Probability | qp.probs | Array | No | Yes | No | Out:  array(\[0., 1., 0., 0.\]) |
| Expectation Value | qp.expval | Float | No | Yes | Yes | qp.expval(qp.Z(wires=0)), qp.expval(qp.Z(wires=1))<br><br>Out: (1.0, -1.0) |

### Observables

Observables are a class based on Hermitian Operators (like PauliX, Hadamard) offered by PennyLane, and can be required as arguments for some measurements. Custom operators can also be created, using existing operators. For example, to create the operation $`O=\frac{1}{2}X\otimes X+\frac{1}{4}Y\otimes Y`$, we type the following, where @ represents the tensor product and 0 and 1 represent the relevant wires.

```python
O = 1/2 * qp.X(0)@qp.X(1) + 1/4 * qp.Y(0) @ qp.Y(1)
```
An observable O represents the possible measurements of the quantum system:

The eigenvalues $`\lambda_i`$ of the operator O represent the values that the measurement outcomes can have.

The eigenstate $`|\lambda_i\rangle`$ associated by the eigenvalue $`\lambda_i`$ represents the state right after measurement

Remember that, by Bohr’s Rule, the probability of measuring $`\lambda_i`$ in state $`|\psi\rangle`$ right before measurement is

$`p(i)=|\langle\lambda_i|\psi\rangle|^2`$.

### Samples

Quantum samples are implemented in PennyLane via qp.sample by specifying the number of shots (number of times we run the circuit and obtain a sample). The following code represents specifying the sample with a computation.

```javascript
dev = qp.device("default.qubit",wires=1,shots=10)

@qp.qnode(dev)
def circuit():
	qp.Hadamard(wires=0)
	return qp.sample()
print(circuit())
//output resembles:
//Out[1]:[0 0 0 1 1 1 0 1 0 1]
```
Note that, the greater the number of samples, the closer the output will come to match the expected probability.

The sample function can also be used with an observable, as shown in the example below.

```javascript
@qp.qnode(dev)
def circuit_paulix():
	qp.Hadamard(wires=0)
	return qp.sample(qp.PauliX(0))
print(circuit_paulix())
//output resembles:
//Out[2]: [1. 1. 1. 1. 1. 1. 1. 1. 1. 1.]
```

### Counts

With many samples (hundreds or thousands of shots), you get too many output lines to reasonably review. By using counts, this information can be summarized - instead of a list, you produce a dictionary with the number of counts for each sample.

Examples are shown below for both computation and observable.

```javascript
dev = qp.device("default.qubit",wires=1,shots=1000)

@qp.qnode(dev)
def circuit_counts():
	qp.Hadamard(wires=0)
	return qp.counts()
print(circuit_counts())
//output resembles:
//Out[3]: {'0': array(497),'1': array(503)}
```

```javascript
@qp.qnode(dev)
def counts_paulix():
	qp.Hadamard(wires=0)
	return qp.sample(qp.PauliX(0))
print(counts_paulix())
//output resembles:
//Out[4]: {1.0: array(1000)}
```

### Probabilities

Rather than counting samples, we can also come up with an expected probability by using qp.probs(), leaving shots=None (the default setting).

```javascript
dev = qp.device("default.qubit",wires=2)
@qp.qnode(dev)
def simple_circuit():
	qp.PauliX(1)
	return qp.probs()
//output resembles:
//Out[4]: array([0.,1.,0.,0.])
```
Note that 1 corresponds to state $`|01\rangle`$, meaning that we find that state with a probability of 1.

We can also find the probability for a single wire, as below:

```javascript
@qp.qnode(dev)
def simple_circuit():
	qp.PauliX(1)
	return qp.probs(wires=0)
//output resembles:
//Out[5]: array([1.,0.])
```
Note that, since wires is the first argument for qp.probs, we need to specify op or PennyLane will try to read the operator as a wire.

```javascript
dev = qp.device("default.qubit",wires=2)

@qp.qnode(dev)
def prob_circuit_paulix():
	qp.PauliX(1)
	return qp.probs(op=qp.PauliX(0)@qp.PauliX(10)
print(prob_circuit_paulix())
//output resembles:
//Out[6]: array([0.25, 0.25, 0.25, 0.25])
```

### Expectation Values

Expectation values are the most commonly used measurements, particularly for optimization and machine learning (since differentiable and therefore not stochastic). We represent the observable (with operator $`\hat{O}`$ and state $`|\psi\rangle`$ yielding expectation value $`\langle\hat{O}\rangle=\langle\psi|\hat{O}|\psi\rangle`$.

We can also check to see the analytic expectation value. An example of Pauli Z is shown below.

```javascript
dev=qp.device(default.qubit",wires=2)

@qp.qnode(dev)
def simple_circuit_expval():
	qp.PauliX(wires=1)
	return qp.expval(qp.PauliZ(0))
simple_circuit_expval()
//output resembles:
//Out[7]: array(1.)
```
Note that qp.expval only takes an observable as an argument, since wires are specified in the observable itself.

### Multiple Measurements

PennyLane can also return multiple measurements at once; for example, the following produces a tuple ((1.0,-1.0)). Multiple different types of measurements can be combined.

```javascript
qp.expval(qp.Z(wires=0)),qp.expval(qp.Z(wires=1))
//output resembles:
//(1.0,-1.0)
```

### Interfaces

Note that PennyLane also interfaces with classical and quantum libraries; if QNode receives input data/parameters of a specific type (eg Torch tensors), it outputs a related type (eg Torch tensor rather than an array).

### Codercise PF.3.1 - Taking Samples

Given the following quantum circuit, take a single sample from both qubits in the computational basis. Note that you will need to create an instance of a device and add a measurement to the circuit function representing the quantum circuit. Note that you should only use a single measurement for both qubits (rather than one measurement per qubit).

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

<details>
<summary>My Code</summary>

```javascript
dev = qp.device("default.qubit",wires=2,shots=1) # Define a two-qubit device here

@qp.qnode(dev)
def circuit():
    """
    This quantum function implements the circuit shown above
    and should return a sample from all qubits
    """

    qp.H(wires=0)
    qp.CNOT(wires=[0,1])

    return qp.sample() # Add your measurement here

```

</details>

### Codercise PF.3.2 - Hermitian Observable

In PennyLane, we can represent an arbitrary Hermitian observable by using qp.Hermitian.

Note that calculating the expectation value of a Hermitian matrix A can be useful for obtaining information such as the fidelity between A and the state of the circuit on measurement. Note also that using qp.Hermitian together with qp.probs is not supported here.

For the same circuit as in PF.3.1, build a 2-qubit device and return the expectation value for A when it acts on the first qubit, using the analytic value rather than shots.

<details>
<summary>My Code</summary>

```javascript

dev = qp.device("default.qubit",wires=2) # Define a two-qubit device here

A = np.array([[1, 0], [0, -1]])

@qp.qnode(dev)
def circuit():
    """
    This quantum function implements a Bell state and
    should return the expectation value the observable
    corresponding to the matrix A applied to the first qubit
    """
    qp.H(wires=0)
    qp.CNOT(wires=[0,1])
    return qp.expval(qp.Hermitian(A,0)) # Add your measurement here
```

</details>

### Codercise PF.3.3 - Tensor Observables

In PennyLane, we can use @ to create a tensor product of observables. For example. qp.Z(wires=0)@qp.Z(wires=1) represents the tensor product of the PauliZ observable for qubits 0 and 1, which is useful for taking a measurement for multiple qubits at once.

For the same circuit as in PF.3.1, build a 2-qubit device and return the probabilities for the tensor observable $`\langle Z_0 \otimes Z_1 \rangle`$. Use the analytic value rather than shots.

<details>
<summary>My Code</summary>

```javascript

dev =qp.device("default.qubit",wires=2) # Define a two-qubit device here

@qp.qnode(dev)
def circuit():
    """
    This quantum function implements a Bell state and
    should return the probabilities for the PauliZ 
    observable on both qubits, using a single measurement
    """
    qp.H(wires=0)
    qp.CNOT(wires=[0,1])
    return qp.probs(op=qp.Z(0)@qp.Z(1)) # Add your measurement here
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
