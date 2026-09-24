# Prepare Yourself

_Quantum Computing › Single-Qubit Gates_

[Course link](https://pennylane.ai/codebook/single-qubit-gates/prepare-yourself)

<!-- notion-import -->
## Notes from Notion

_Copied from **Prepare yourself (Review)** in [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

### Codercise 8.1.1 - State Preparation

Now that you've learned all about single-qubit gates, you have the tools to<br>perform arbitrary **quantum state preparation** of a single qubit!<br>State preparation takes place at the start of many algorithms. Given a target state<br>we would like the qubit to be in, we need to figure out the sequence<br>of operations that, acting on   produces the desired state. Furthermore,<br>we ideally want this sequence of operations to be as small as possible.

For this codercise, you will be asked to write a circuit that prepares the quantum state below up to a global phase using as few gates as possible.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

<details>
<summary>My Code</summary>

```javascript
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def prepare_state():
    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY OPERATIONS TO PREPARE THE TARGET STATE
    qml.H(wires=0)
    qml.T(wires=0)
    qml.T(wires=0)
    qml.T(wires=0)
    qml.T(wires=0)
    qml.T(wires=0)
    
    return qml.state()

```

</details>

### Codercise 8.1.2 - State Preparation Revisited

Let's have another try. Write a circuit that prepares the quantum state below up to a global phase using as few gates as possible.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

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

<details>
<summary>My Code</summary>

```javascript
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

### Codercise 8.1.3 - State preparation with Mottonen’s method

PennyLane contains a library of templates, some of which perform state preparation. These templates can be used like other gates.

StatePrep automatically prepares any normalized qubit state vector up to a global phase, and accepts a normalized state vector and a set of wires.

Use qml.StatePrep to prepare the state below, and return the state of the system. qml.draw will then show which operations were used in the creation of the new state.

$`|v\rangle=(0.52889389-0.14956775i)|0\rangle+(0.67262317+0.49545818i)|1\rangle`$

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
*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

---


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
