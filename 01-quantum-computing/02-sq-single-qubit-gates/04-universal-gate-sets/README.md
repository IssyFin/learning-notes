# Universal Gate Sets

_Quantum Computing › Single-Qubit Gates_

[Course link](https://pennylane.ai/codebook/single-qubit-gates/universal-gate-sets)

<!-- notion-import -->
## Notes from Notion

_Copied from **Universal gate sets (build all possible single-qubit gates from a subset)** in [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

---

The most general form of a unitary matrix is the following:

$$
U(\phi,\theta,\omega)=\begin{pmatrix}e^{-i(\phi+\omega)/2}cos(\theta/2)&-e^{i(\phi+\omega)/2}sin(\theta/2)\\e^{-i(\phi+\omega)/2}sin(\theta/2)&e^{i(\phi+\omega)/2}cos(\theta/2)\end{pmatrix}
$$

However, we can simplify it using RX, RY, and RZ:

### Exercise I.7.1

Prove that U can be expressed using only 3 gates from the set \{RY, RZ\}:

Reminder:

$$
RX(\theta)=e^{-i\theta X/2}\\
RY(\theta)=e^{-i\theta Y/2}\\
RZ(\theta)=e^{-i\theta Z/2}\\
RY(\theta)=\begin{pmatrix}cos(\frac{\theta}{2})&-sin(\frac{\theta}{2})\\sin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}\\
RZ(\theta)=\begin{pmatrix}e^{-i \frac{\theta}{2}}&0\\0&e^{i \frac{\theta}{2}}\end{pmatrix}
$$

Knowing that we’re using 3 gates, and that RZRZ is same as RZ(twice angle) (also for RY), they must alternate:

Options are RZ,RY,RZ or RY,RZ,RY. Examining the given matrix, we need $`RY(\theta), RZ(\omega), R(\phi)`$. By matrix multiplication, we see this holds:

$$
RZ(\omega)RY(\theta)RZ(\phi)=\begin{pmatrix}cos(\frac{\omega}{2})&-sin(\frac{\omega}{2})\\sin(\frac{\omega}{2})&cos(\frac{\omega}{2})\end{pmatrix}\begin{pmatrix}e^{-i \frac{\theta}{2}}&0\\0&e^{i \frac{\theta}{2}}\end{pmatrix}\begin{pmatrix}cos(\frac{\phi}{2})&-sin(\frac{\phi}{2})\\sin(\frac{\phi}{2})&cos(\frac{\phi}{2})\end{pmatrix}\\
=\begin{pmatrix}e^{-i(\phi+\omega)/2}cos(\theta/2)&-e^{i(\phi+\omega)/2}sin(\theta/2)\\e^{-i(\phi+\omega)/2}sin(\theta/2)&e^{i(\phi+\omega)/2}cos(\theta/2)\end{pmatrix}
$$

### Exercise I.7.2

Prove that U can be expressed using only 3 gates from the set \{RX, RZ\}:

$$
U(\phi,\theta,\omega)=\begin{pmatrix}e^{-i(\phi+\omega)/2}cos(\theta/2)&-e^{i(\phi-\omega)/2}sin(\theta/2)\\e^{-i(\phi-\omega)/2}sin(\theta/2)&e^{i(\phi+\omega)/2}cos(\theta/2)\end{pmatrix}
$$

Since we already know how to express the matrix in terms of RZ and RY, we just have to remember that Y and X can be related via $`Y=SXS^\dagger`$:

$$
RY(\theta)=e^{-i\frac{\theta}{2}Y}=cos(\frac{\theta}{2}I)-i sin(\frac{\theta}{2}Y)\\
RY(\theta)=cos(\frac{theta}{2}I)-isin(\frac{\theta}{2}SXS^\dagger)\\
=S(cos\frac{\theta}{2}I-isin\frac{\theta}{2}X)S^\dagger\\
=SRX(\theta)S^\dagger
$$

Since $`S=RZ(\frac{\pi}{2})`$, recover U by:

$$
U(\phi,\theta,\omega)=RZ(\omega)RY(\theta)RZ(\phi)\\
=RZ(\omega)RZ(\frac{\pi}{2})RX(\theta)RZ(-\frac{\pi}{2})RZ(\phi)\\
=RZ(\omega+\frac{\pi}{2})RX(\theta)RZ(\phi-\frac{\pi}{2})
$$

As a bonus, be aware that **there are 12 different ways to decompose an arbitrary single-qubit operation into a sequence of 3 RX, RY, RZ gates** (combo of RZ and RY is most commonly encountered).

With **just two types of rotations, we can implement *****any *****single-qubit unitary operation**! HW only needs to be able to do 2 things well, rather than implement a lot of different things. Any two rotations from \{RX,RY, RZ\} form a universal gate set.

For any $`U`$, we can find a $`V`$ composed only of gates from the universal set such that $`||U-V||\le \epsilon`$. Finding this sequence of gates making up $`V`$ is called **quantum circuit synthesis.**

For RZ and RZ, or RZ and RY, precision is taken for granted since we’re just rotating. For other gate sets not containing parameterized rotations (eg $`\{H,T\}`$), you can approximate it up to an arbitrary precision (sequence may be enormous, but the particularly precise approximation can still be found.

### Codercise I.7.1: Universality of Rotations

Remember that the most general single-qubit unitary, a rotation, is implemented in PennyLane as qml.Rot. It actually applies a sequence of three operations:

```javascript
def decomposed_rot(phi,theta,omega):
	qml.RZ(phi,wires=0)
	qml.RY(theta,wires=0)
	qml.RZ(omega,wires=0)
```
Under the hood, it’s just RZ and RY gates. Together, they form a universal gate set for single-qubit operations (as do RZ and RX, or RY and RX)

---

Find a set of angles phi, theta, and omega st the sequence of gates

```javascript
	qml.RZ(phi,wires=0)
	qml.RX(theta,wires=0)
	qml.RZ(omega,wires=0)
```
acts the same as a Hadamard gate (up to a global phase).

Hint: Matrix forms for H and RX are below. Start by identifying the angle of RX for the magnitude of the elements, then use RZ to adjust signs to give H up to a global phase.

$$
H=\frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}\\
RX(\theta)=\begin{pmatrix}cos(\frac{\theta}{2})&-isin(\frac{\theta}{2})\\-isin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}\\
RY(\theta)=\begin{pmatrix}cos(\frac{\theta}{2})&-sin(\frac{\theta}{2})\\sin(\frac{\theta}{2})&cos(\frac{\theta}{2})\end{pmatrix}\\
$$

