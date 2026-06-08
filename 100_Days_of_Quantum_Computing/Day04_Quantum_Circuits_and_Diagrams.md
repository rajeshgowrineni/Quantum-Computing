# Day 4: Quantum Circuits and Diagrams

Welcome back! Now that you know qubits, superposition, entanglement, and gates, it's time to combine them into full **quantum circuits** — the quantum equivalent of classical programs. Today you'll learn to read, draw, and build circuits using both diagrams and code.

---

## Theoretical Foundation: What Is a Quantum Circuit?

### From Gates to Circuits

A quantum circuit is an **ordered sequence of quantum gates** applied to a set of qubits, followed by measurements. Just like a classical program is a sequence of logic gates (AND, OR, NOT) acting on bits, a quantum circuit is a sequence of quantum gates acting on qubits.

Think of it this way:
- **Classical program:** Input → [AND gate] → [NOT gate] → [OR gate] → Output
- **Quantum circuit:** |00⟩ → [H gate] → [CNOT gate] → [Measure] → Classical result

### Key Components of Quantum Circuits

| Component | Example | Description |
|-----------|---------|-------------|
| **Qubit line** | `q0: ─────` | A horizontal wire representing one qubit over time (flows left to right) |
| **Single-qubit gate** | `q0: ─[H]─` | A quantum gate (H, X, Z, etc.) acting on one qubit |
| **Two-qubit gate** | `q0: ─■─` <br> `q1: ─X─` | CNOT with control (●) on q0 and target (X) on q1 |
| **Measurement** | `q0: ─[M]─` | Read the qubit state and store in a classical bit |
| **Classical bit** | `c0: ═════` | Double-line wire for classical data output |

### Understanding Circuit Metrics

Every quantum circuit has measurable properties:

- **Width:** The number of qubits (how many quantum wires). More qubits = more quantum parallelism, but also more hardware requirements.

- **Depth:** The number of sequential time steps or "layers" (from left to right). A layer is a set of gates that can execute in parallel. Depth determines runtime — higher depth means more time for errors to accumulate on real hardware.

- **Gate count:** Total number of operations. More gates = more opportunities for errors.

- **Two-qubit gate count:** This is the most important metric. Two-qubit gates (like CNOT) are much noisier than single-qubit gates on real quantum hardware.

### Why Circuits Matter

Circuits are the **universal language** of quantum computing. Whether you use IBM Qiskit, Google Cirq, Rigetti pyQuil, or Amazon Braket, you describe your quantum algorithm as a circuit. Understanding how to compose, optimize, and analyze circuits is essential for every quantum programmer.

---

## Walkthrough: Reading and Building Circuits

### Example 1: Bell State Circuit (Creating Entanglement)

This is the simplest meaningful circuit. It prepares the Bell state |Φ⁺⟩ from Day 2.

```
     ┌───┐
q0: ─┤ H ├──■──
     └───┘┌─┴─┐
q1: ──────┤ X ├
          └───┘
```

**Reading this diagram (left to right, top to bottom):**

1. **Initial state:** Both qubits start in |0⟩. Total state: |00⟩

2. **Apply Hadamard (H) to qubit 0 (q0):**
   - The H gate creates a superposition
   - State becomes: (1/√2)(|00⟩ + |10⟩)
   - Interpretation: q0 is now 50% |0⟩ and 50% |1⟩, while q1 stays |0⟩

3. **Apply CNOT with q0 as control, q1 as target (X):**
   - If q0 is |0⟩, q1 stays |0⟩ → stays in |00⟩
   - If q0 is |1⟩, q1 flips to |1⟩ → becomes |11⟩
   - State becomes: (1/√2)(|00⟩ + |11⟩) = |Φ⁺⟩
   
4. **Result:** The two qubits are now **entangled** — they always measure to the same value!

---

### Example 2: Swap Two Qubits Using CNOT Decomposition

Sometimes you want to swap the values of two qubits. On some quantum hardware, SWAP isn't a native gate, so you decompose it into three CNOTs:

```
q0: ─■──X──■──
     │  │  │
q1: ─X──■──X──
```

**How it works:**
- The pattern: CNOT(0,1) → CNOT(1,0) → CNOT(0,1)
- This achieves the same effect as a SWAP gate
- Useful on real hardware where SWAP is slow or unavailable

---

### Example 3: GHZ State (3-Qubit Entanglement)

The GHZ state extends Bell state entanglement to three qubits:

$$|\text{GHZ}\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)$$

This creates a state where all three qubits are entangled together — either all 0 or all 1.

```
     ┌───┐
q0: ─┤ H ├──■──────■──
     └───┘┌─┴─┐    │
q1: ──────┤ X ├────■──
          └───┘┌───┴─┐
q2: ───────────┤ X   ├
              └──────┘
```

**Step-by-step:**

