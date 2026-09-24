# Dynamic Circuits

_Quantum Computing › PennyLane Fundamentals_

[Course link](https://pennylane.ai/codebook/pennylane-fundamentals)

<!-- notion-import -->
## Notes from Notion

_Copied from **Dynamic Circuits** in [🐣 PF (Pennylane fundamentals)](https://app.notion.com/p/2b544de2a6568084b8c2c3aa39b0e9a0)._

---

- Inspecting Quantum Circuits
dev = qp.device("default.qubit", wires = 3)

@qp.qnode(dev)<br>def circuit_as_function(params):<br>"""<br>Implements the circuit shown in the codercise statement.<br>Args:<br>- params (np.ndarray): \[theta_0, theta_1, theta_2, theta_3\]<br>Returns:<br>- (np.tensor): \<Z0\><br>"""

```plain text
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
```
angles = np.linspace(0, 4 \* np.pi, 200)<br>output_values = np.array(\[circuit_as_function(\[0.5, t, 0.5, 0.5\]) for t in angles\])


## Summary


## Key ideas

- 

## Worked examples / code


## Questions

- 

## Resources

- 
