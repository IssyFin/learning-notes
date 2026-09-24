# Measurements

_Quantum Computing › Single-Qubit Gates_

[Course link](https://pennylane.ai/codebook/single-qubit-gates/measurements)

<!-- notion-import -->
## Notes from Notion

_Copied from **Measurements (Probabilistic nature)** in [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

### Projective Measurements

A qubit $`|\psi\rangle=\alpha|0\rangle+\beta|1\rangle`$ will be found in state $`|0\rangle`$ with probability $`|\alpha|^2=\alpha\alpha^*`$ and in state $`|1\rangle`$ with probability $`|\beta|^2=\beta\beta^*`$. We can also express these measurement outcome probabilities with the *inner product:*

Given a quantum state $`|\psi\rangle`$, the probability that we observe it in state $`|\phi\rangle`$ when we measure it with respect to a basis that includes $`|\phi\rangle`$ is equal to:

$`PR(\phi)=|\langle\phi|\psi\rangle|^2`$,

#### Exercise I.9.1

Compute Pr(0) and Pr(1) for $`|\psi\rangle=\alpha|0\rangle+\beta|1\rangle`$ using the inner product method. Note that, since the basis states are orthogonal, the calculation is simplified

$$
Pr(0) = |\langle0|\psi\rangle|^2=|\alpha\langle0|0\rangle+\beta\langle0|1\rangle|^2=|\alpha|^2\\
Pr(1)=|\langle1|\psi\rangle|^2=|\alpha\langle1|0\rangle+\beta\langle1|1\rangle|^2=|\beta|^2
$$

This measurement is known as a ***projective measurement***. Essentially, “how much does each basis vector contribute to a given state”? It can be thought of visually as an overlap of two state vectors with their inner product, like in linear algebra projections. Note that in projective measurements, we take the modulus squared of the inner product so we can get a real-valued probability.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

### Bases

We do not always work in the $`|0\rangle`$ and $`|1\rangle`$ bases - sometimes, measuring this way makes it impossible to tell quantum states apart. If the basis is unspecfiied, however, we can generally assume that our basis is $`|0\rangle |1\rangle`$.

#### Exercise I.9.2

A basis consists of vectors that are orthonormal (both normalized and orthogonal). This ensures that basis states are valid qubit states (normalization) and that the states are linearly independent (orthogonal).

Do the states below form a basis?

$$
|+\rangle=\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)\\
|-\rangle=\frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)
$$

Step 1: See if states are normalized (Sum the squares, should = 1)

$$
|+\rangle^2=\frac{1}{\sqrt{2}}\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)=\frac{1}{2}|0\rangle+\frac{1}{2}|1\rangle\\
|-\rangle^2=\frac{1}{2}|0\rangle-\frac{1}{2}|1\rangle\\
|+\rangle^2+|-\rangle^2=1|0\rangle+0|1\rangle=1|0\rangle=1\checkmark
$$

Or:

$$
\langle+|+\rangle=\begin{pmatrix}\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}\end{pmatrix}\begin{pmatrix}\frac{1}{\sqrt{2}}&\frac{1}{\sqrt{2}}\end{pmatrix}\\
\langle+|+\rangle=\frac{1}{2}+\frac{1}{2}=1\checkmark\\
\langle-|-\rangle=\begin{pmatrix}\frac{1}{\sqrt{2}}\\-\frac{1}{\sqrt{2}}\end{pmatrix}\begin{pmatrix}\frac{1}{\sqrt{2}}&-\frac{1}{\sqrt{2}}\end{pmatrix}\\
\langle-|-\rangle=\frac{1}{2}-(-\frac{1}{2})=1\checkmark
$$

Step 2: See if states are orthogonal (Multiply the states, if 0, then orthogonal)

$$
|+\rangle=\begin{pmatrix}\frac{1}{\sqrt{2}}\\\frac{1}{\sqrt{2}}\end{pmatrix}\\
|-\rangle=\begin{pmatrix}\frac{1}{\sqrt{2}}\\-\frac{1}{\sqrt{2}}\end{pmatrix}\\
\langle+|-\rangle=\frac{1}{2}-\frac{1}{2}=0\checkmark
$$

Note that, while there are an infinite number of bases we can use to represent a qubit state, the most common ones are the computational basis and the Hadamard basis.

### Basis Rotations

We can move from one basis to another via basis rotation. One way to do this is to re-express computational base states via Hadamard base states.

$$
|0\rangle=\frac{1}{\sqrt{2}}(|+\rangle+|-\rangle)\\
|1\rangle=\frac{1}{\sqrt{2}}(|+\rangle-|-\rangle)
$$

When we substitute these into the expression for $`|\psi\rangle`$, we see the following:

