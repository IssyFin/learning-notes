# SQ: Single-Qubit Gates

_Quantum Computing_

[Course link](https://pennylane.ai/codebook/single-qubit-gates)


## Unit notes


<!-- notion-import -->
## Notes from Notion

_Copied from [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

---

### Chapter 2 Summary - Content

*Single-Qubit Gates*

---

<details>
<summary>Quantum Operations</summary>

| **Symbol** | **Name** | **Matrix** | **Operation** | **Description** | **Visual Representation** | PennyLane Representation |
|---|---|---|---|---|---|---|
| X | Pauli X | 0 1<br>1 0 | X\|0⟩ = \|1⟩<br>X\|1⟩ = \|0⟩ | “NOT” | ⊕, X | qml.PauliX(wires=int) |
| H | Hadamard | <span underline="true"> 1  </span>     1  1<br>√2     1 -1 | H\|0⟩ = (1/√2) (\|0⟩ + \|1⟩) = \|+⟩<br>H\|1⟩ = (1/√2)  (\|0⟩ - \|1⟩) = \|-⟩ | Uniform superposition of the two states \|0⟩ and \|1⟩ | H | qml.Hadamard(wires=int) |
| Z | Pauli Z |  | Z\|0⟩ = \|0⟩<br>Z\|1⟩ = -\|1⟩<br>Z\|+⟩ = (1/√2)  (\|0⟩ - \|1⟩) = \|-⟩<br>Z\|-⟩ =  (1/√2)  (\|0⟩ + \|1⟩) = \|+⟩ | Flips the \|1⟩ state, leaves the \|0⟩ state intact.<br>Produces a relative phase in other states. | Z |  |
| Rz | Z Rotation | e\^-\{iw/2\} 0<br>0    e\^\{iw/2\} | Rz(w) = a\|0\> + Be\^\{iw\} \|1\> | A more general form of the Pauli Z gate. <br>Given angle, w, rotates by w. | Rz | qml.Rz(w,wires=int) |
| S | Phase Gate | 1       0<br>0     e\^-\{iπ/2\} | S = a\|0\> + Be\^\{i π/2\} \|1\> | Rz where w=pi/2. Note that S† = SSS | S |  |
| T | Pi-Over-8 Gate | 1       0<br>0     e\^-\{iπ/4\} | S = a\|0\> + Be\^\{i π/4\} \|1\> | Rz where w=pi/4. Note that T†=TTTTTTT | T |  |
| Rx | X Rotation | cos(θ/2)     -isin(θ/2)<br>-isin(θ/2)    cos(θ/2) |  |  | Rx |  |
| Ry | Y Rotation | cos(θ/2)   -sin(θ/2)<br>sin(θ/2)    cos(θ/2) |  | Similar to Rx but without the complex component | Ry |  |

</details>

<details>
<summary>Visual Representations of Quantum Operations</summary>

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

</details>

<details>
<summary>Blochsphere</summary>

States corresponding to the Pauli Z Operator Eigenvectors:

- $`|0\rangle`$ corresponds to the top-most state (+z)
- $`|1\rangle`$ corresponds to the bottom-most state (-z)
States corresponding to the Pauli X Operator Eigenvectors:

- $`|+\rangle`$ corresponds to the front-most axis (+x)
- $`|-\rangle`$ corresponds to the back-most axis (-x)
States corresponding to the Pauli Y Operator Eigenvectors (which we will learn later):

- $`|y+\rangle`$ corresponds to the right-most axis (+y)
- $`|y-\rangle`$ corresponds to the left-most axis (-y)
*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

</details>

<details>
<summary>Universal Gate Sets</summary>

The most general form of a unitary matrix is: $`U(\phi,\theta,\omega)=\begin{pmatrix}e^{-i(\phi+\omega)/2}cos(\theta/2)&-e^{i(\phi+\omega)/2}sin(\theta/2)\\e^{-i(\phi+\omega)/2}sin(\theta/2)&e^{i(\phi+\omega)/2}cos(\theta/2)\end{pmatrix}`$

All single-bit qubit operations can be expressed as a combination of a subset of two types of rotations (Ry and Rz combo is most common)

</details>

<details>
<summary>Projective Measurement</summary>

How much does a given basis vector contribute to a given state?

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

We can rotate bases to measure in a different, easier-to-measure basis:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

</details>

### Chapter 2 Summary - Code

*Single-Qubit Gates*

---

<details>
<summary>Initializing a state</summary>

```python

    # CREATE THE |0> STATE
    state = np.array([1,0])
    
    #Apply Hadamard to get |+> from |0>
    qml.Hadamard(wires=0)
```

</details>

<details>
<summary>Use an adjoint (†, or negative angle in operation)</summary>

```python
qml.adjoint(qml.RZ)(omega,wires=0)
```

</details>

<details>
<summary>Initialize a state (example: |\psi\rangle = \frac{\sqrt{3}}{2}|0\rangle-\frac{i}{2}|1\rangle)</summary>

<details>
<summary>Long-way</summary>

<details>
<summary>Walkthrough</summary>

1. What are the coefficients, using Bloch-Sphere form? $`|\psi\rangle=cos(\frac{\theta}{2})|0\rangle+e^{i\phi}sin(\frac{\theta}{2})|1\rangle`$
$`cos(\frac{\theta}{2})=|\alpha|=\frac{\sqrt{3}}{2}

\\sin(\frac{\theta}{2})=|\beta|=\frac{1}{2}`$

Using the unit circle, we find that

for $`cos(\frac{\theta}{2})=\frac{\sqrt{3}}{2}`$ and $`sin(\frac{\theta}{2})=\frac{1}{2}`$, $`\angle=30\degree=\frac{\pi}{6}`$; $`\theta=\frac{\pi}{3}`$

2. Find the global phase
Since $`\alpha`$ is real and positive, we can use this as the phase.

3. Find the relative phase:
$`\beta=-\frac{i}{2}`$

Convert to polar form:

$`\beta=-\frac{i}{2}=\frac{1}{2}e^{-i\pi/2}`$

$`-\frac{\pi}{2}`$ is our relative phase

(ignore the 1/2 element - phase depends on direction, not magnitude)

4. Apply to a qubit state:
$`|\psi\rangle=Rz(\phi)Ry(\theta)|0\rangle=Rz(-\frac{\pi}{2})Ry(\frac{\pi}{3})|0\rangle`$

</details>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def prepare_state():
    ##################
    # YOUR CODE HERE #
    ##################
    qml.RY(np.pi/3,wires=0)
    qml.RZ(-np.pi/2,wires=0)

    return qml.state()
print(prepare_state())
```

</details>

<details>
<summary>Short way (using qml state prep)</summary>

```python
v = np.array([0.52889389 - 0.14956775j, 0.67262317 + 0.49545818j])

##################
# YOUR CODE HERE #
##################

# CREATE A DEVICE
dev = qml.device("default.qubit", wires=1)

# CONSTRUCT A QNODE THAT USES qml.MottonenStatePreparation
# TO PREPARE A QUBIT IN STATE V, AND RETURN THE STATE

@qml.qnode(dev)
def prepare_state(state=v):
    qml.StatePrep(v,wires=0)
    return qml.state()

# This will draw the quantum circuit and allow you to inspect the output gates
print(prepare_state(v))
print()
print(qml.draw(prepare_state, level="device")(v))

```

</details>

</details>

<details>
<summary>Visualize a quantum circuit and output gate observation</summary>

```python
v = np.array([0.52889389 - 0.14956775j, 0.67262317 + 0.49545818j])

# CREATE A DEVICE
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def prepare_state(state=v):
    qml.StatePrep(v,wires=0)
    return qml.state()

# This will draw the quantum circuit and allow you to inspect the output gates
print(qml.draw(prepare_state, level="device")(v))

```

</details>

<details>
<summary>Measure variance with numpy variance method</summary>

```python
def variance_experiment(n_shots):
    """Run an experiment to determine the variance in an expectation
    value computed with a given number of shots.

    Args:
        n_shots (int): The number of shots

    Returns:
        float: The variance in expectation value we obtain running the
        circuit 100 times with n_shots shots each.
    """

    # To obtain a variance, we run the circuit multiple times at each shot value.
    n_trials = 100

    # CREATE A DEVICE WITH GIVEN NUMBER OF SHOTS
    dev = qml.device("default.qubit", wires=1, shots=n_shots)
    

    @qml.qnode(dev)
    def circuit():
        qml.Hadamard(wires=0)
        return qml.expval(qml.PauliZ(wires=0))

    # RUN THE QNODE N_TRIALS TIMES AND RETURN THE VARIANCE OF THE RESULTS
    samples=[]
    
    for x in range(n_trials):
        samples.append(circuit())

    variance = np.var(samples)

    return variance
```

</details>

<details>
<summary>Get an expectation value</summary>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def circuit():
    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT THE CIRCUIT IN THE PICTURE AND MEASURE PAULI Y
    qml.RX(np.pi/4,wires=0)
    qml.H(wires=0)
    qml.PauliZ(wires=0)
    

    return qml.expval(qml.PauliY(wires=0))

print(circuit())
```

</details>

---

## Lessons

1. [X and H](./01-x-and-h/)
2. [It's Just a Phase](./02-its-just-a-phase/)
3. [From a Different Angle](./03-from-a-different-angle/)
4. [Universal Gate Sets](./04-universal-gate-sets/)
5. [Prepare Yourself](./05-prepare-yourself/)
6. [Measurements](./06-measurements/)
7. [What Did You Expect?](./07-what-did-you-expect/)