---

To start, note that we want a magnitude of $`\frac{1}{\sqrt{2}}`$. To do this, we set $`\theta`$ to $`45\degree`$, or $`\frac{\pi}{2}`$.

Continuing on, examine RX and RZ:

$$
RZ(\phi)=\begin{pmatrix}e^{-i\phi/2}&0\\0&e^{i\phi/2}\end{pmatrix}\\
RX(\frac{\pi}{2})=\begin{pmatrix}\frac{1}{\sqrt{2}}&\frac{-i}{\sqrt{2}}\\\frac{-i}{\sqrt{2}}&\frac{1}{\sqrt{2}}\end{pmatrix}
$$

$$
RZ(\phi)RX(\frac{\pi}{2})RZ(\omega)=\begin{pmatrix}e^{-i\phi/2}&0\\0&e^{i\phi/2}\end{pmatrix}\begin{pmatrix}\frac{1}{\sqrt{2}}&\frac{-i}{\sqrt{2}}\\\frac{-i}{\sqrt{2}}&\frac{1}{\sqrt{2}}\end{pmatrix}\begin{pmatrix}e^{-i\omega/2}&0\\0&e^{i\omega/2}\end{pmatrix}\\
=\begin{pmatrix}\frac{e^{-i\phi/2}}{\sqrt{2}}&-i\frac{e^{-i\phi/2}}{\sqrt{2}}\\-i\frac{e^{-i\phi/2}}{\sqrt{2}}&\frac{e^{-i\phi/2}}{\sqrt{2}}\end{pmatrix}\begin{pmatrix}e^{-i\omega/2}&0\\0&e^{i\omega/2}\end{pmatrix}\\
=\begin{pmatrix}\frac{e^{-i(\phi+\omega)/2}}{\sqrt{2}}
&
-i\frac{e^{-i(\phi-\omega)/2}}{\sqrt{2}}
\\
-i\frac{e^{-i(\phi-\omega)/2}}{\sqrt{2}}
&
\frac{e^{-i(\phi+\omega)/2}}{\sqrt{2}}\end{pmatrix}
$$

To get a result of $`\frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}`$, pull out the $`\frac{1}{\sqrt{2}}`$ and solve. Here, we see that when $`\omega=\phi`$, things are simplified:

