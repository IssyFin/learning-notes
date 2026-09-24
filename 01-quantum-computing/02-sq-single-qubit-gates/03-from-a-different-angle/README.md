# From a Different Angle

_Quantum Computing › Single-Qubit Gates_

[Course link](https://pennylane.ai/codebook/single-qubit-gates/from-a-different-angle)

<!-- notion-import -->
## Notes from Notion

_Copied from **From a different angle (qubit rotation about x and y axes)** in [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

### The Bloch Sphere

Qubits act interestingly in 3-dimensional space.

In 2d space, the most general form of a qubit is $`|\psi\rangle=a|0\rangle+be^{i\phi}|1\rangle`$ (where a and b are too real numbers), with normalization requirements meaning that $`a^2+b^2=1`$. We can further see the relationship via trigonometric functions since $`a=cos(\theta/2)`$ and $`b=sin(\theta/2)`$:

$$
|\psi\rangle=cos(\frac{\theta}{2})|0\rangle+sin(\frac{\theta}{2})e^{i\phi}|1\rangle
$$

In 2d, we have a single-qubit state parameterized by 2 angles: $`\theta`$ and $`\phi`$, with length 1. We can therefore make associations between qubit states and unit vectors in 3-dimensional spaces expressed in spherical coordinates.

However, we have not 2, but 3 axes to rotate around: Z, X, and Y.

This can be tricky to visualize, so the **Bloch sphere** comes in handy here. Each qubit state vector corresponds to a *real* vector on the surface of the sphere in a 3d space:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

In addition to the 3 axes we’re familiar with (x, y, and z), we have other corresponding states:

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

Visually, RX, RY, and RZ rotate the qubit’s state vector about the appropriate axis:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

Simply put, $`RX(\pi)`$ sends the state to the opposite x direction, $`RY(\pi)`$ to the opposite Y direction, and $`RZ(\pi)`$ to the opposite Z direction.

### RX and RY

Because these RX and RY operators are essentially just rotations, we can express them as matrices:

$$
RX(\theta)=\begin{pmatrix}cos(\frac{\theta}{2})&-isin(\frac{\theta}{2})\\-isin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}
$$

#### Exercise I.6.1

Determine $`\theta`$ for which $`RX(\theta)=X`$ (up to a global phase)

This is easier to interpret visually; $`\theta`$ will simply be angles where $`cos(\theta/2)=0,sin(\theta/2)=1.`$ This is satisfied by the angle $`\theta=\pi`$. Though there is a global phase of -i present, global phases have no effect and can safely be removed:

$$
RX(\pi)=\begin{pmatrix}0&-i\\-i&0\end{pmatrix}\\
RX=-iX
$$

The matrix representation of a Y rotation is similar, just without a complex Y component:

$$
RY(\theta)=\begin{pmatrix}cos(\frac{\theta}{2})&-sin(\frac{\theta}{2})\\sin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}
$$

#### Exercise I.6.2

Just like how Pauli X and Z are special cases of RX and RZ, Pauli Y is the special case of $`RY(\theta)`$ when $`\theta=\pi`$. Typically, it is written as:

$$
Y=\begin{pmatrix}0&-i\\i&0\end{pmatrix}
$$

Furthermore, there is a nice relationship between X, Y, and Z.

Show that $`Y=iXZ=iRY(\pi)`$

For $`Y=iXZ:`$

$$
Y=\begin{pmatrix}0&-i\\i&0\end{pmatrix}\\
X=\begin{pmatrix}0&1\\1&0\end{pmatrix}\\
Z=\begin{pmatrix}1&0\\0&-1\end{pmatrix}\\
iXY=i\begin{pmatrix}0&1\\1&0\end{pmatrix}\begin{pmatrix}1&0\\0&-1\end{pmatrix}\\
iXY=i\begin{pmatrix}0&-1\\1&0\end{pmatrix}=\begin{pmatrix}0&-i\\i&0\end{pmatrix} \checkmark
$$

