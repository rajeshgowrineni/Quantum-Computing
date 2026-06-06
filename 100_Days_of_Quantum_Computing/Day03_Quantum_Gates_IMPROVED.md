# Day 3: Quantum Gates

Welcome back! Today we learn how to **manipulate** qubits. Just as classical computers use logic gates (AND, OR, NOT) to process bits, quantum computers use **quantum gates** to transform qubits. These gates are the building blocks of every quantum algorithm.

---

## Theoretical Foundation: What Are Quantum Gates?

**Classical vs. Quantum Gates**

A classical gate transforms bits deterministically. A NOT gate flips 0 to 1 and 1 to 0.

A quantum gate transforms **qubit states** by applying a **unitary matrix** to the state vector. Because qubits can be in superposition, a single gate operation can simultaneously act on both |0> and |1> — this is the source of quantum parallelism.

**Key Properties of Quantum Gates**

1. **Reversibility:** Every quantum gate has an inverse (you can undo it), meaning no information is lost during computation. This is different from classical AND/OR gates, which collapse information and can't be reversed.

2. **Unitary:** The gate matrix must satisfy the unitary property (U† times U equals the identity matrix). In simpler terms: probabilities always stay normalized (they add up to 1) no matter what gate you apply.

3. **Linear:** If a gate acts on a superposition, it acts on each component of the superposition independently. This allows the parallelism that makes quantum computers powerful.

**Why This Matters**

Quantum gates allow us to:
- Create superposition from a definite state (using the Hadamard gate)
- Flip qubits (using the Pauli-X gate)
- Introduce phase (using the S and T gates)
- Entangle qubits (using the CNOT gate)
- And ultimately build quantum circuits that solve real problems

**Understanding Phase: Global vs. Relative**

This is crucial: there are two types of phase in quantum mechanics.

- **Global phase:** A phase applied to the entire state equally. Example: if both |0> and |1> get multiplied by i (the imaginary unit), the measurement probabilities don't change. Global phase is invisible.

- **Relative phase:** A phase difference between different parts of the superposition. Example: if |0> gets +1 but |1> gets -1, this creates interference patterns that affect measurements.

Think of it like two dancers: global phase means both move in sync together (looks the same). Relative phase means they move out of sync (creates interference patterns we can observe).

---

## Walkthrough: Essential Quantum Gates

**1. The Pauli-X Gate (NOT Gate)**

The simplest gate: flips |0> to |1> and |1> to |0>.

Matrix form:
X = [0  1]
    [1  0]

Effect:
- X|0> = |1>
- X|1> = |0>

**Analogy:** Exactly like a classical NOT gate, but it also works on superpositions. If you have (1/sqrt(2))(|0> + |1>), the X gate produces (1/sqrt(2))(|1> + |0>), which is the same state with the labels swapped.

**2. The Pauli-Z Gate (Phase Flip)**

Leaves |0> unchanged but flips the phase of |1>.

Matrix form:
Z = [1   0]
    [0  -1]

Effect:
- Z|0> = |0>
- Z|1> = -|1>  (the minus sign is a 180-degree phase flip)

**Why it matters:** Phase is where quantum algorithms get their power. The relative phase between amplitudes creates interference patterns. You can't measure a global phase directly, but relative phase determines how amplitudes interfere with each other.

**3. The Hadamard Gate (H)**

The star of quantum computing: creates superposition. This is the most important gate for quantum algorithms.

Matrix form:
H = (1/sqrt(2)) * [1   1]
                   [1  -1]

Effect:
- H|0> = (1/sqrt(2))(|0> + |1>)   [equal superposition]
- H|1> = (1/sqrt(2))(|0> - |1>)   [equal superposition with phase flip]

**Key insight:** The Hadamard is its own inverse. Apply it twice and you get back where you started: H(H|0>) = |0>.

**Visual representation:**