$$
\frac{1}{\sqrt{2}}\begin{pmatrix}e^{-i(\phi+\omega)/2}
&
-ie^{-i(\phi-\omega)/2}
\\
-ie^{i(\phi-\omega)/2}
&
e^{i(\phi+\omega)/2}\end{pmatrix}\\
=\frac{1}{\sqrt{2}}\begin{pmatrix}e^{-i\alpha}&-ie^{0}\\-ie^{0}&e^{i\alpha}\end{pmatrix}
$$

When $`\omega=\phi=\frac{\pi}{2}`$, this gives us the matrix we are expecting.

<details>
<summary>My Code</summary>

```javascript
dev = qml.device("default.qubit", wires=1)

##################
# YOUR CODE HERE #
##################

# ADJUST THE VALUES OF PHI, THETA, AND OMEGA
phi, theta, omega = np.pi/2, np.pi/2, np.pi/2

@qml.qnode(dev)
def hadamard_with_rz_rx():
    qml.RZ(phi, wires=0)
    qml.RX(theta, wires=0)
    qml.RZ(omega, wires=0)
    return qml.state()

```

</details>

### Codercise I.7.2: Synthesizing a Circuit

Rewrite the following circuit over the gate set \[RZ,RX\] (recall that it’s okay for the circuit to work up to a global phase). What is the minimum number of such gates needed to do so?

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

Remember that $`H=RZ(\frac{\pi}{2})RX(\frac{\pi}{2})RZ(\frac{\pi}{2})`$, that $`S=RZ(\frac{\pi}{2})`$, that $`T=RZ(\frac{\pi}{4})`$, and that $`Y=S^\dagger X S`$.

Remember also that $`T^\dagger=TTTTTTT`$ and $`S^\dagger=SSS`$

Remember also that we can use <br>    qml.adjoint(qml.T)(wires=0) to take $`T^\dagger`$, and the same for $`S^\dagger`$.

<details>
<summary>My Code</summary>

```javascript
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def convert_to_rz_rx():
    ##################
    # YOUR CODE HERE #
    ##################

    # IMPLEMENT THE CIRCUIT IN THE PICTURE USING ONLY RZ AND RX
    # Note that, to save lines, we'll combine "touching" lines 
    # (eg, qml.RZ(np.pi/2) 2 times in a row will be commented out and replaced)

    # Apply H
    qml.RZ(np.pi/2, wires=0)
    qml.RX(np.pi/2, wires=0)
    #qml.RZ(np.pi/2)

    # Apply S
    #qml.RZ(np.pi/2)

    # Apply T dagger
    # qml.RZ(7*np.pi/4)

    # Apply Y
    # qml.RZ(3*np.pi/2)
    qml.RZ(17*np.pi/4, wires=0)
    qml.RX(np.pi/2, wires=0)
    qml.RZ(np.pi/2, wires=0)

    return qml.state()

```

</details>

### Codercise I.7.3: Universality of H and T

Note that H and T are also a universal gate set. By combining just H and T, we can approximate to arbitrary precision any single-qubit operation (just like we can do with RZ and RY).

Write a PennyLane circuit that applies the Unitary matrix below using a total of 6 H and T gates.

$$
U=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1+e^{i\pi/4}+i(1-e^{i\pi/4})&1-e^{i\pi/4}+i(1+e^{i\pi/4})\\1+e^{i\pi/4}-i(1-e^{i\pi/4})&1-e^{i\pi/4}-i(1+e^{i\pi/4})\end{pmatrix}
$$

This process is called quantum circuit synthesis and is part of the broader subject of quantum compilation. Designing high-quality, automated compilation tools is an active area of research.

---

Hints:

$$
T=\begin{pmatrix}1&0\\0&e^{i\pi/4}\end{pmatrix}\\
H=\frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}
$$

Remember that Hadamard is its own inverse; limits the ordering of how it can be applied in sequence; will not have $`HH^\dagger`$ or $`H^\dagger H`$

Remember also that the common denominator $`\frac{1}{\sqrt{2}^3}`$ gives us information on how many times Hadamards are applied. The largest cumulative phase will give an idea as to how many Ts are necessary

---

To start out, it’s probably safe to say we’re applying Hadamard 3 times. Having the inverses on hand will be helpful, too:

$$
T^\dagger=\begin{pmatrix}1&0\\0&e^{-i\pi/4}\end{pmatrix}\\
H^\dagger=H=\frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}
$$

---

Let’s try a symmetrical set to start: HTHTHT

$$
HT=\frac{1}{\sqrt{2}}\begin{pmatrix}1&e^{i\pi/4}\\1&-e^{i\pi/4}\end{pmatrix}\\