1. Apply H to q0: (1/√2)(|000⟩ + |100⟩)
2. Apply CNOT(q0, q1): (1/√2)(|000⟩ + |110⟩)
3. Apply CNOT(q0, q2): (1/√2)(|000⟩ + |111⟩) ← All three qubits entangled!

Now when you measure q0, it instantly determines q1 and q2. If q0 is 0, they're all 0. If q0 is 1, they're all 1.

---

## Practical Exercise: Build Circuits with Qiskit

Let's write real code using IBM's **Qiskit**, the most popular quantum programming framework. This will make the circuits concrete and runnable.

### Setup (One-time installation)

```bash
pip install qiskit qiskit-aer matplotlib
```

### Building a Bell State Circuit

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_histogram, circuit_drawer
import matplotlib.pyplot as plt

import io
import sys
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')


# Create quantum and classical registers
qubits = QuantumRegister(2, 'q')      # 2 quantum bits
cbits = ClassicalRegister(2, 'c')     # 2 classical bits to store measurement results

# Create the circuit
circ = QuantumCircuit(qubits, cbits)

# Build the Bell state circuit
circ.h(0)              # Hadamard on qubit 0 → creates superposition
circ.cx(0, 1)          # CNOT: control=q0, target=q1 → creates entanglement
circ.measure([0, 1], [0, 1])  # Measure both qubits into classical bits

# Display the circuit
print(circ)

# Run on simulator
simulator = AerSimulator()
job = simulator.run(circ, shots=1024)  # Run 1024 times
result = job.result()
counts = result.get_counts()

# Print and visualize results
print("\nMeasurement outcomes (1024 shots):")
print(counts)

# Expected: ~512 counts of '00' and ~512 counts of '11'
# You should NOT see '01' or '10' — that's the entanglement!

plot_histogram(counts)
plt.show()
```

**What to expect:**
- You'll see approximately 50% `00` outcomes and 50% `11` outcomes
- You will **never** see `01` or `10`
- This proves the qubits are entangled — they're correlated!

### Challenge 1: Build the 3-Qubit GHZ Circuit

Extend the Bell state circuit to three qubits:

```python
import io
import sys
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

# Challenge 1: GHZ State (3-qubit entanglement)
ghz_qubits = QuantumRegister(3, 'q')
ghz_cbits = ClassicalRegister(3, 'c')
ghz_circuit = QuantumCircuit(ghz_qubits, ghz_cbits)

# Build it yourself! Hint: H on q0, then CNOT(q0,q1), then CNOT(q0,q2)
ghz_circuit.h(0)
ghz_circuit.cx(0, 1)
ghz_circuit.cx(0, 2)
ghz_circuit.measure([0, 1, 2], [0, 1, 2])

print(ghz_circuit)

# Run and check
simulator = AerSimulator()
job = simulator.run(ghz_circuit, shots=1024)
counts = job.result().get_counts()

print("\nGHZ State Results:")
print(counts)
# Expected: ~50% '000' and ~50% '111', nothing else!

plot_histogram(counts)
plt.show()
```

### Challenge 2: SWAP Decomposition

Verify that three CNOTs can swap two qubits:

```python
import io
import sys
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')

from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

# Challenge 2: SWAP Decomposition
swap_qubits = QuantumRegister(2, 'q')
swap_cbits = ClassicalRegister(2, 'c')
swap_circuit = QuantumCircuit(swap_qubits, swap_cbits)

# Set initial state: q0 = 1, q1 = 0
swap_circuit.x(0)              # X gate flips q0 to |1>

# Apply SWAP decomposition: CNOT(0,1) → CNOT(1,0) → CNOT(0,1)
swap_circuit.cx(0, 1)
swap_circuit.cx(1, 0)
swap_circuit.cx(0, 1)
swap_circuit.measure([0, 1], [0, 1])

print(swap_circuit)

# Run and check
simulator = AerSimulator()
job = simulator.run(swap_circuit, shots=1024)
counts = job.result().get_counts()

print("\nSWAP Results:")
print(counts)
# Expected: All outcomes should be '01'
# (q0 started as 1, q1 started as 0, they swapped!)

plot_histogram(counts)
plt.show()
```

---

## Hand Calculation Exercise

### Problem 1: Draw a Circuit for Ψ⁺ State

Draw the circuit diagram (on paper or in text) for preparing the Bell state:

$$|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$$

Starting from |00⟩.

**Hint:** Use X gate on q1 first, then Hadamard on q0, then CNOT.

<details>
<summary>Click for solution</summary>

**Solution 1:**

Circuit diagram:
```
q0: ──────────[H]──■──
              