For $`Y=iRY(\pi)`$

$$
Y=iRY(\pi)=i\begin{pmatrix}cos(\frac{\pi}{2})&-sin(\frac{\pi}{2})\\sin(\frac{\pi}{2})&cos(\frac{\pi}{2})\end{pmatrix}\\
Y=iRY(\pi)=i\begin{pmatrix}0&-1\\1&0\end{pmatrix}=\begin{pmatrix}0&-i\\i&0\end{pmatrix}\checkmark
$$

#### Exercise I.6.3

Evaluate the action of $`RX(\theta)`$ and $`RY(\theta)`$ on $`|0\rangle`$ and $`|1\rangle`$. Express results as linear combinations of those computational basis states.

$$
|0\rangle=\begin{pmatrix}1\\0\end{pmatrix},|1\rangle=\begin{pmatrix}0\\1\end{pmatrix}\\

RX(\theta)|0\rangle=\begin{pmatrix}cos(\frac{\theta}{2})\\-isin(\frac{\theta}{2})\end{pmatrix}=cos(\frac{\theta}{2})|0\rangle-isin(\frac{\theta}{2})|1\rangle\\

RX(\theta)|1\rangle=\begin{pmatrix}-isin(\frac{\theta}{2})\\cos(\frac{\theta}{2})\end{pmatrix}=cos(\frac{\theta}{2})|1\rangle-isin(\frac{\theta}{2})|0\rangle\\

RY(\theta)|0\rangle=\begin{pmatrix}cos(\frac{\theta}{2})\\sin(\frac{\theta}{2})\end{pmatrix}=cos(\frac{\theta}{2})|0\rangle+sin(\frac{\theta}{2})|1\rangle\\

RY(\theta)|1\rangle=\begin{pmatrix}-sin(\frac{\theta}{2})\\cos(\frac{\theta}{2})\end{pmatrix}=cos(\frac{\theta}{2})|1\rangle-sin(\frac{\theta}{2})|0\rangle\\
$$

#### Exercise I.6.4

There are many other relationships you can derive between these three types of rotation. Such relationships are often useful for simplifying sequences of quantum operations in circuits. For exxample, what is the result of applying X before and after an application of $`RY(\theta)`$?

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

Since the X gate is essentially a bit flip, bit flipping before and after $`RY(\theta)`$ will essentially rotate the state in the other direction (eg $`RY(-\theta)`$):

$$

$$

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

#### Exercise I.6.5

Show that RX, RY, and RZ can be represented as follows:

$$
RX(\theta)=e^{-i\theta X/2}\\
RY(\theta)=e^{-i\theta Y/2}\\
RZ(\theta)=e^{-i\theta Z/2}
$$

Note that these representations are important for Hamiltonian Simulation.

$$
RX(\theta)=\begin{pmatrix}cos(\frac{\theta}{2})&-isin(\frac{\theta}{2})\\-isin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}\\
e^{-i\theta X/2}=I+(-i\frac{\theta}{2}X)+\frac{1}{2!}(-i\frac{\theta}{2}X)^2+\frac{1}{3!}(-i\frac{\theta}{2}X)^3+\frac{1}{4!}(-i\frac{\theta}{2}X)^4+...\\
=I-i\frac{\theta}{2}X-\frac{1}{2}(\frac{\theta}{2})^2X^2+\frac{1}{3!}i(\frac{\theta}{2})^3X^3+\frac{1}{4!}(\frac{\theta}{2})^4X^4+...
$$

Applying X more than once flips the bit, bringing us back to where we started…

$$
=(1-\frac{1}{2}(\frac{\theta}{2})^2+\frac{1}{4!}(\frac{\theta}{2})^4)I-i((\frac{\theta}{2}-\frac{1}{3!}(\frac{\theta}{2})^3+...)X
$$

Since the expressions in parentheses are just expansions of sine and cosine, we can write in closed form and recover the matrix expression:

$$
=cos(\frac{\theta}{2})I-isin(\frac{\theta}{2})X\\
=cos(\frac{\theta}{2})\begin{pmatrix}1&0\\0&1\end{pmatrix}-isin(\frac{\theta}{2})\begin{pmatrix}0&1\\1&0\end{pmatrix}\\
=\begin{pmatrix}cos(\frac{\theta}{2})&-isin(\frac{\theta}{2})\\-isin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}
$$

