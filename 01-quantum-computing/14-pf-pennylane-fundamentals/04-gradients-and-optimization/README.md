# Gradients and Optimization

_Quantum Computing › PennyLane Fundamentals_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)

<!-- notion-import -->
## Notes from Notion

_Copied from **Gradients and optimization** in [🐣 PF (Pennylane fundamentals)](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)._

### Circuits as Functions

Quantum circuits are used as mathematical models and functions. For example, the following circuit can be represented as a function:

*[Image in Notion: Coordinates $`\theta_0 - \theta_3`$ are the gate parameters and $`\langle Z_0 \rangle`$ is the expectation value of the Pauli-Z observable on the first wire.](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

Note that simple circuits rarely have an “interesting” mathematical application.

PennyLane lets us create circuits. By using qp.BasicEntanglingLayers, we can create a circuit with multiple layers:

```python
n_wires = 4
dev = qp.device("default.qubit", wires = n_wires)

@qp.qnode(dev)
def entangler_circuit(weights):
  qp.BasicEntanglerLayers(weights, wires = range(n_wires))
  return qp.expval(qp.PauliZ(0))
  
In[1]: print(qp.draw(circuit, level = "device")([[0.1,0.2,0.3,0.4],[0.5,0.6,0.7, 0.8]]))

Out[1]:
0: ──RX(0.10)─╭●───────╭X──RX(0.50)─╭●───────╭X─┤  <Z>
1: ──RX(0.20)─╰X─╭●────│───RX(0.60)─╰X─╭●────│──┤     
2: ──RX(0.30)────╰X─╭●─│───RX(0.70)────╰X─╭●─│──┤     
3: ──RX(0.40)───────╰X─╰●──RX(0.80)───────╰X─╰●─┤     
```
We can add more entanglement via qp.StronglyEntanglingLayers, which can create a much more complex circuit:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

#### Codercise PF.4.1- Circuits As Functions

Complete the circuit-as-function qnode below, implementing the quantum circuit shown below and returning $`\langle Z_0\rangle`$ on the first wire. The params argument is an np.ndarray representing $`[\theta_0,\theta_1,\theta_2,\theta_3]`$ in that order. The backend will plot a cross-section of the circuit, where x is the angles and y is output_values. All parameters are constant except for the angle $`\theta_1`$ on the RY gate on the first wire.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

<details>
<summary>Code</summary>

```javascript
dev = qp.device("default.qubit", wires = 3)

@qp.qnode(dev)
def circuit_as_function(params):
    """
    Implements the circuit shown in the codercise statement.
    Args:
    - params (np.ndarray): [theta_0, theta_1, theta_2, theta_3]
    Returns:
    - (np.tensor): <Z0>
    """

    ####################
    ###YOUR CODE HERE###
    ####################
    qml.RX(params[0],wires=0)
    qml.CNOT(wires=[0,1])
    qml.CNOT(wires=[1,2])
    qml.CNOT(wires=[2,0])
    qml.RY(params[1],wires=0)
    qml.RY(params[2],wires=1)
    qml.RY(params[3],wires=2)

    return qml.expval(qml.PauliZ(wires=0))# Return the expectation value

angles = np.linspace(0, 4 * np.pi, 200)
output_values = np.array([circuit_as_function([0.5, t, 0.5, 0.5]) for t in angles])
```

</details>

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

Also note another helpful function: qp.AllSinglesDoubles template can prepare a superposition of all possible excitations of electronic states, starting from two electrons in the lowest energy level.

PennyLane offers other templates as well, found here:

[https://docs.pennylane.ai/en/stable/introduction/templates.html](https://docs.pennylane.ai/en/stable/introduction/templates.html)

### Gradient of a Circuit

The circuit below is a real-valued function $`F: \R^4 \rightarrow \R`$, with a derivative encoded in its gradient $`\nabla F= (\frac {\partial F}{\partial \theta_0},\frac {\partial F}{\partial \theta_1},\frac {\partial F}{\partial \theta_2},\frac {\partial F}{\partial \theta_3})`$. We can find this partial derivative through parameter-shift rules. When F represents the expectation value of a quantum circuit with only single-parameter gates,

$$
\frac{\partial F}{\partial \theta_i}=\frac{1}{2}[F(\theta_i+\frac{\pi}{2})-F(\theta_i-\frac{\pi}{2})]
$$

This rule allows us to compute gradients with little computational overhead and a robustness to errors.

By using qp.jacobian, we can find the gradient of our circuit:

```javascript
n_wires = 4
dev = qp.device("default.qubit", wires = n_wires)

@qp.qnode(dev,interface="autograd", diff_method="parameter-shift")
def entangler(weights):
    qp.BasicEntanglerLayers(weights, wires = range(n_wires))
    return qp.expval(qp.PauliZ(0))
    
In[2]: test_weights = np.array([[0.1,0.2,0.3,0.4]], requires_grad = True)

print(qp.jacobian(entangler)(test_weights))

Out[2]: [[ 1.38777878e-17 -1.74813749e-01 -2.66766415e-01 -3.64609810e-01]]
```
Note in this example that we need to specify that the input parameters (theta of 0.1-0.4) need to be differentiable - we specify “requires_grad=True” and define the inputs as a np.array. We also need to decorate the qnode with a diff method to specify we want to calculate the partial derivative using a parameter shift - specifically, interface="autograd", diff_method="parameter-shift”

### Jacobian of a Circuit

qp.jacobian also computes the Jacobian matrix of a circuit (which is the transformation matrix of the derivatives). To get the Jacobian matrix output, we specify “requires_grad=true”.

```javascript
dev = qp.device("default.qubit", wires = 2)

@qp.qnode(dev)
def vector_valued_circuit(params):
    qp.RX(params[0], wires = 0)
    qp.CNOT(wires=[0,1])
    qp.RY(params[1], wires = 0)
    return qp.probs(wires = [0,1])

sample_params = np.array([0.1,0.2], requires_grad = True)
print(qp.jacobian(vector_valued_circuit)(sample_params))

Out[4]: 
[[-0.0494192  -0.09908654]
 [ 0.00049751  0.00024813]
 [-0.00049751  0.09908654]
 [ 0.0494192  -0.00024813]]
```
Since there are 4 computational basis outcome probabilities (00, 01, 10, 11) and 2 gate parameters, the Jacobian Matrix is 4x2.

### Higher Order Derivatives

We can also use higher-powered derivatives. In scalar valued circuits, $`F: \R^n \rightarrow \R`$, we want to find the stationary points of F (where the gradient becomes zero), which may be local or global maxima or minima, saddles, or inflections of F. We can extract stationary point information using the Hessian Matrix H\[F\] containing the partial derivatives of F:

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

Positive definite matrices indicate a global minimum and negative definite indicate a global maximum. By examining eigenvectors and eigenvalues, we can parse out saddle and inflection point information.

We can calculate the Hessian Matrix as the Jacobian of the gradient of F: $`H[F]=J[\nabla F]`$

Since the Jacobian of a circuit is differentiable, we can find the Hessian of a scalar-valued function by nesting 2 qp.jacobians. Note that we set the differentiation method as parameter shift, and set max_diff to 2 to indicate we want the QNode to be differentiated twice (default value is 1)

```javascript
dev = qp.device("default.qubit", wires = 2)

@qp.qnode(dev, diff_method = "parameter-shift", max_diff = 2)
def scalar_valued_circuit(params):
  qp.RX(params[0], wires = 0)
  qp.CNOT(wires=[0,1])
  qp.RY(params[1], wires = 0)
  return qp.expval(qp.PauliZ(0))

test_params = np.array([0.7,0.3], requires_grad = True)
qp.jacobian(qp.jacobian(circuit))(test_params)

Out[5]:
[[-0.73068165  0.19037934]
 [ 0.19037934 -0.73068165]]
```

### Optimizing Circuits

To examine how we can optimize circuits, we’ll follow an example:

```javascript
dev = qp.device("default.qubit", wires = 2)

@qp.qnode(dev, diff_method = "parameter-shift")
def scalar_valued_circuit(params):
    qp.RX(params[0], wires = 0)
    qp.CNOT(wires=[0,1])
    qp.RY(params[1], wires = 0)
    return qp.expval(qp.PauliZ(0))
```
We want to know the minimum expectation value that $`\langle Z_0\rangle`$ can have. To do this, we treat the circuit as a cost function. The simplest  optimizer (way to find the minimum of a function) is by using gradient descent, which PennyLane offers as a built-in feature. qp.GradientDescent has a stepsize argument (the learning rate).

We write our optimizing function, specifying the cost function, stepsize, and initial parameters.

```javascript
def optimize(cost_function, init_params, *steps):

    opt = qp.GradientDescentOptimizer(stepsize = 0.4)
    steps = 100
    params = init_params

    for i in range(steps):
      params = opt.step(cost_function, params)

    return params, cost_function(params)
```
In return, we will see the parameters for which the cost function is optimized, as well as the minimum value of the cost function.

Given parameters \[0.3,0.7\] and 100 steps, we see the following results:

```javascript
In[6]: initial_parameters = np.array([0.7,0.3], requires_grad = True)
print(optimize(scalar_valued_circuit, initial_parameters, 100))
Out[6]: (tensor([3.14159265e+00, 6.72460964e-17], requires_grad=True), array(-1.))
```

### Codercise PF.4.2 - A Strong Entangler

Complete the circuit below so that it applies qp.StronglyEntanglingLayers to a four-qubit initial state $`|0\rangle^{\otimes 4}`$ as a function of weights and returns the expectation value $`\langle Z_0\rangle`$. Then, write a valid input weights with the correct shape to produce a valid output.

Note that stronglyentangledlayers applies the cnots by default - that’s how the wires are entangled!

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```javascript
dev = qp.device("default.qubit", wires = 4)

@qp.qnode(dev)
def strong_entangler(weights):
    """
    Applies Strongly Entangling Layers to the default initial state
    Args:
    - weights (np.ndarray): The weights argument for qp.StronglyEntanglingLayers
    Returns:
    - (np.tensor): <Z0>
    """
    qml.StronglyEntanglingLayers(weights, wires=[0,1,2,3])
    
    return qp.expval(qp.PauliZ(0))

test_weights = np.array([[[0,0,0],[0,0,0],[0,0,0],[0,0,0]]])# Write some valid weights here.

print("The output of your circuit with these weights is: ", strong_entangler(test_weights))

```

### Codercise PF.4.3 - Training your QNode

Complete the embedding_and_circuit QNode which depends on a non-trainable array of parameters features and trainable parameters params. The features are the arguments of a qp.AngleEmbedding routine, applied at the start of the circuit to encode some features. Then it is followed by a quantum model that depends on params = $`[\theta_0,\theta_1,\theta_2]`$<br>The full circuit is shown below.

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```javascript
dev = qp.device("default.qubit", wires = 3)

@qp.qnode(dev,interface="autograd",diff_method="parameter-shift")
def embedding_and_circuit(features, params):
"""
A QNode that depends on trainable and non-trainable parameters
Args:
- features (np.ndarray): Non-trainable parameters in the AngleEmbedding routine
- params (np.ndarray): Trainable parameters for the rest of the circuit
Returns:
- (np.tensor): <Z0>
"""
qp.AngleEmbedding(features=features,wires=[0,1,2])
# qp.BasicEntanglerLayers(params,wires=range(3))
qml.CNOT(wires=[0,1])
qml.CNOT(wires=[1,2])
qml.CNOT(wires=[2,0])
qml.RY(params[0],wires=0)
qml.RY(params[1],wires=1)
qml.RY(params[2],wires=2)

return qp.expval(qp.PauliZ(0))

features = np.array([0.3,0.4,0.6], requires_grad = False)
params = np.array([0.4,0.7,0.9], requires_grad = True)
print("The gradient of the circuit is:", qp.jacobian(embedding_and_circuit)(features, params))

```

### Codercise PF.4.4 - Differentiate and Repeat

Write the circuit_for_hessian QNode associated with the circuit below, which depends on params =$`[\theta_0,\theta_1,\theta_2]`$<br>and returns the expectation value $`\langle Z_0 \otimes Z_1\rangle`$.

The QNode should be set to be differentiated with the parameters-shift method a maximum of two times.

Then, compute the hessian of the circuit at the point test_params = \[0.1,0.2,0.3,0.4\].

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```javascript
dev = qp.device("default.qubit", wires = 2)

@qp.qnode(dev, diff_method = "parameter-shift", max_diff = 2)
def circuit_for_hessian(params):
    """
    Implements the circuit shown in the codercise statement
    Args:
    - params (np.ndarray): [theta_0, theta_1, theta_2, theta_3]
    Returns:
    - np.tensor: <Z0xZ1>
    """
    qml.RY(params[0],wires=0)
    qp.IsingXX(params[1],wires=[0,1])
    qml.RX(params[2],wires=0)
    qml.RX(params[3],wires=1)
    

    return qp.expval(op=qp.Z(wires=0)@qp.Z(wires=1))# Return the expectation value required

test_params = np.array([0.1,0.2,0.3,0.4], requires_grad = True)
# Don't change test_params! 

hessian = qp.jacobian(qp.jacobian(circuit_for_hessian))(test_params)# Compute the Hessian
print("The hessian of the circuit is: \n", hessian)

```

### Codercise PF.4.5 - At What Cost?

Given the circuit “circuit_as_a_function” with output x (expectation value $`\langle Z_0\rangle`$), and cost function defined as $`F(x)=x^3-\frac{1}{2}x^2+x`$, write the cost_function as a function of params (input arguments to circuit_as_a_function).

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)*

```javascript
def cost_function(params):
    """
    Computes the cost function given in the codercise, as a function of the
    parameters of circuit_as_function.
    Args:
    - params (np.ndarray): The parameters we pass to circuit_as_function
    Returns:
    - np.float: The cost function evaluated in params.
    """
    ################
    #YOUR CODE HERE#
    ################
    ## cost_function = x^3 - 0.5(x^2)+x
    x=circuit_as_function(params)
    return x**3-(0.5)*(x**2)+x# Return the value of the cost function

```

### Codercise PF.4.6 - Cost Effective

By using the optimize function, we can input a cost function, initial set of parameters, and a step count to optimize a cost function.

Use optimize to find the minimum of cost function “cost_function”, where minimum is a np.array containing only the minimum value of cost_function.

```javascript
def optimize(cost_function, init_params, steps):

    opt = qp.GradientDescentOptimizer(stepsize = 0.4) # Change this as you see fit
    # steps=100
    params = init_params

    for i in range(steps):

        params = opt.step(cost_function, params)

    return float(cost_function(params))
initial_parameters=np.array([np.pi,0.7,0.7,0.7],requires_grad=True)
minimum = optimize(cost_function, initial_parameters, 100)

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