q1: ──[X]──────────X──
```

**Step-by-step state evolution:**

| Step | Operation | State |
|------|-----------|-------|
| 0 | Initial | \|00⟩ |
| 1 | X on q1 | \|01⟩ |
| 2 | H on q0 | (1/√2)(\|01⟩ + \|11⟩) |
| 3 | CNOT(q0,q1) | (1/√2)(\|01⟩ + \|10⟩) = \|Ψ⁺⟩ ✓ |

**Why it works:** The X gate on q1 flips it to |1⟩. Then H creates a superposition on q0. When CNOT executes:
- If q0 is |0⟩, q1 stays |1⟩ → results in |01⟩
- If q0 is |1⟩, q1 flips from |1⟩ to |0⟩ → results in |10⟩

Final state: (1/√2)(|01⟩ + |10⟩) — the opposite pattern from Φ⁺!

</details>

### Problem 2: Understand Circuit Metrics

A circuit has width 5 and depth 8. What do these numbers physically mean?

<details>
<summary>Click for solution</summary>

**Solution 2:**

- **Width = 5:** The circuit uses 5 qubits. This means:
  - You need a 5-qubit quantum processor
  - More qubits means potential for more quantum parallelism
  - More qubits on real hardware = more noise and errors

- **Depth = 8:** The longest path through the circuit has 8 sequential gate layers. This means:
  - Even with parallel execution, the circuit takes at least 8 time steps
  - On real hardware, each time step accumulates error
  - Higher depth = longer runtime and more accumulated noise
  - To minimize error, we want to minimize depth

**Real-world impact:** A 5-qubit 8-depth circuit is moderate. If it were 5-qubit 100-depth, you'd lose most quantum information to noise on today's hardware.

</details>

### Problem 3: Trace Through a Circuit

Manually trace through this circuit. Start with |00⟩, apply the gates step-by-step, and write the state after each operation:

```
     ┌───┐
q0: ─┤ H ├──■──
     └───┘┌─┴─┐
q1: ──────┤ X ├
          └───┘
```

<details>
<summary>Click for solution</summary>

**Solution 3:**

**Step 0 (Initial):** |00⟩

**Step 1 (H on q0):**
- Hadamard on q0: (1/√2)(|0⟩ + |1⟩)
- q1 unchanged: |0⟩
- Combined: (1/√2)(|0⟩|0⟩ + |1⟩|0⟩) = (1/√2)(|00⟩ + |10⟩)

**Step 2 (CNOT with q0 control, q1 target):**
- For |00⟩ term: q0=0 (control is 0), so no flip on q1 → stays |00⟩
- For |10⟩ term: q0=1 (control is 1), so q1 flips: |0⟩ → |1⟩ → becomes |11⟩
- Final: (1/√2)(|00⟩ + |11⟩) = |Φ⁺⟩

**Result:** Bell state! The two qubits are perfectly entangled.

</details>

---

## Key Takeaways

- **Circuits are the blueprint for quantum algorithms:** They describe the sequence of operations needed to solve a problem. Understanding circuits is the first step to quantum programming.

- **Time flows left to right:** Read circuits from left to right. Vertical position represents different qubits. Gates that are vertically aligned can execute in parallel.

- **Width and depth matter for real hardware:** Width determines qubit count requirements. Depth determines runtime and error accumulation. Optimizing depth is critical for near-term quantum computers.

- **Entanglement requires multi-qubit gates:** Single-qubit gates (H, X, Z, S, T) can create superposition but only multi-qubit gates (CNOT, SWAP) can create entanglement.

- **CNOT is the workhorse:** The CNOT gate appears in almost every quantum algorithm. Mastering CNOT is essential.

- **Measurement collapses the superposition:** Once you measure, the quantum state becomes classical. The order of measurement matters!

- **Qiskit makes circuits executable:** You can design a circuit and immediately run it on a simulator or (after queuing) on real IBM quantum hardware.

---

## Preview for Tomorrow

Tomorrow we'll explore **Measurement and Quantum Measurement Postulates** — what physically happens when you measure a qubit, how superposition collapses, and how to use measurement strategically in quantum algorithms (conditional logic, error detection, etc.).

We'll also cover mid-circuit measurement — measuring some qubits partway through a circuit and using those results to decide what gates to apply next!

---

**Ready for tomorrow?** Measurement is where the "quantum magic" meets classical information. Understanding it is crucial for real quantum programming!

---

## Appendix: Standard Circuit Symbol Reference

For reference, here are the standard symbols you'll see in quantum circuits:

| Gate | Symbol | Effect |
|------|--------|--------|
| **Hadamard** | `[H]` | Creates superposition |
| **Pauli-X** | `[X]` | Flips \|0⟩ ↔ \|1⟩ |
| **Pauli-Y** | `[Y]` | Complex phase + flip |
| **Pauli-Z** | `[Z]` | Applies phase to \|1⟩ |
| **S gate** | `[S]` | 90° phase rotation |
| **T gate** | `[T]` | 45° phase rotation |
| **CNOT** | `■ (control), ⊕ (target)` | Controlled flip |
| **Measurement** | `[M]` | Read qubit into classical bit |