#### Codercise I.6.1-Applying RX

In addition to the RZ, we also have RX and RY rotations. These parametric gates are available in Pennylane as qml.RX and qml.RY.

Write a QNode that applies qml.RX with an angle of $`\pi`$ to one of the computational basis states. What operation is this?

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_rx_pi(state):
    """Apply an RX gate with an angle of \pi to a particular basis state.

    Args:
        state (int): Either 0 or 1. If 1, initialize the qubit to state |1>
            before applying other operations.

    Returns:
        np.array[complex]: The state of the qubit after the operations.
    """
    if state == 1:
        qml.PauliX(wires=0)

    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY RX(pi) AND RETURN THE STATE
    qml.RX(3.1415926535,wires=0)

    return qml.state()

print(apply_rx_pi(0))
print(apply_rx_pi(1))

```

</details>

#### Codercise I.6.2-Plotting RX

In the previous exercise, we should have noticed that, for this special case $`RX(\pi)=X`$ up to a global phase of -i. But what does RX do more generally?

The matrix representation of RX is

$$
RX(\theta)=\begin{pmatrix}cos(\frac{\theta}{2})&-isin(\frac{\theta}{2})\\-isin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}\\
$$

How does this affect the amplitudes when we apply it to a quantum state? Implement a QNode that applies the qml.RX operation with parameter $`\theta`$ to a specified basis state. Then, run the code to plot the amplitudes of the $`|0\rangle`$ and $`|1\rangle`$ after applying $`RX(\theta)`$ to the $`|0\rangle`$ state.

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_rx(theta, state):
    """Apply an RX gate with an angle of theta to a particular basis state.

    Args:
        theta (float): A rotation angle.
        state (int): Either 0 or 1. If 1, initialize the qubit to state |1>
            before applying other operations.

    Returns:
        np.array[complex]: The state of the qubit after the operations.
    """
    if state == 1:
        qml.PauliX(wires=0)

    ##################
    # YOUR CODE HERE #
    ##################
    qml.RX(theta,wires=0)

    # APPLY RX(theta) AND RETURN THE STATE

    return qml.state()

# Code for plotting
angles = np.linspace(0, 4 * np.pi, 200)
output_states = np.array([apply_rx(t, 0) for t in angles])

plot = plotter(angles, output_states)

```
*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

</details>

#### Codercise I.6.3-Plotting RY

Repeat the above exercise, but using qml.RY. From the amplitudes you obtain for $`RY(\theta)|0\rangle`$, can you start deducing the matrix form of RY?

<details>
<summary>My Code</summary>

```python
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def apply_ry(theta, state):
    """Apply an RY gate with an angle of theta to a particular basis state.

    Args:
        theta (float): A rotation angle.
        state (int): Either 0 or 1. If 1, initialize the qubit to state |1>
            before applying other operations.

    Returns:
        np.array[complex]: The state of the qubit after the operations.
    """
    if state == 1:
        qml.PauliX(wires=0)

    ##################
    # YOUR CODE HERE #
    ##################

    # APPLY RY(theta) AND RETURN THE STATE
    qml.RY(theta,wires=0)

    return qml.state()

# Code for plotting
angles = np.linspace(0, 4 * np.pi, 200)
output_states = np.array([apply_ry(t, 0) for t in angles])

plot = plotter(angles, output_states)

```
*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

</details>

Although the real and imaginary components of $`|0\rangle`$ don’t seem to change, rotating around the Y axis (RY) adjusts the real component of $`|1\rangle`$and rotating around the X axis (RY) adjusts the imaginary component of $`|1\rangle`$.

---


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