Input: |0>  -->  Apply H  -->  Output: |+> = (1/sqrt(2))(|0> + |1>)
Input: |1>  -->  Apply H  -->  Output: |-> = (1/sqrt(2))(|0> - |1>)

**4. The CNOT Gate (Controlled-NOT)**

The first multi-qubit gate. It creates entanglement.

How it works: CNOT has two qubits:
- The **control** qubit (usually the top one)
- The **target** qubit (usually the bottom one)

If the control qubit is |1>, the target qubit gets flipped. Otherwise, nothing happens.

Matrix form:
CNOT = [1  0  0  0]
       [0  1  0  0]
       [0  0  0  1]
       [0  0  1  0]

Examples:
- CNOT|00> = |00>    (control is 0, target unchanged)
- CNOT|01> = |01>    (control is 0, target unchanged)
- CNOT|10> = |11>    (control is 1, target flipped from 0 to 1)
- CNOT|11> = |10>    (control is 1, target flipped from 1 to 0)

**Creating Bell states:**

If you apply Hadamard to the first qubit, then CNOT with the first qubit as control:
- Start: |00>
- After H on first qubit: (1/sqrt(2))(|0> + |1>)|0> = (1/sqrt(2))(|00> + |10>)
- After CNOT: (1/sqrt(2))(|00> + |11>)

This is the Phi+ entangled Bell state from Day 2!

**5. The S and T Gates (Phase Gates)**

These gates add specific phase angles to the |1> state without touching |0>.

**S gate:**
S = [1  0]
    [0  i]

Effect: S|1> = i|1> (multiplies by i, a 90-degree phase rotation)

**T gate:**
T = [1      0    ]
    [0  e^(i*pi/4)]

Effect: Rotates |1> by 45 degrees

**Why they matter:** These gates allow fine-grained phase control. By themselves, they don't change measurement probabilities. But when combined with other gates (especially interference effects), they create the quantum advantage.

---

## Practical Exercise: Simulating Quantum Gates

Run this Python code to see quantum gates in action. Only NumPy is required.

```python
import numpy as np

# Define quantum gates as matrices
I = np.array([[1, 0], [0, 1]])                    # Identity (do nothing)
X = np.array([[0, 1], [1, 0]])                    # Pauli-X (NOT)
Z = np.array([[1, 0], [0, -1]])                   # Pauli-Z (Phase flip)
H = (1/np.sqrt(2)) * np.array([[1, 1], [1, -1]]) # Hadamard
S = np.array([[1, 0], [0, 1j]])                   # S gate (90 degree phase)

# Basis states as column vectors
ket0 = np.array([1, 0])
ket1 = np.array([0, 1])

def apply_gate(gate, state):
    """Apply a gate (matrix) to a quantum state (vector)."""
    new_state = gate @ state
    return new_state

def print_state(state, name):
    """Pretty-print a state as alpha|0> + beta|1>"""
    alpha, beta = state[0], state[1]
    print(f"{name} = {alpha:.3f}|0> + {beta:.3f}|1>")

# === Exercise 1: Flip a qubit ===
print("=== X Gate (NOT) ===")
print_state(ket0, "|0>")
state = apply_gate(X, ket0)
print_state(state, "X|0>")
print()

# === Exercise 2: Create superposition ===
print("=== H Gate (Hadamard) ===")
state_plus = apply_gate(H, ket0)
print_state(state_plus, "H|0> = |+>")

state_minus = apply_gate(H, ket1)
print_state(state_minus, "H|1> = |->")

# Verify: applying H twice returns you to start
back = apply_gate(H, state_plus)
print_state(back, "H|+> (back to original)")
print()

# === Exercise 3: Phase manipulation ===
print("=== Z Gate on superposition ===")
state1 = apply_gate(Z, state_plus)
print_state(state1, "Z|+>")
print("Notice: probabilities are the same (50-50),")
print("but the phase relationship changed!")
print()

# === Exercise 4: CNOT gate and Bell states ===
CNOT = np.array([
    [1, 0, 0, 0],
    [0, 1, 0, 0],
    [0, 0, 0, 1],
    [0, 0, 1, 0]
])

def tensor(a, b):
    """Tensor product (combines two single-qubit states into a two-qubit state)"""
    return np.kron(a, b)

print("=== CNOT Gate (Creating Bell States) ===")

# Create the Bell state: H(|0>)|0> then apply CNOT
first_qubit_superposition = apply_gate(H, ket0)
two_qubit_state = tensor(first_qubit_superposition, ket0)
bell_state = CNOT @ two_qubit_state

print(f"After H on first qubit, before CNOT: {two_qubit_state}")
print(f"After CNOT (Bell state): {bell_state}")
print("This is the |Phi+> entangled state from Day 2!")
```

