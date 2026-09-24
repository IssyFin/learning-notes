# All About Qubits

_Quantum Computing › Introduction to Quantum Computing_

[Course link](https://pennylane.ai/codebook/introduction-to-quantum-computing/all-about-qubits)

<!-- notion-import -->
## Notes from Notion

_Copied from **[All about qubits](https://pennylane.ai/codebook/introduction-to-quantum-computing/all-about-qubits)** in [🔆 IQC (introduction to quantum computing)](https://app.notion.com/p/2b544de2a656800ab0a3fe5708654ea3)._

### Notion of qubit

- Classical computers use binary values
- Qubits are the quantum bits used by quantum computers
  - Rely on mathematical representation of *state*, a way to measure the qubit to determine current state, and a way to manipulate the state to perform computation

### States

- State: A column vector of two elements
  - Most basic: analogues of a bit’s “0” and “1” state:

$$
|0〉 = \begin{pmatrix}
1\\
0\\
\end{pmatrix}
, |1〉=
\begin{pmatrix}
0\\
1\\
\end{pmatrix}
\\
|Ψ〉 = Arbitrary State
$$

- However, this way of writing it out can be tedious; instead, use Dirac notation or bra-ket notation (state vector is a ket, the notation for which is \|\* 〉**)**
  - What goes between the \| and \> is a label to donate a particular state.
  - Psi represents a qubit in an arbitrary state
  - For every ket (state), there is an associated bra (a row vector, or ket turned on its side)
    - Each element in the vector is the complex conjugate of the corresponding element in the ket

$$
⟨0| = (1,0) , ⟨1| = (0,1)

$$

- States  \|0〉and \|1〉are special because they form a basis (can write anything in the space as a linear combination of these basis vectors)
- Inner product between 2 qubit states is computed by taking the dot product between the bra of one and the ket of the other (they combine to form a “bra-ket” expression).
- Example for \|0〉and \|1: show orthogonality:

$$
⟨0||1〉= ⟨0|1〉= \\\begin{pmatrix}0 &1\end{pmatrix}•\begin{pmatrix}0\\1\end{pmatrix}\\=1•0 + 0•1\\
=0
$$

- When orthogonal, called the computational basis, the most commonly-used basis in which to express quantum states
- Computational bases also contain states that are normalized to have length 1. You can compute the length of a qubit state vector just like a regular 2-dimensional vector: simply compute the inner product with itself and take the square root

$$
\sqrt{⟨1|1〉} = \sqrt{\begin{pmatrix}0&1\end{pmatrix}•\begin{pmatrix}0\\1\end{pmatrix}}\\
=\sqrt{0•0+1•1}\\
=1
$$

- As a reminder, when a basis consists of two normalized, orthogonal vectors, it is called an orthonormal basis.

### Superposition

- More possible states than just \|0⟩ and \|1⟩ (those are just binary)
  - Superposition states exist between \|0⟩ and \|1⟩; the state of a qubit in superposition is a linear combination of the basis states:

$$
|\psi⟩ = \alpha|0⟩ + \beta|1⟩ = \begin{pmatrix}\alpha\\\beta\end{pmatrix}
$$

where α and β are complex numbers st

$$
\alpha\alpha^* + \beta\beta^* = 1
$$

  - α and β are considered amplitudes, or probability amplitudes here; convey information of the relative strength of \|0⟩ and \|1⟩ in the state.
- **Common misconception alert: A qubit is only ever *in *one state, never two at the same time. It’s just that sometimes, the state may be a linear combination of the basis states**
- Example: 2 states, psi and phi: would like to take the inner product between them (〈Φ\|Ψ⟩)

$$
|\psi⟩ = \alpha|0⟩ + \beta|1⟩\\
|\phi⟩ = \gamma|0⟩+\delta|1⟩
$$

- First, compute the bra of \|Φ⟩: Do so by taking the bra of each basis state one at a time, taking the *conjugate* of the amplitudes:

$$
|\phi⟩ = \gamma|0⟩+\delta|1⟩\\
〈\phi| = \gamma^*〈0| + \delta^*〈1| \\
〈\phi| = (\gamma^*\delta^*)
$$

- Note that, in more interesting quantum states, the inner products can tell us more about the overlap/similarity between two states. If 0, the states are orthogonal, and if 1, they are the same and the state is normalized.
- To compute the inner product of two superposition states, you can either:
- Write out the matrices:

$$
〈\phi|\psi⟩ = \begin{pmatrix}\gamma^*&\delta^*\end{pmatrix}\begin{pmatrix}\alpha\\\beta\end{pmatrix}
$$

- Work purely in bra-ket notation (linear inner product with expansion, such as with polynomials)

$$
〈\phi|\psi⟩ = (\gamma^*〈0|+\delta^*〈1|)• (\alpha|0⟩+\beta|1⟩)\\
\gamma^*\alpha〈0|0⟩ +\delta^*\alpha〈1|0⟩+\gamma^*\beta〈0|1⟩+\delta^*\beta〈1|1⟩\\
\gamma^*\alpha(1*1+0*0) + \sigma^*\alpha(0*1+1*0)+\gamma^*\beta(1*0+0*1)+\delta^*\beta(0*0+1*1)\\
\gamma^*\alpha(1) + \sigma^*\alpha(0)+\gamma^*\beta(0)+\delta^*\beta(1)\\

\boldsymbol{\gamma^*\alpha + \delta^*\beta}\\
$$

- Example: Verify that the superposition state below is normalized

$$
|\psi⟩=\frac{3}{5}|0⟩-\frac{4}{5}e^{\frac{\pi}{6}i}|1⟩\\

〈\psi|\psi⟩=(\frac{3}{5}〈0|-\frac{4}{5}e^{-\frac{\pi}{6}i}〈1|)(\frac{3}{5}|0⟩-\frac{4}{5}e^{\frac{\pi}{6}i}|1⟩)\\
=\frac{9}{25}+\frac{16}{25}\\
\textbf{=1}\checkmark
$$

- Note: because we’re using the complex conjugate, e\^(-pi\*i/6) \* e\^(+pi\*i/6) becomes -1.

### Measurement Outcome Probabilities

- At the beginning of an algorithm, we create qubit states and put them in superposition
- At the end of algorithm, we need to get info *from* the qubits via measurement
  - Measurement in quantum computing is probabilistic; can’t see if a qubit is in superposition, but can observe the qubit in either 0 or 1 basis state. Amplitudes α and β convey that information
    - Prob(measure and observe \|0⟩ ) = **\|α\|\^2** (Measurement outcome of 0)
    - Prob(measure and observe \|1⟩ ) = **\|β\|\^2 **(Measurement outcome of 1)
    - Note: notation \|•\|\^2 is called “mod squared”; a multiplication of a number by its complex conjugate
    - Since only 2 possible outcomes, these probabilities must sum to 1 for a valid qubit state (why quantum states must be normalized)
      - \|α\|\^2 + \|β\|\^2 = 1
  - By taking many measurements, we can estimate the outcome probabilities, and thus α and β
**Example**: Suppose you have a qubit in the state

$$
|\psi ⟩=\frac{1}{2} |0⟩-\frac{\sqrt{3}i}{2}|1 ⟩
$$

Is this state normalized? If so, what is the probability of observing the qubit in state \|1\> after measuring it?

1. Check to see if it’s normalized by multiplying it by its complex conjugate

$$
〈\psi|\psi ⟩ = \frac{1}{2}*\frac{1}{2}-\frac{\sqrt{3}i}{2}*\frac{\sqrt{3}i}{2}\\
〈\psi|\psi ⟩=\frac{1}{4}+\frac{3}{4}\\
〈\psi|\psi ⟩=1\checkmark
$$

2. If normalized, find \|β\|\^2

$$
\beta =-\frac{\sqrt{3}i}{2}\\
|\beta|^2 = -\frac{\sqrt{3}i}{2}*-\frac{\sqrt{3}i}{2}\\
|\beta|^2=\frac{3}{4}\\
Prob(\beta)=0.75\checkmark
$$

### Operations on qubit states

- States are vectors and can be modified by 2x2 matrices
  - Modified state \|Ψ⟩ is typically modeled as state \|Ψ\`⟩, with 2x2 matrix U
$`|\psi'>=U|\psi>`$

  - Note that matrix U must preserve the normalziation of the state; these are known as unitary matrices (defining property is UU† = I,
    -  † indicates taking the complex conjugate of all elements in the transpose of U
    - I is the identity matrix
-

Exercise: Suppose we have a qubit in the state

$$
|\psi⟩=\frac{1}{2}|0⟩-\frac{\sqrt{3}i}{2}|1⟩
$$

and we want to apply the operation

$$
U=\begin{pmatrix}0&i\\-i&0\end{pmatrix}
$$

Compute the state of the qubit after applying U. Then, compute the measurement outcome probabilities of 0 and 1 and verify that the state is normalized

1. Apply U and compute the state of the qubit

$$
|\psi'⟩=\begin{pmatrix}0&i\\-i&0\end{pmatrix}(\frac{1}{2}\begin{pmatrix}1\\0\end{pmatrix}-\frac{\sqrt{3}i}{2}\begin{pmatrix}0\\1\end{pmatrix})\\
|\psi'⟩=\begin{pmatrix}0&i\\-i&0\end{pmatrix}(\begin{pmatrix}\frac{1}{2}\\0\end{pmatrix}-\begin{pmatrix}0\\\frac{\sqrt{3}i}{2}\end{pmatrix})\\
|\psi'⟩=\begin{pmatrix}0&i\\-i&0\end{pmatrix}(\begin{pmatrix}\frac{1}{2}\\-\frac{\sqrt{3}i}{2}\end{pmatrix})\\

|\psi'⟩=\begin{pmatrix}\frac{\sqrt{3}}{2}\\\frac{-i}{2}\end{pmatrix}\\

|\psi'⟩=\frac{\sqrt{3}}{2}|0⟩-\frac{i}{2}|1⟩
$$

2. Compute the measurement outcome of 0

$$
Prob(0)=|\alpha|^2=\frac{\sqrt{3}}{2}*\frac{\sqrt{3}}{2}\\
|\alpha|^2=\frac{3}{4}
$$

3. Compute the measurement outcome of 1

$$
Prob(1)=|\beta|^2=(\frac{-i}{2})(\frac{i}{2})\\

|\beta|^2=\frac{1}{4}
$$

4. Verify that prob(0)+prob(1)=1

$$
|\alpha|^2+|\beta|^2=\frac{3}{4}+\frac{1}{4}=1\checkmark

$$

### Codercises

#### Codercise I.1.1-Normalization of quantum states

Unnormalized vector $`|\psi⟩=\alpha|0⟩+\beta|1⟩, |\alpha|^2+|\beta|^2 \neq 1`$

Goal: Turn into an equivalent, valid quantum state $`|\psi'⟩ = \alpha'|0⟩+\beta'|1⟩, |\alpha'|^2+|\beta'|^2=1`$

<details>
<summary>Code</summary>

```javascript
# Here are the vector representations of |0> and |1>, for convenience
ket_0 = np.array([1, 0])
ket_1 = np.array([0, 1])

def normalize_state(alpha, beta):
    """Compute a normalized quantum state given arbitrary amplitudes.

    Args:
        alpha (complex): The amplitude associated with the |0> state.
        beta (complex): The amplitude associated with the |1> state.

    Returns:
        np.array[complex]: A vector (numpy array) with 2 elements that represents
        a normalized quantum state.
    """

    ##################
    # YOUR CODE HERE #
    ##################
    # Find some complex number K such that a'=Ka, b'=Kb, and |a'|^2+|b'|^2=1

    # Base case: real numbers: eg 3 and 4. solution for c here will be 9+16=25
    # To normalize, we find the total length (sqrt(a^2+b^2)) and divide each of the components by that total length
    a_real = alpha.real
    b_real = beta.real
    c_real = (a_real**2+b_real**2)**(0.5)
    if c_real==0:
      ap_real = 0
      bp_real=0
    else:
      ap_real = a_real/c_real
      bp_real = b_real/c_real
    #ap_real = a_real/c_real
    #bp_real = b_real/c_real
    print("C real is "+str(c_real))

    # To extend to complex, need to identify the complex component.  eg 3i and 4i. Solution for c here will be -9-16=-25
    a_imag = alpha.imag
    b_imag = beta.imag
    c_imag = (a_imag**2+b_imag**2)**0.5
    if c_imag==0:
      ap_imag=0
      bp_imag=0
    else:
      ap_imag = a_imag/c_imag
      bp_imag = b_imag/c_imag
    print("C imag is "+str(c_imag))

    c_total = (c_imag**2+c_real**2)**0.5
    
    if c_total!=0:
      ap_imag = a_imag/c_total
      bp_imag = b_imag/c_total
      ap_real = a_real/c_total
      bp_real = b_real/c_total
        
    k = 1/(c_real+c_imag*1j)

    # CREATE A VECTOR [a', b'] BASED ON alpha AND beta SUCH THAT |a'|^2 + |b'|^2 = 1
    prime_vec = np.array([complex(ap_real,ap_imag),complex(bp_real,bp_imag)])
    # prime_vec = np.array([k*alpha,k*beta])

    # RETURN A VECTOR
    return prime_vec
    pass
```

</details>

#### Codercise I.1.2- Inner Product and Orthonormal Bases

Goal: Complete the inner_product function that computes the inner product between two arbitrary states. Then, use it to verify that \|0 ⟩ and \|1 ⟩ form an orthonormal basis (states are normalized and orthogonal)

<details>
<summary>Code</summary>

```javascript
def inner_product(state_1, state_2):
    """Compute the inner product between two states.

    Args:
        state_1 (np.array[complex]): A normalized quantum state vector
        state_2 (np.array[complex]): A second normalized quantum state vector

    Returns:
        complex: The value of the inner product <state_1 | state_2>.
    """

    ##################
    # YOUR CODE HERE #
    ##################

    # COMPUTE AND RETURN THE INNER PRODUCT
    s1ra = state_1.real[0]
    s1ia = state_1.imag[0]
    s1a = s1ra - 1j*s1ia 
    s1rb = state_1.real[1]
    s1ib = state_1.imag[1]
    s1b = s1rb - 1j*s1ib
    s2ra = state_2.real[0]
    s2ia = 1j*state_2.imag[0]
    s2a = s2ra + s2ia 
    s2rb = state_2.real[1]
    s2ib = 1j*state_2.imag[1]
    s2b = s2rb + s2ib

    product = ((s1a*s2a)+(s1b*s2b))
    
    return product

# Test your results with this code
ket_0 = np.array([1, 0])
ket_1 = np.array([0, 1])
test_0 = np.array([0.8,0.6])
test_1 = np.array([1 / np.sqrt(2), 1j / np.sqrt(2)])

print({inner_product(test_0,test_1)})
print(f"<0|0> = {inner_product(ket_0, ket_0)}")
print(f"<0|1> = {inner_product(ket_0, ket_1)}")
print(f"<1|0> = {inner_product(ket_1, ket_0)}")
print(f"<1|1> = {inner_product(ket_1, ket_1)}")

```

</details>

#### Codercise I.1.3- Sampling Measurement Outcomes

Goal: Write the function measure_state that takes a quantum state vector as input and simulates the outcomes of an arbitrary number of quantum measurements (i.e., return a list of samples 0 or 1 based on the probabilities given by the input state)

<details>
<summary>Code</summary>

```javascript
def measure_state(state, num_meas):
    """Simulate a quantum measurement process.

    Args:
        state (np.array[complex]): A normalized qubit state vector.
        num_meas (int): The number of measurements to take

    Returns:
        np.array[int]: A set of num_meas samples, 0 or 1, chosen according to the probability
        distribution defined by the input state.
    """

    ##################
    # YOUR CODE HERE #
    ##################

    measurements = []
    # Initialize a list
    
    # COMPUTE THE MEASUREMENT OUTCOME PROBABILITIES
    # Square each of the inputs for the associated probability
    prob = 100*(state[0]**2)
    print(prob)
    
    # MEASURER
    # For loop for number of measurements: Random function to pick a number from 1-100 (random die roll)
    # If the die roll is equal to or below the 0 rate, return 0. Otherwise, return 1. Add to the list

    # RETURN A LIST OF SAMPLE MEASUREMENT OUTCOMES
    for x in range(1,num_meas):
        rand = np.random.randint(1,100)
        print(rand)
        if rand<prob:
            measurements.append(0)
        else:
            measurements.append(1)

    npmeasurements = np.array(measurements)
    return npmeasurements
    pass

measure_state([0.8,0.6],10)
```

</details>

#### Codercise I.1.4- Applying a Quantum Operation

Goal: Complete the function apply_u to apply the provided quantum operation U to an input state

<details>
<summary>Code</summary>

```javascript
U = np.array([[1, 1], [1, -1]]) / np.sqrt(2)

def apply_u(state):
    """Apply a quantum operation.

    Args:
        state (np.array[complex]): A normalized quantum state vector.

    Returns:
        np.array[complex]: The output state after applying U.
    """

    ##################
    # YOUR CODE HERE #
    ##################
    #s0r: state 0 real, s0i: state 0 imaginary, s1r: state 1 real, s1i: state 1 imaginary

    print("Anything happening here?")
    s0 = state[0]
    s1 = state[1]
    print("States are "+str(s0)+" and "+str(s1))

    #For the top entry, we multiply the first row of U with both elements and sum them
    top_item = U[0][0]*s0 + U[0][1]*s1
    print("top item is "+str(top_item))
    bottom_item = U[1][0]*s0 + U[1][1]*s1
    print("bottom item is "+str(bottom_item))

    full_array = np.array([top_item,bottom_item])
    print("full array is "+str(full_array))
    return full_array
    
    # APPLY U TO THE INPUT STATE AND RETURN THE NEW STATE
    pass

# my_array=np.array[1,1]

# print("My array  is "+str(my_array))
# apply_u(my_array)

```

</details>

#### Codercise I.1.5-Quantum Simulator

Goal: Use the functions in one full package to simulate the outcome of running quantum algorithms on a single qubit.

1. Initialize a qubit in state \|0\>
2. Apply the provided operation U
3. Simulate measuring output state 100 times.

<details>
<summary>Code</summary>

```javascript
U = np.array([[1, 1], [1, -1]]) / np.sqrt(2)

def initialize_state():
    """Prepare a qubit in state |0>.

    Returns:
        np.array[float]: the vector representation of state |0>.
    """

    ##################
    # YOUR CODE HERE #
    ##################
    state = np.array([1,0])
    return state

    # PREPARE THE STATE |0>
    pass

def apply_u(state):
    """Apply a quantum operation."""
    return np.dot(U, state)

def measure_state(state, num_meas):
    """Measure a quantum state num_meas times."""
    p_alpha = np.abs(state[0]) ** 2
    p_beta = np.abs(state[1]) ** 2
    meas_outcome = np.random.choice([0, 1], p=[p_alpha, p_beta], size=num_meas)
    return meas_outcome

def quantum_algorithm():
    """Use the functions above to implement the quantum algorithm described above.

    Try and do so using three lines of code or less!

    Returns:
        np.array[int]: the measurement results after running the algorithm 100 times
    """

    ##################
    # YOUR CODE HERE #
    ##################
    nsamples = 100
    state = initialize_state()
    state2 = apply_u(state)
    measurements = measure_state(state2,nsamples)
    return measurements
    
    # PREPARE THE STATE, APPLY U, THEN TAKE 100 MEASUREMENT SAMPLES
    pass

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