HTHTHT=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1&e^{i\pi/4}\\1&-e^{i\pi/4}\end{pmatrix}\begin{pmatrix}1&e^{i\pi/4}\\1&-e^{i\pi/4}\end{pmatrix}\begin{pmatrix}1&e^{i\pi/4}\\1&-e^{i\pi/4}\end{pmatrix}\\

HTHTHT=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1-e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}\\1-e^{i\pi/4}&e^{i\pi/4}+e^{i\pi/2}\end{pmatrix}\begin{pmatrix}1&e^{i\pi/4}\\1&-e^{i\pi/4}\end{pmatrix}\\
HTHTHT=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1-e^{i\pi/2}+e^{i\pi/4}-e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}-e^{i\pi/2}+e^{3i\pi/4}\\1-e^{i\pi/4}+e^{i\pi/4}+e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}+e^{i\pi/2}-e^{3i\pi/4}\end{pmatrix}\\

HTHTHT=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1-2e^{i\pi/2}+e^{i\pi/4}
&
e^{i\pi/4}-2e^{i\pi/2}+e^{3i\pi/4}
\\
1+e^{i\pi/2}
&
e^{i\pi/4}-e^{3i\pi/4}\end{pmatrix}\\
$$

This doesn’t seem to be the right matrix. The only other combinations (since H has to be separated) are THTHTH, HTTHTH, and HTHTTH: Let’s try another. Note that, since we have HTHT calculated already as $`HTHT=\frac{1}{\sqrt{2}^2}\begin{pmatrix}1-e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}\\1-e^{i\pi/4}&e^{i\pi/4}+e^{i\pi/2}\end{pmatrix}`$, we can simplify the process.

We can also note that the largest cumulative phase is $`e^{i\pi/4}`$, so it’s unlikely we’ll have two Ts immediately adjacent. For that reason, we’ll try THTHTH:

$$
T(HTHT)=\frac{1}{\sqrt{2}^2}\begin{pmatrix}1&0\\0&e^{i\pi/4}\end{pmatrix}\begin{pmatrix}1-e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}\\1-e^{i\pi/4}&e^{i\pi/4}+e^{i\pi/2}\end{pmatrix}\\
=\frac{1}{\sqrt{2}^2}\begin{pmatrix}1-e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}\\e^{i\pi/4}-e^{i\pi/2}&e^{i\pi/2}+e^{3i\pi/4}\end{pmatrix}\\
THTHTH = \frac{1}{\sqrt{2}^3}\\\begin{pmatrix}1-e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}\\e^{i\pi/4}-e^{i\pi/2}&e^{i\pi/2}+e^{3i\pi/4}\end{pmatrix}\begin{pmatrix}1&1\\1&-1\end{pmatrix}\\
\frac{1}{\sqrt{2}^3}
\begin{pmatrix}\end{pmatrix}
$$

The phase still seems to be too big, so we’ll discontinue the calculation and try one of the others:

HTHTTH:

$$
HTHTTH=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1-e^{i\pi/2}&e^{i\pi/4}-e^{i\pi/2}\\1-e^{i\pi/4}&e^{i\pi/4}+e^{i\pi/2}\end{pmatrix}\begin{pmatrix}1&0\\0&e^{i\pi/4}\end{pmatrix}\begin{pmatrix}1&1\\1&-1\end{pmatrix}\\
HTHTTH=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1-e^{i\pi/2}&e^{i\pi/2}-e^{3i\pi/4}\\1-e^{i\pi/4}&e^{i\pi/2}+e^{3i\pi/4}\end{pmatrix}\begin{pmatrix}1&1\\1&-1\end{pmatrix}\\

HTHTTH=\frac{1}{\sqrt{2}^3}\begin{pmatrix}1-e^{3i\pi/4}&1-e^{i\pi}+e^{3i\pi/4}\\1+e^{3i\pi/4}+e^{i\pi/2}-e^{i\pi/4}&1-e^{i\pi/4}-e^{i\pi/2}-e^{3i\pi/4}\end{pmatrix}
$$

Note- my math is wrong, but this one seems to do the trick! Implementing it in code is straightforward:

<details>
<summary>My Code:</summary>

```javascript
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def unitary_with_h_and_t():
    ##################
    # YOUR CODE HERE #
    ##################
    qml.H(wires=0)
    qml.T(wires=0)
    qml.H(wires=0)
    qml.T(wires=0)
    qml.T(wires=0)
    qml.H(wires=0)
    # APPLY ONLY H AND T TO PRODUCE A CIRCUIT THAT EFFECTS THE GIVEN MATRIX

    return qml.state()

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
