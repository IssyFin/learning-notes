# Dynamic Circuits

_Quantum Computing › PennyLane Fundamentals_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)

## Mid-Circuit Measurements

**Why Dynamic Circuits:** Mid-circuit measurements (MCM) -is flexible; algorithms can rely on intermediate results, which allow for qubit usage optimization

- Helpful for error correction code

### Measure and Reset

Perform in PennyLane via qp.measure() , and reuse that measured qubit by setting reset=True

```python
@qp.qnode(dev)
def func():
	qp.PauliX(1)
	m_1 = qp.measure(1, reset=True) # mid-circuit measurement and reset on wire 1
	m_2 = qp.measure(2, reset=False) # mid-circuit measurement on wire 2, no reset
	qp.PauliX(2)
	return qp.probs(wires=[1,2])

fig, a = qp.draw_mpl(func)()
fig.show()
```

!image.png

### Post Select

Based on the measurement outcomes, we can keep or discard the qubit using postselect within qp.measure.

- postselect=0 is equivalent to applying the projector $|0\rangle \langle0|$ and discarding all instances where outcome is 1

```python
dev = qp.device("default.qubit")

@qp.qnode(dev)
def func(x):
	qp.RX(x,wires=0)
	m_0 = qp.measure(0,postselect=0)
	return qp.sample(wires=0)

print(func(np.pi / 2, shots=10))

Out[1]: [0 0 0 0 0]
```

### Conditional Operators

Perform operators based on outcome of mid-circuit measurements using qp.cond(). In example below, qp.cond() returns a function to be performed whenever m_0==0 is fulfilled

```python
dev = qp.device("default.qubit")

@qp.qnode(dev)
def qnode_conditional_op_on_zero(x,y):
	qp.RY(x,wires=0)
	qp.CNOT(wires=[0,1])
	m_0 = qp.measure(1)
	
	qp.cond(m_0==0,qp.RY)(y,wires=0)
	return qp.probs(wires=[0])

pars = np.array([0.643,0.246],requires_grad=True)

In[2]: qnode_conditional_op_on_zero(*pars)
Out[2]: tensor([0.88660045,0.11339955],requires_grad=True)
```

## Collecting Statistics

Several terminal measurement statistics functions are provided by PennyLane:

- counts()
- expval()
- probs()
- sample()
- var()

These can also modify mid-circuit elements. For example:

```python
dev = qp.device("default.qubit")

@qp.qnode(dev)
def circuit(phi, theta):
	qp.RX(phi,wires=0)
	m_0 = qp.measure(wires=0)
	qp.RY(theta,wires=1)
	m_1 = qp.measure(wires=1)
	return qp.sample(~m_0 - 2 * m_1)
```

<aside>

## Codercise PF.5.1 - Boom! - bomb tester

In 1993, Elitzur and Vaidman devised a thought experiment for testing potentially defective bombs without detonating them. The idea relies on the principle that photon self-interference is destroyed when which-path information is, even in principle, available. They called this principle *Interaction-free measurement*.

Imagine a batch of photosensitive bombs, some live and others defective (duds). These bombs are tested by sending a single photon through a Mach-Zehnder interferometer with two output detectors, labeled C and D. The bomb to be tested is placed in one of the paths after the first beam splitter.

!image.png

Without a bomb or with a dud, the photon is always detected at C. With a live bomb, each photon has a 25% chance of detection at D (identifying a live bomb without detonation), a 50% chance of causing an explosion, and a 25% chance of detection at C (uncertain diagnosis). Check this paper for a detailed explanation about these statistics and the idea of interaction-free measurement.

Complete the following PennyLane circuit to model the behavior for a live-bomb test using mid-circuit measurements. Practice collecting statistics using counts and calculate the probability, `prob_suc`, of identifying a live bomb without detonation.

- Code
    
    ```python
    n_shots = 10000
    dev = qp.device("default.qubit", shots=n_shots)
    np.random.seed(0)
    
    @qp.qnode(dev)
    def circuit():
        """
        This quantum function implements the 'bomb tester' for a live bomb using mid-circuit measurements
        and returns relevant statistics with qp.counts
        """
    
          # 1st beam-splitter
        qp.Hadamard(0)
        m_bomb = qp.measure(0)  # measure and postselect
          # 2nd beam-splitter
        qp.Hadamard(0)
        m_det =  qp.measure(0) # measure (detectors)
        return qp.counts([m_bomb,m_det])  # collect counts for m_bomb and m_det
    
    results = circuit()
    prob_suc = results.get('01',0) / n_shots # favorable cases over total cases
    
    print('the success probability is',prob_suc)
    
    ```
    
</aside>

<aside>

## Codercise PF.5.2 - Improved Tester

As we discussed in the previous Codercise, the success rate from such bomb tester is 25%. Could you think of a way to bump up the success rate?
Use qp.cond() in the following circuit to go from a 25% to a 31.25% success rate.

```python
n_shots = 100000
dev = qp.device("default.qubit", shots=n_shots)
np.random.seed(0)

@qp.qnode(dev)
def circuit():
    # first pass
    qp.Hadamard(0)                          # 1st beam-splitter
    m_bomb = qp.measure(0, postselect=0)    # live bomb
    qp.Hadamard(0)                          # 2nd beam-splitter
    m_det = qp.measure(0)                   # detectors

    # retry (the four lines)
    qp.cond(m_det == 1, qp.Hadamard)(0)     # 1st beam-splitter, retry only
    qp.measure(0, postselect=0)             # bomb again (unconditional)
    qp.cond(m_det == 1, qp.Hadamard)(0)     # 2nd beam-splitter, retry only
    m_det_2 = qp.measure(0)                 # second set of detectors

    return qp.counts(op=[m_bomb, m_det]), qp.counts(op=[m_det, m_det_2])

results = circuit()
prob_suc_1 = results[0]["00"] / n_shots
prob_suc_2 = results[1]["10"] / n_shots
prob_suc = prob_suc_1 + prob_suc_2
print("The success probability is", prob_suc)
```

*Note - my solution is showing up as “not quite right” in the codercise panel and I haven’t been able to debug it*

</aside>

## Resources

- 
