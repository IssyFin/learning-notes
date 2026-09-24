# What Did You Expect?

_Quantum Computing › Single-Qubit Gates_

[Course link](https://pennylane.ai/codebook/single-qubit-gates/what-did-you-expect)

<!-- notion-import -->
## Notes from Notion

_Copied from **What did you expect? (Sample and process quantum measurement outcomes)** in [🔘 SQ (single qubit gates)](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)._

Note that we are interested in other measurable quantities of a state, besides probabilities (typically, energy). In quantum mechanics and quantum computing, physical quantities are related to an *observable* - observables correspond to Hermitian matrices whose eigenvalues represent the possible values of the measurement outcome.

### Reminder: Hermitian Matrices

A matrix B is Hermitian if $`B=B^\dagger`$. Hermitian matrices have real eigenvalues.

### Example: Pauli Z Operation

$$
Z=\begin{pmatrix}1&0\\0&-1\end{pmatrix}\\
Z^\dagger=\begin{pmatrix}1&0\\0&-1\end{pmatrix}\\
\lambda=1,-1\\
\lambda=1: |0\rangle\\
\lambda=-1:|1\rangle
$$

Note that Pauli X is similar, but has eigenstates $`\lambda=1: |+\rangle`$ and $`\lambda=-1:|-\rangle`$.

Following measurement, the qubit’s state will be that of the eigenvector of the corresponding measurement outcome.

The expectation value of an observable is the weighted average of what would be seen over multiple experiments.

### Exercise I.10.1

Suppose we measure an observable B for which there is a 70% chance we obtain eigenvalue 1 and 30% chance we obtain the eigenvalue -1. What is the expectation value of B?

$$
\langle B \rangle=0.7*1+0.3*(-1)=0.4
$$

Note that, while in each individual experiment we see either 1 or -1, we see 1 more often — the expected value is closer to 1 than it is to -1

The expectation value of some observable B for a state $`|\psi\rangle`$ is given by

$`\langle B \rangle=\langle \psi|B|\psi\rangle`$

We first compute the result of the matrix B applied to $`|\psi\rangle`$, followed by its inner product with $`|\psi\rangle`$.

### Exercise I.10.2

Let $`|\psi\rangle=\frac{4}{5}|0\rangle-\frac{3}{5}e^{i\pi/3}|1\rangle`$, with observable $`B=\begin{pmatrix}1&-2i\\2i&2\end{pmatrix}`$.

1. What are the possible outcomes of the measurement?
First, find eigenvalues:

$$
\begin{pmatrix}1&-2i\\2i&2\end{pmatrix}\\
det\begin{pmatrix}1-\lambda&-2i\\2i&2-\lambda\end{pmatrix}=(2-\lambda)(1-\lambda)-(2i)(-2i)=0\\
(2-\lambda)(1-\lambda)-(-4)(-1)\rightarrow (2-\lambda)(1-\lambda)=4\\
(2-\lambda)(1-\lambda)-4=0\\
2-3\lambda+\lambda^2-4=0\\
\lambda^2-3\lambda-2=0\\
\lambda=3.56155281,-0.56155281\checkmark
$$

Possible measurements are 3.56155281 and -0.56155281

2. For each outcome, what state is the qubit in after measurement? (Express in the computational basis)
Finding eigenvectors:

$$
\begin{pmatrix}1-3.56155281&-2i\\2i&2-3.56155281\end{pmatrix}\begin{pmatrix}v_1\\v_2\end{pmatrix}=\begin{pmatrix}0\\0\end{pmatrix}\\
(1-\lambda)v_1-2iv_2=0, 2iv_1+2v_2-\lambda v_2=0\\
2iv_1=(\lambda-2)v_2: v_1=\frac{\lambda-2}{2i}v_2\\
\lambda=3.56155281:\\
v1=\frac{1-3.56155}{2i}v_2=\frac{-1.28}{i}v_2\\
if v_2=1:\\
v_1=\frac{-1.28}{i}\\
span=\begin{pmatrix}-1.28i\\1\end{pmatrix}\\
\lambda=-0.56155281\\
v_1=\frac{-2.56155281}{2i}v_2\\
if v_2=1:\\
v_1=\frac{-1.28}{i}\\
span=\begin{pmatrix}-1.28/i\\1\end{pmatrix}

$$

Then we normalize:

$$
ev_1=\begin{pmatrix}(-1.28i)/mag\\1/mag\end{pmatrix}\\
mag_1=\sqrt{(-1.28i)^2+1^2}=1.62\\
ev_1=\begin{pmatrix}\frac{-1.28i}{1.62}\\\frac{1}{1.62}\end{pmatrix}=\begin{pmatrix}-0.79i\\0.617\end{pmatrix}\\
ev_2=\begin{pmatrix}-1.28/i*mag\\1/mag\end{pmatrix}\\
mag_2=\sqrt{(-1.28/i)^2+1}=1.62\\
ev_2=\begin{pmatrix}0.79/i\\0.617\\\end{pmatrix}
$$

Note- my answers are a little off - the correct evectors are:

$$
|v_1\rangle=-0.61541221i|0\rangle+0.78820544|1\rangle\\
|v_2\rangle=0.78820544|0\rangle-0.61541221i|1\rangle
$$

3. What is the expectation value of B computed analytically?
First, compute matrix applied to $`|\psi\rangle`$:

$$
\langle B \rangle=\langle \psi|B|\psi\rangle\\
B|\psi\rangle:\\
\begin{pmatrix}1&-2i\\2i&2\end{pmatrix}\begin{pmatrix}\frac{4}{5}\\\frac{-3}{5}e^{i\pi/3}\end{pmatrix}\\
=\begin{pmatrix}\frac{4}{5}+\frac{6i}{5}e^{i\pi/3}\\\frac{8i}{5}-\frac{6}{5}e^{i\pi/3}\end{pmatrix}

$$

Then, take the inner product with $`\langle\psi|`$:

$$
\langle\psi|B|\psi\rangle:\\
\begin{pmatrix}\frac{4}{5}&\frac{3}{5}e^{i\pi/3}\end{pmatrix}\begin{pmatrix}\frac{4}{5}+\frac{6i}{5}e^{i\pi/3}\\\frac{8i}{5}-\frac{6}{5}e^{i\pi/3}\end{pmatrix}\\
=\frac{16}{25}+\frac{24i}{25}e^{i\pi/3}-\frac{24i}{25}e^{i\pi/3}-\frac{18}{25}e^{2i\pi/3}\\
=\frac{2}{25}(8-9e^{2i\pi/3})=...
$$

Note: Some arithmetic error here: Full process is below

$$
\langle\psi|B|\psi\rangle=\frac{1}{25}\begin{pmatrix}4&-3e^{-i\pi/3}\end{pmatrix}\begin{pmatrix}1&-2i\\2i&2\end{pmatrix}\begin{pmatrix}4\\-3e^{i\pi/3}\end{pmatrix}=-0.302769
$$

4. Suppose we prepared this state and measured B 1,000 times. 54 of the trials yielded the larger of the two possible measurement outcomes, and the remaining tries yielded the smaller one. What is the experimentally-obtained expectation value of B?
The experimentally obtained expectation value of B would be:

$$
\frac{54}{1000}*3.56155281+\frac{946}{1000}*-0.56155281\\
-0.3389\checkmark
$$

Note that the match is not exact, but we take it anyway to limit an infinite number of trials.

---

### Codercise I.10.1 - Measurement of the PauliY Observable

To compute expectation values, we can use qml.expval rather than the qml.probs function.

Common observables include qml.PauliX, qml.PauliY, and qml.PauliZ. Possible outcomes of any Pauli-based expectation value measurement will be either 1 or -1 (their eigenvalues).

However, to measure the expectation value, we must specify what observable we are measuring, and at which wire. Format is “qml.expval(qml.PauliZ(wires=0))”

**Design and run a PennyLane circuit that performs the following, where **$`\langle Y\rangle`$** indicates the measurement of the PauliY observable.**

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

Note: It is usually more convenient to use the shorthand qml.PauliZ(0) when specifying expectation values. Otherwise, the lines of code will get quite long when you get to the multi-qubit case!

<details>
<summary>My code</summary>

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

### Codercise I.10.2 - Setting up the number of experiment shots

One run and measurement is considered a “shot” or a “sample”. Typically, we’ll measure a thousand or so runs to get expected outcome probabilities. We can specify the number of shots during device construction:<br>dev=qml.device(’default.qubit’,wires=1,shots=1000

In the code below is a list of possible numbers of shots. For each value, initialize a device, then create a QNode that runs the circuit from the previous exercise. What happens to the expectation value as you increase the number of shots?

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

```python
# An array to store your results
shot_results = []

# Different numbers of shots
shot_values = [100, 1000, 10000, 100000, 1000000]

for shots in shot_values:
    ##################
    # YOUR CODE HERE #
    ##################

    # CREATE A DEVICE, CREATE A QNODE, AND RUN IT
    dev = qml.device("default.qubit", wires=1,shots=shots)
    @qml.qnode(dev)
    def circuit():
        qml.RX(np.pi/4,wires=0)
        qml.H(wires=0)
        qml.PauliZ(wires=0)
        
    
        return qml.expval(qml.PauliY(wires=0))
    expresult = circuit()
    shot_results.append(expresult)
    # STORE RESULT IN SHOT_RESULTS ARRAY
    
    pass

print(qml.math.unwrap(shot_results))

```
As the number of shots increases, our experimental expectation value gets closer to the computed expectation value.

Results:<br><br>`[-0.78, -0.734, -0.712, -0.70896, -0.707426]`

### Codercise I.10.3 - Evaluating the samples

We can use the sample results to compute the expectation value the same way we’d normally take a weighted average: $`\langle Y\rangle=\frac{1*(1s)+(-1)*(-1s)}{shots}`$

We can access samples directly by returning qml.sample rather than qml.expval

“qml.sample(qml.PauliZ(wires=0))”

Using the circuit from earlier, replace qml.expval with qml.sample. Then, write a function to compute an estimate of the expectation value based on the samples.

```python
dev = qml.device("default.qubit", wires=1, shots=100000)

@qml.qnode(dev)
def circuit():
    qml.RX(np.pi / 4, wires=0)
    qml.Hadamard(wires=0)
    qml.PauliZ(wires=0)

    ##################
    # YOUR CODE HERE #
    ##################

    # RETURN THE MEASUREMENT SAMPLES OF THE CORRECT OBSERVABLE

    return qml.sample(qml.PauliY(wires=0))

def compute_expval_from_samples(samples):
    """Compute the expectation value of an observable given a set of
    sample outputs. You can assume that there are two possible outcomes,
    1 and -1.

    Args:
        samples (np.array[float]): 100000 samples representing the results of
            running the above circuit.

    Returns:
        float: the expectation value computed based on samples.
    """

    estimated_expval = 0

    ##################
    # YOUR CODE HERE #
    ##################
    negCount = 0
    posCount = 0
    print(samples[1:10])
    totalCount=0
    for sample in samples:
        if sample==-1:
            # estimated_expval+=1
            negCount+=1
        else:
            # estimated_expval+=-1
            posCount+=1
    estimated_expval=(posCount-negCount)/100000

    # USE THE SAMPLES TO ESTIMATE THE EXPECTATION VALUE

    return estimated_expval

samples = circuit()
print(compute_expval_from_samples(samples))

```

### Codercise I.10.4 - The variance of sample measurements

Note that, with variance, our experimental expectation value will vary each time the code is run, since the set of samples will likely vary. Given a certain number of shots, how close can we expect to get?

Explore how the accuracy of the expectation value depends on the number of shots. Eg, if we run 100 experiments with 100 shots each, what are the mean and variance of the distribution of expectation values obtained? and how does variance scale with the number of shots?

We will use a very simple circuit with a Hadamard and a measurement of the PauliZ observable to directly extract the dependence of the variance on the number of shots.

Based on the plot, complete the variance_scaling function to determine the relationship between variance and shots.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a65680c297fdc68c28e0c1e3)*

<details>
<summary>My Code</summary>

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

    ##################
    # YOUR CODE HERE #
    ##################

    # CREATE A DEVICE WITH GIVEN NUMBER OF SHOTS

    # DECORATE THE CIRCUIT BELOW TO CREATE A QNODE
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

def variance_scaling(n_shots):
    """Once you have determined how the variance in expectation value scales
    with the number of shots, complete this function to programmatically
    represent the relationship.

    Args:
        n_shots (int): The number of shots

    Returns:
        float: The variance in expectation value we expect to see when we run
        an experiment with n_shots shots.
    """

    estimated_variance = 1/n_shots

    ##################
    # YOUR CODE HERE #
    ##################

    # ESTIMATE THE VARIANCE BASED ON SHOT NUMBER

    return estimated_variance

# Various numbers of shots; you can change this
shot_vals = [10, 20, 40, 100, 200, 400, 1000, 2000, 4000]

# Used to plot your results
results_experiment = [variance_experiment(shots) for shots in shot_vals]
results_scaling = [variance_scaling(shots) for shots in shot_vals]
plot = plotter(shot_vals, results_experiment, results_scaling)

```

</details>


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