$$
|\psi\rangle=\alpha|0\rangle+|\beta\rangle\\
=\alpha\frac{1}{\sqrt{2}}(|+\rangle+|-\rangle+\beta\frac{1}{\sqrt{2}}(|+\rangle-|-\rangle)\\
=(\frac{\alpha+\beta}{\sqrt{2}})|+\rangle+(\frac{\alpha-\beta}{\sqrt{2}})|-\rangle
$$

After rotating bases, we can take a measurement with respect to that basis:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

#### Exercise I.9.3

Use the inner product method to compute the measurement outcome probabilities of measuring $`|\psi\rangle=\alpha|0\rangle+\beta|1\rangle`$ in the Hadamard basis $`|+\rangle,|-\rangle`$

$$
(\frac{\alpha+\beta}{\sqrt{2}})|+\rangle+(\frac{\alpha-\beta}{\sqrt{2}})|-\rangle\\
|+\rangle:\\
\frac{\alpha+\beta}{\sqrt{2}}*\frac{\alpha+\beta}{\sqrt{2}}=\frac{|\alpha|^2+|\beta|^2+2\alpha\beta}{2}\\
|-\rangle:\\
\frac{\alpha-\beta}{\sqrt{2}}*\frac{\alpha-\beta}{\sqrt{2}}=\frac{|\alpha|^2+|\beta|^2-2\alpha\beta}{2}\\
$$

Note that, in quantum computing hardware (and software), measurements in other bases are very difficult in practice - it’s much more straightforward to measure in the computational basis.

However, we can calculate in one basis, then rotate bases before measurement. Eg, if we want to measure in the Hadamard basis, we can “trick”the quantum computer by rotating states before applying the measurement, so long as we have an operation that maps between the two bases (\|+\> to \|0\> and \|-\> back to \|1\>). We can then measure and observe \|0\>, knowing what we really had was \|+\> and likewise for \|1\> and \|1\>. Hadamard is its own inverse, but generally one must use the adjoint of the operation.

#### Codercise I.9.1 - Measuring a Superposition

In addition to qml.state() allowing us to check a qubit’s state, we can measure the probabilities via qml.probs(wires=…).

Write a simple circuit that applies a Hadamard gate to either \|0\> or \|1\> and returns the measurement outcome probabilities.

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_h_and_measure(state):
    """Complete the function such that we apply the Hadamard gate
    and measure in the computational basis.

    Args:
        state (int): Either 0 or 1. If 1, prepare the qubit in state |1>,
            otherwise leave it in state 0.

    Returns:
        np.array[float]: The measurement outcome probabilities.
    """
    if state == 1:
        qml.PauliX(wires=0)

    ##################
    # YOUR CODE HERE #
    ##################
    qml.H(wires=0)
    myprobs=qml.probs(wires=0)

    # APPLY HADAMARD AND MEASURE

    return myprobs

print(apply_h_and_measure(0))
print(apply_h_and_measure(1))

```

#### Codercise I.9.2 - Y Basis Rotation

Suppose we have prepared the state $`|\psi\rangle=\frac{1}{2}|0\rangle+i\frac{\sqrt{3}}{2}|1\rangle`$, and want to make a measurement in the basis

$`|y_+\rangle=\frac{1}{\sqrt{2}}(|0\rangle+i|1\rangle),|y_-\rangle=\frac{1}{\sqrt{2}}(|0\rangle-i|1\rangle)`$

First, implement a quantum function prepare_psi that prepares the state $`|\psi\rangle`$.

- We’ll use qml.StatePrep for this, with the input array of \[0.5, np.sqrt(3)\*0.5j\]
Then, determine how to prepare the two basis states y+ and y- from 0 and 1, respectively, implemented as a second quantum function, y_basis_rotation.

- Note that \|y+\> and \|y→ are the eigenvectors of the Pauli Y operation
- Note that we can’t just rotate around y, that would give us a different set of vectors. Rotating around the x axis 45 degrees (pi/2) should do what we need. Or, more simply, HS, the textbook Clifford basis rotation

```python
##################
# YOUR CODE HERE #
##################

# WRITE A QUANTUM FUNCTION THAT PREPARES (1/2)|0> + i(sqrt(3)/2)|1>
def prepare_psi():
    v=np.array([0.5,np.sqrt(3)*0.5j])
    qml.StatePrep(v,wires=0)
    pass

# WRITE A QUANTUM FUNCTION THAT SENDS BOTH |0> TO |y_+> and |1> TO |y_->

def y_basis_rotation():
    qml.H(wires=0)
    qml.S(wires=0)
    pass

```

#### Codercise I.9.3 - Measurement in the Y basis

Using the functions from the previous exercise, perform the basis rotation and return the measurement outcome probabilities.

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def measure_in_y_basis():
    ##################
    # YOUR CODE HERE #
    ##################

    # PREPARE THE STATE
    prepare_psi()

    # PERFORM THE ROTATION BACK TO COMPUTATIONAL BASIS
    qml.adjoint(y_basis_rotation)()

    # RETURN THE MEASUREMENT OUTCOME PROBABILITIES    
    myprobs=qml.probs(wires=0)
    
    return myprobs

print(measure_in_y_basis())

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