**Your Turn:**

1. **Run the script** and verify each gate does what we described. Try changing input states.

2. **Compute HZH|0>** (apply H, then Z, then H to |0>). What gate does this sequence simulate? (Hint: it should be equivalent to X)

3. **Try applying the S gate** multiple times. S|1> = i|1>. What happens if you apply S four times?

---

## Hand Calculation Exercise

**Problem 1:** Apply the Hadamard gate to the state |0> by hand. Show the matrix multiplication step-by-step.

**Problem 2:** What is the result of applying X then H to |0>? (i.e., compute HX|0>)

**Problem 3:** Suppose you have a two-qubit system in state |10>. Write this as a 4x1 column vector and apply the CNOT matrix to find the output.

**Solutions:**

**Solution 1:**

H|0> = (1/sqrt(2)) * [1   1] * [1]
                      [1  -1]   [0]

     = (1/sqrt(2)) * [1*1 + 1*0]
                      [1*1 + (-1)*0]

     = (1/sqrt(2)) * [1]
                      [1]

This is the state (1/sqrt(2))(|0> + |1>), the equal superposition.

**Solution 2:**

First, apply X to |0>:
X|0> = |1>

Then, apply H to |1>:
H|1> = (1/sqrt(2))(|0> - |1>)

So HX|0> = (1/sqrt(2))(|0> - |1>), which is the |-> state.

**Solution 3:**

The state |10> (first qubit is 1, second is 0) corresponds to the column vector:
[0]
[0]
[1]
[0]

Applying CNOT:
[1  0  0  0]   [0]   [0]
[0  1  0  0] * [0] = [0]
[0  0  0  1]   [1]   [0]
[0  0  1  0]   [0]   [1]

The result is [0, 0, 0, 1], which represents |11>.

Why? The control qubit (first) is 1, so the target qubit (second) gets flipped from 0 to 1.

---

## Key Takeaways

- **Gates are unitary matrices:** Every gate has an inverse (you can undo it), which preserves total probability. No information is lost.

- **Hadamard creates superposition:** The H gate is your tool for entering the quantum realm. It turns definite states into equal superpositions.

- **CNOT creates entanglement:** Combined with H, the CNOT generates Bell states (from Day 2).

- **Phase is powerful but subtle:** Relative phase differences don't change measurement probabilities alone, but they create interference patterns that quantum algorithms exploit.

- **Gate order matters:** Applying gates in sequence means multiplying their matrices. Gates do NOT always commute — meaning AB is not always equal to BA. The order you apply gates changes the result.

- **Matrix multiplication is the key:** Understanding gates means understanding how matrix-vector multiplication works. This is the language of quantum mechanics.

---

## Preview for Tomorrow

Tomorrow we'll **build quantum circuits** — sequences of gates that perform real computations. We'll learn about:
- Circuit depth (how many layers of gates)
- Qubit connectivity (which qubits can interact)
- How to design circuits to solve problems
- Introduction to Qiskit for circuit visualization

---

**Ready for tomorrow?** We'll combine today's gates into full quantum programs and see the first real quantum algorithms!
