# MQ: Circuits with Many Qubits

_Quantum Computing_

[Course link](https://pennylane.ai/codebook/circuits-with-many-qubits)


## Unit notes


<!-- notion-import -->
## Notes from Notion

_Copied from [💠 MQ (Many Qubits)](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)._

### Chapter 3 Summary - Content

---

<details>
<summary>Representing more than one qubit</summary>

- Basis consists of all different representations of the basis states (eg a 2-qubit system could be \|00\>,\|01\>,\|10\>, or \|11\>)
- Can also represent as binary equivalent (eg \|11\> → \|3\>)
- Associated element in the 4-dimensional basis vector:
  - \|11\> → \|3\> → (0,0,0,1)
  - \|10\> → \|2\> → (0,0,1,0)
- Size expands quickly w/ # of qubits: 2\^n basis vectors of size 2\^n
  - 2 qubits: 4 basis vectors of size 4 (eg (a,b,c,d))
  - 3 qubits: 8 of size 8
  - 4 qubits: 16 of size 16
  - This is why we need real quantum computers (rather than just simulating)

</details>

<details>
<summary>Tensors</summary>

- Represented by $`\otimes`$ (eg $`Z\otimes X`$)
- Effectively when 2 different qubits are operated on independently;
  - When we measure a state with 2 or more qubits, we multiply them together for the expected value

</details>

<details>
<summary>Entanglement</summary>

- A state is separable if it can be described as the tensor product of individual qubit states. If it cannot, it is entangled.
- Fully entangled states: None of the qubits can be written independently of the others.
- Bipartite state: Can be split up into two subsystems, where one is fully entangled and one is separable.

</details>

<details>
<summary>Multi-Qubit Gates</summary>

<details>
<summary>CNOT</summary>

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

*[Image in Notion: image](https://app.notion.com/p/2b544de2a6568010950df53cce7a7432)*

</details>

- Universal gate sets for multi-qubit systems are our single-qubit system universal gate set and the CNOT operation. (Popular choices are $`{CNOT,H,T}`$ and $`CNOT,RY,RZ`$. )
- For odd-numbered qubit counts, use Hadamard to expand:<br>$`|0\rangle\langle0| \otimes H \otimes I \otimes H+|1\rangle\langle1| \otimes H \otimes U \otimes H`$
- Control operations can be signified by using the Identity matrix and Unitary Matrix Operation, along with filler zeros:<br>$`\begin{pmatrix}I_2&0\\0&U\end{pmatrix}=\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&U_{11}&U_{12}\\0&0&U_{21}&U_{22}\end{pmatrix}`$

</details>

### Chapter 3 Summary - Code

---

<details>
<summary>Apply a tensor (@)</summary>

```python
qml.PauliZ(wires=0)@qml.PauliZ(wires=1)
```

</details>

<details>
<summary>Visualize using plotter</summary>

```python

plot = plotter(theta, ZI_results, IZ_results, ZZ_results, combined_results)
```

</details>

<details>
<summary>Multi-qubit gates</summary>

<details>
<summary>CNOT</summary>

```python
qml.CZ(wires=[0,1])
#Where 0 is control and 1 is target
```

</details>

<details>
<summary>Toffoli</summary>

```python
qml.Toffoli(wires=[0,1,2])
# Where wires 0 and 1 are control and 2 is target
```

</details>

<details>
<summary>Mixed-Control</summary>

```python
qml.MultiControlledX(wires=[0,1,2,3],control_values=[1,0,1])
```

</details>

</details>

## Lessons

1. [Multi-Qubit Systems](./01-multi-qubit-systems/)
2. [All Tied Up](./02-all-tied-up/)
3. [We've Got It Under Control](./03-weve-got-it-under-control/)
4. [Multi-Qubit Gate Challenge](./04-multi-qubit-gate-challenge/)
