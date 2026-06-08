# Day 5: Measurement, Post-Selection & Fibonacci Review

> **Goal:** Understand how quantum measurement collapses a superposition, learn to measure in different bases, use mid-circuit measurement and classical feed-forward, apply post-selection to shape outcomes, and reinforce Days 1–4 with spaced repetition.

---

## Fibonacci-Spaced Review (Days 1–4)

| Interval | Day | Core Idea | Why It Matters Today |
|----------|-----|-----------|----------------------|
| **1 day ago** | Day 1 – Qubits & Superposition | Qubit = α\|0⟩ + β\|1⟩, with \|α\|² + \|β\|² = 1. Superposition lets a qubit hold both 0 and 1 simultaneously. | Measurement **projects** this superposition onto exactly one of \|0⟩ or \|1⟩. The amplitudes α and β determine the probabilities. |
| **1 day ago** | Day 2 – Entanglement | Bell states like \|Φ⁺⟩ = (1/√2)(\|00⟩ + \|11⟩). Measuring one qubit instantly determines the other. | Measuring one half of an entangled pair collapses the other — the cornerstone of post-selection and quantum teleportation. |
| **2 days ago** | Day 3 – Quantum Gates | Single-qubit gates (X, Z, H), two-qubit CNOT. Gates are reversible unitary matrices. | The gates you apply **before** measurement set the amplitudes that measurement will sample. Unlike gates, measurement is **irreversible**. |
| **3 days ago** | Day 4 – Circuits & Diagrams | A circuit = ordered gates + measurement. Width = qubit count, depth = sequential layers. Qiskit lets you build and simulate. | Today you'll extend circuits with measurement operations, mid-circuit measurement, and classical conditional gates. |

> **Spaced Repetition Schedule:** Re-read the Day 1 and Day 2 rows tomorrow. Re-read Day 3 the day after. Re-read Day 4 in three days. Reviewing at these intervals reinforces long-term retention with minimal effort.

---

## Theoretical Foundation: Quantum Measurement

### 1. What Is Measurement and Why Is It Special?

Quantum measurement is fundamentally different from any classical operation or quantum gate. Here is why:

| Property | Quantum Gates | Quantum Measurement |
|----------|--------------|---------------------|
| **Reversible?** | Yes — every gate has an inverse | No — you cannot undo a measurement |
| **Deterministic?** | Yes — same input always gives same output | No — outcome is **random**, governed by probability |
| **Preserves superposition?** | Yes — superposition evolves but persists | No — superposition **collapses** to one definite state |
| **Destroys information?** | No | Yes — all other amplitude information is lost |

This irreversibility is not a limitation of our technology — it is a fundamental feature of quantum mechanics. Once you look at a qubit, the superposition is gone forever.

### 2. The Born Rule: How Probabilities Are Calculated

The **Born rule** is the fundamental postulate that connects quantum amplitudes to measurable probabilities. It was formulated by Max Born in 1926 and is the core of quantum mechanics.

For a qubit in state:

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$

The Born rule states:

$$P(\text{outcome} = 0) = |\alpha|^2$$

$$P(\text{outcome} = 1) = |\beta|^2$$

And since one outcome must occur:

$$|\alpha|^2 + |\beta|^2 = 1$$

**After measurement, the state collapses:**

- If you measure and get **0** → the qubit is now definitively in state |0⟩. All information about β is lost.
- If you measure and get **1** → the qubit is now definitively in state |1⟩. All information about α is lost.

**Why does this matter?** Because you can only extract one bit of classical information from one qubit measurement, no matter how much quantum information the qubit contained. This is why quantum computing is not about "storing exponentially more data" — it is about using interference and entanglement cleverly before measurement.

### 3. Measurement Destroys Superposition — A Concrete Example

Suppose a qubit is in the equal superposition:

$$|\psi\rangle = \frac{1}{\sqrt{2}}|0\rangle + \frac{1}{\sqrt{2}}|1\rangle$$

**Before measurement:** The qubit genuinely exists in both |0⟩ and |1⟩ simultaneously. Both amplitudes are active and can interfere with each other if more gates are applied.

**At measurement:** You randomly get either 0 (50%) or 1 (50%).

**After measurement:** Say you got 0. The state is now |0⟩. The amplitude for |1⟩ is gone. If you measure again immediately, you get 0 with 100% certainty — not 50%. The superposition is permanently destroyed.

This is called **wavefunction collapse**.

---

### 4. Measurement Bases: Z-Basis, X-Basis, and Y-Basis

A critical and often overlooked concept: **the outcome of a measurement depends on which basis you measure in.** The computational basis (|0⟩ and |1⟩) is not the only option.

#### Z-Basis (Standard / Computational Basis)

The default. Measures whether the qubit is |0⟩ or |1⟩.

- **Basis states:** |0⟩ and |1⟩ (eigenstates of the Pauli-Z gate)
- **Circuit:** Measure directly with no preceding gate

```
q0: ──[any gates]──[M]──
                    |
                    z-basis measurement
```

For the state |+⟩ = (1/√2)(|0⟩ + |1⟩), a Z-basis measurement gives:
- P(0) = 1/2
- P(1) = 1/2

#### X-Basis (Hadamard Basis)

Measures whether the qubit aligns with |+⟩ = (1/√2)(|0⟩ + |1⟩) or |−⟩ = (1/√2)(|0⟩ − |1⟩).

- **Basis states:** |+⟩ and |−⟩ (eigenstates of the Pauli-X gate)
- **Circuit:** Apply Hadamard gate **before** measuring in Z-basis

```
q0: ──[any gates]──[H]──[M]──
                         |
                         z-basis measurement, but in x-basis frame
```

**Why use X-basis?** Because |+⟩ and |−⟩ are eigenstates of the X gate. If you measure |+⟩ in the X-basis, you always get outcome 0 (definite result, no randomness). This is the basis used in quantum key distribution (BB84 protocol).

For the state |+⟩ = (1/√2)(|0⟩ + |1⟩), an X-basis measurement gives:
- P(+) = 1 → always get "0" (certain result)
- P(−) = 0 → never get "1"

#### Y-Basis

Measures along the Y axis of the Bloch sphere.

- **Basis states:** |i+⟩ = (1/√2)(|0⟩ + i|1⟩) and |i−⟩ = (1/√2)(|0⟩ − i|1⟩)
- **Circuit:** Apply S† gate then Hadamard gate before measuring

```
q0: ──[any gates]──[S†]──[H]──[M]──
```

**Summary of measurement bases:**

| Basis | Distinguishes between | Gate before measuring |
|-------|----------------------|----------------------|
| **Z-basis** | \|0⟩ and \|1⟩ | None (direct measurement) |
| **X-basis** | \|+⟩ and \|−⟩ | H gate |
| **Y-basis** | \|i+⟩ and \|i−⟩ | S† then H gate |

**The key insight:** Applying a gate before measuring is equivalent to rotating your frame of reference. You are still physically measuring in the Z-basis, but you have rotated the qubit first so you are effectively measuring a different aspect of its state.

---

### 5. Partial Measurement of Multi-Qubit Systems

When you have multiple entangled qubits and measure only one, something important happens to the remaining qubits.

**Scenario:** Two qubits in the Bell state |Φ⁺⟩ = (1/√2)(|00⟩ + |11⟩). You measure only qubit 0.

**Step-by-step collapse:**

1. **Before measurement:** The system is in (1/√2)(|00⟩ + |11⟩). Both qubits are in superposition. Neither has a definite value.

2. **Measure qubit 0, get outcome 0:**
   - The |11⟩ term is eliminated (it required qubit 0 to be 1)
   - The |00⟩ term survives
   - After renormalization (dividing by the probability amplitude): the full system collapses to |00⟩
   - **Qubit 1 is now definitively |0⟩** — even though you never touched it

3. **Measure qubit 0, get outcome 1:**
   - The |00⟩ term is eliminated
   - System collapses to |11⟩
   - **Qubit 1 is now definitively |1⟩**

**The rule:** Measuring one qubit in an entangled system collapses the **entire** multi-qubit state. The unmeasured qubits are left in definite states (or reduced mixed states for partially entangled systems).

```
Before measuring q0:         After measuring q0 = 0:
                             
   (1/√2)(|00⟩ + |11⟩)  →  |00⟩
                             
q0: ──[H]──■──[M=0]──       q0 → now definite: 0
           |                 
q1: ───────X──(?)──         q1 → now definite: 0 (instantly)
```

This is why measuring one qubit of an entangled pair "instantly" determines the other — the correlation was always encoded in the joint quantum state.

---

### 6. POVMs: Measuring Beyond the Computational Basis

> **POVM stands for Positive Operator-Valued Measure.** This section expands on the "quick note" from the original file.

The computational basis measurement (|0⟩/|1⟩) is just one special case of a much broader class of measurements called **POVMs**.

**Why do POVMs exist?**

Standard projective measurements (Z-basis) have a limitation: after measurement, the qubit is always in |0⟩ or |1⟩, destroying any prior quantum information. Sometimes we want to extract partial information without fully collapsing the state. POVMs make this possible.

**How they work:**

A POVM is a set of measurement operators {M₁, M₂, ..., Mₙ} where:
- Each operator Mᵢ is positive semi-definite
- They sum to the identity: M₁ + M₂ + ... + Mₙ = I
- P(outcome i) = ⟨ψ|Mᵢ|ψ⟩

**Practical implementation of POVMs:**

Any POVM can be implemented by:
1. Adding one or more **ancilla qubits** (helper qubits)
2. Applying a **unitary** to the system + ancilla
3. Measuring the ancilla in the computational basis

```
System qubit: ──────────[U (entangling)]──── (system after measurement)
                                |
Ancilla qubit: ──|0⟩──────────────────────[M]── → classical outcome
```

**Everyday example of a POVM:** The X-basis measurement we described above is actually a POVM implemented by rotating with H and then measuring.

**When do POVMs matter in practice?**
- **Quantum key distribution:** Distinguishing between non-orthogonal states
- **Quantum state discrimination:** Identifying which of several non-orthogonal states was prepared
- **Quantum error correction:** Syndrome measurements that detect errors without reading data
- **Quantum sensing:** Extracting maximum information about a physical parameter

---

### 7. Post-Selection

**Post-selection** means running an experiment many times, then discarding all runs that did not produce a desired measurement outcome, keeping only the "successful" runs.

**Why is it useful?**

Post-selection lets you:
- Simulate rare or specific quantum states without needing feed-forward hardware
- Isolate specific outcomes for analysis
- Implement certain protocols (e.g., linear optical quantum computing relies heavily on post-selection)

**Why are there limitations?**

- **Efficiency cost:** If you only keep 10% of runs, you need 10× more shots to get the same statistics
- **Not scalable:** Real algorithms cannot discard 99% of runs — post-selection is primarily a research and simulation tool
- **Cannot create faster-than-light communication:** Even with post-selection, the selected outcomes look random to an outside observer without a classical communication channel

**Concrete example:**

You want to study the |00⟩ state from a Bell pair, but you cannot reliably prepare it directly. Instead:
1. Prepare |Φ⁺⟩ = (1/√2)(|00⟩ + |11⟩)
2. Run 1000 shots measuring qubit 0
3. Keep only the ~500 runs where qubit 0 measured as 0
4. In those runs, qubit 1 is guaranteed to be 0 → you have |00⟩

This is post-selection: you used the measurement outcome as a filter.

---

### 8. Mid-Circuit Measurement and Classical Feed-Forward

Mid-circuit measurement and classical feed-forward are key features of advanced quantum circuits and real quantum error correction.

#### What Is Mid-Circuit Measurement?

**Standard circuit:** All gates → Measure all qubits at the end

**Mid-circuit measurement:** Gates → **Measure some qubits partway through** → More gates → Measure remaining qubits

```
Standard circuit:                     Mid-circuit measurement:

q0: ──[H]──■─────────[M]──          q0: ──[H]──■──[M]──[X if c0=1]──[M]──
           │                                    │
q1: ───────X─────────[M]──          q1: ───────X────────────────────[M]──
```

**Why is this useful?**

1. **Qubit reset and reuse:** Measure a qubit, reset it to |0⟩, and use it again. This reduces the total qubit count needed.
2. **Error detection:** Measure "syndrome" qubits mid-circuit to detect errors without disturbing data qubits.
3. **Adaptive circuits:** Use measurement outcomes to decide what gate to apply next.

#### What Is Classical Feed-Forward?

Classical feed-forward means using a measurement result (a classical bit) to **conditionally control** a quantum gate applied later in the circuit.

```
q0: ──[H]──[M]─────────────────────────
            │ (classical bit c0)
            └──── if c0 == 1: apply X ──► q1: ──────────────[conditional X]──[M]──
```

**How it differs from quantum control:**
- Classical feed-forward uses a **measured classical bit** to control a gate. The decision is classical — no quantum superposition of "apply / do not apply."
- Quantum controlled gates (like CNOT) keep both control options in superposition simultaneously.

**Real-world use case — Quantum Teleportation:**

Quantum teleportation is the most famous example of classical feed-forward:
1. Alice and Bob share an entangled pair
2. Alice has a qubit |ψ⟩ she wants to teleport to Bob
3. Alice applies a Bell measurement (H + CNOT + measure both qubits)
4. Alice gets two classical bits from her measurement
5. Alice **sends those two bits to Bob over a classical channel**
6. Bob applies corrections based on those bits: if bit 0 = 1, apply X; if bit 1 = 1, apply Z
7. Bob's qubit is now in state |ψ⟩ — Alice's original state

This is exactly classical feed-forward. The teleported state is only correctly reconstructed after Bob applies the classically communicated corrections.

```
Alice's side:               Bob's side:
|ψ⟩──────────●──[H]──[M]──c0──────────────────────────► if c0=1: Z
              │             c1──────────────────────────► if c1=1: X
|Φ⁺⟩ pair:   X──────────[M]──►                                     │
                                                                    ▼
                                                              Bob's qubit = |ψ⟩
```

---

## Walkthrough: Measuring an Entangled Pair Step by Step

**Setup:** Two qubits prepared in the Bell state |Φ⁺⟩. We measure only the first qubit.

**Circuit:**

```
     ┌───┐        ┌───┐
q0: ─┤ H ├──■─────┤ M ├──── c0 (classical bit)
     └───┘┌─┴─┐  └───┘
q1: ──────┤ X ├──────────── (still quantum, affected by q0's measurement)
          └───┘
```

**Step-by-step state evolution:**

| Step | Operation | System State | Interpretation |
|------|-----------|--------------|----------------|
| 0 | Initial | \|00⟩ | Both qubits definite |
| 1 | H on q0 | (1/√2)(\|00⟩ + \|10⟩) | q0 in superposition, q1 still \|0⟩ |
| 2 | CNOT(q0,q1) | (1/√2)(\|00⟩ + \|11⟩) | Both qubits entangled — Bell state |
| 3 | Measure q0 = 0 (50% chance) | \|00⟩ | q0 collapsed to 0, q1 instantly became 0 |
| 3 | Measure q0 = 1 (50% chance) | \|11⟩ | q0 collapsed to 1, q1 instantly became 1 |

**Post-selection:** If we keep only the runs where q0 measured as 0, every run in our filtered set has q1 also as 0. We now have a deterministic |00⟩ state selected from entanglement.

---

## Practical Exercise: Measurement in Qiskit

### Exercise 1: Bell State with Partial Measurement and Post-Selection

```python
# Day 5 Exercise 1 — Partial Measurement and Post-Selection
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

# --- Build the Bell state circuit ---
qc = QuantumCircuit(2, 2)   # 2 qubits, 2 classical bits
qc.h(0)                     # Hadamard on q0: creates superposition
qc.cx(0, 1)                 # CNOT: creates entanglement between q0 and q1

# Measure ONLY qubit 0 first (partial measurement)
qc.measure(0, 0)            # q0 → classical bit c0

# Measure qubit 1 separately
qc.measure(1, 1)            # q1 → classical bit c1

print("Circuit:")
print(qc)

# --- Simulate ---
simulator = AerSimulator()
job = simulator.run(qc, shots=4096)
result = job.result()
counts = result.get_counts()

print("\nAll measurement outcomes:")
print(counts)
# Expected: Only '00' and '11' — never '01' or '10'

# --- Post-selection: keep only runs where c0 = 0 ---
# In Qiskit, bit strings are ordered right-to-left: "c1c0"
post_selected = {
    state: count
    for state, count in counts.items()
    if state[-1] == '0'  # c0 is the rightmost bit
}

total = sum(post_selected.values())
print(f"\nPost-selected outcomes (kept {total} of 4096 runs where c0 = 0):")
print(post_selected)
# Expected: Only '00' — confirming q1 collapsed to 0 when q0 measured 0

# --- Visualize ---
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))
plot_histogram(counts, ax=ax1, title='All outcomes')
plot_histogram(post_selected, ax=ax2, title='Post-selected (c0 = 0)')
plt.tight_layout()
plt.show()
```

**What to observe:**
- `counts` should show roughly 50% `00` and 50% `11` — never `01` or `10`
- `post_selected` should show only `00` — because when q0 = 0, q1 is guaranteed to be 0

---

### Exercise 2: X-Basis Measurement

Measuring in a different basis reveals different information about the same qubit state.

```python
# Day 5 Exercise 2 — X-Basis Measurement
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

# --- Prepare |+> state ---
qc_z = QuantumCircuit(1, 1)
qc_z.h(0)                   # Prepare |+> = (|0> + |1>)/sqrt(2)
qc_z.measure(0, 0)          # Measure in Z-basis (standard)

# --- Same state, X-basis measurement ---
qc_x = QuantumCircuit(1, 1)
qc_x.h(0)                   # Prepare |+>
qc_x.h(0)                   # Rotate to X-basis frame BEFORE measuring
qc_x.measure(0, 0)          # This is now an X-basis measurement

simulator = AerSimulator()

# Run Z-basis
counts_z = simulator.run(qc_z, shots=1024).result().get_counts()
# Run X-basis
counts_x = simulator.run(qc_x, shots=1024).result().get_counts()

print("Z-basis measurement of |+> state:")
print(counts_z)
# Expected: ~50% '0', ~50% '1' — maximally uncertain in Z-basis

print("\nX-basis measurement of |+> state:")
print(counts_x)
# Expected: 100% '0' — |+> is a definite eigenstate in X-basis!

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 4))
plot_histogram(counts_z, ax=ax1, title='Z-basis measurement of |+>')
plot_histogram(counts_x, ax=ax2, title='X-basis measurement of |+>')
plt.tight_layout()
plt.show()
```

**Key insight:** The |+⟩ state is maximally uncertain in the Z-basis (50/50) but is a **definite** eigenstate in the X-basis (always 0). This demonstrates that information in a quantum state is basis-dependent.

---

### Exercise 3: Mid-Circuit Measurement with Classical Feed-Forward

```python
# Day 5 Exercise 3 — Mid-Circuit Measurement and Classical Feed-Forward
from qiskit import QuantumCircuit, ClassicalRegister, QuantumRegister
from qiskit_aer import AerSimulator

# Demonstrate classical feed-forward:
# - Prepare q0 in superposition
# - Measure q0 mid-circuit
# - If result is 1, apply X to q1 (feed-forward correction)
# - This guarantees q1 always ends up in |0>

qr = QuantumRegister(2, 'q')
cr = ClassicalRegister(2, 'c')
qc = QuantumCircuit(qr, cr)

# Step 1: Prepare q0 in superposition
qc.h(0)

# Step 2: Mid-circuit measurement of q0
qc.measure(0, 0)            # Result stored in classical bit c0

# Step 3: Feed-forward — apply X to q1 if c0 = 1
# This corrects q1 based on what q0 measured
with qc.if_test((cr[0], 1)):
    qc.x(1)

# Step 4: Measure q1
qc.measure(1, 1)

print("Mid-circuit measurement circuit:")
print(qc)

# Run simulator
simulator = AerSimulator()
job = simulator.run(qc, shots=2048)
counts = job.result().get_counts()

print("\nResults (c1 c0 format):")
print(counts)
# Expected: Only '00' and '11' — when q0=1, X was applied to q1 making it 1
# Without the feed-forward, q1 would always be 0

# Try removing the if_test block and compare — q1 would always measure 0
```

**What this demonstrates:** The classical bit from measuring q0 is used to decide whether to apply a gate to q1. This is classical feed-forward in action — a fundamental technique in quantum error correction and quantum teleportation.

---

## Hand-Calculation Exercises

### Problem 1: Born Rule and Collapse

A qubit is in state:

$$|\psi\rangle = \frac{3}{5}|0\rangle + \frac{4}{5}|1\rangle$$

**(a)** What is the probability of measuring 0?

**(b)** What is the probability of measuring 1?

**(c)** Verify normalization.

**(d)** After measuring and getting outcome 1, what is the new state of the qubit?

**(e)** If you measure the qubit again immediately after (d), what outcome do you get and with what probability?

<details>
<summary>Click for solution</summary>

**(a)** P(0) = |3/5|² = 9/25 = **0.36 (36%)**

**(b)** P(1) = |4/5|² = 16/25 = **0.64 (64%)**

**(c)** Normalization check: 9/25 + 16/25 = 25/25 = 1 ✓

**(d)** After measuring outcome 1, the state collapses to **|1⟩**. The amplitude 3/5 for |0⟩ is gone. The new state is |1⟩ (with amplitude 1, since it must be normalized).

**(e)** You get outcome **1 with probability 100%**. The qubit is now in definite state |1⟩ — no superposition remains. Measuring a collapsed state always gives the same result.

**Key lesson:** Measurement is irreversible. You cannot get back the original 36/64 split after collapse.

</details>

---

### Problem 2: Partial Measurement of Entangled Pair

The Bell state is:

$$|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$

You measure the **second** qubit and obtain outcome 1.

**(a)** Which term in the superposition survives?

**(b)** What is the post-measurement state of the full two-qubit system?

**(c)** What is the state of the first qubit after this measurement?

**(d)** What would happen if you then measured the first qubit?

<details>
<summary>Click for solution</summary>

**(a)** You got outcome 1 on the second qubit. Only the term |11⟩ has second qubit = 1. The |00⟩ term (second qubit = 0) is eliminated.

**(b)** The surviving term is (1/√2)|11⟩. After renormalization (divide by the probability amplitude 1/√2), the full post-measurement state is **|11⟩**.

**(c)** The first qubit is now in the definite state **|1⟩**. Even though you never measured or touched qubit 1, it collapsed to |1⟩ the moment you measured qubit 2. This is the entanglement correlation.

**(d)** Measuring the first qubit would give outcome **1 with probability 100%**. The system is already in the definite state |11⟩, so there is no randomness remaining.

**Key lesson:** Partial measurement of one qubit in an entangled system collapses the entire joint state.

</details>

---

### Problem 3: Measurement Basis Choice

A qubit is prepared in state |−⟩ = (1/√2)(|0⟩ − |1⟩).

**(a)** If you measure in the Z-basis, what are the probabilities of each outcome?

**(b)** If you apply an H gate and then measure in the Z-basis (X-basis measurement), what do you get?

**(c)** Why does the basis choice change the outcome so dramatically?

<details>
<summary>Click for solution</summary>

**(a)** Z-basis measurement of |−⟩ = (1/√2)(|0⟩ − |1⟩):
- P(0) = |1/√2|² = **50%**
- P(1) = |−1/√2|² = **50%** (the minus sign disappears when squaring)

**(b)** Apply H to |−⟩:
- H|−⟩ = H · (1/√2)(|0⟩ − |1⟩)
- = (1/√2)(H|0⟩ − H|1⟩)
- = (1/√2)((1/√2)(|0⟩ + |1⟩) − (1/√2)(|0⟩ − |1⟩))
- = (1/2)((|0⟩ + |1⟩) − (|0⟩ − |1⟩))
- = (1/2)(2|1⟩)
- = **|1⟩**

Then measuring |1⟩ in Z-basis: **P(1) = 100%**, P(0) = 0%.

**(c)** The |−⟩ state is an eigenstate of the X gate (X-basis). In its own basis, it is definite — always gives "1" in X-basis. But in the Z-basis, it is maximally uncertain. The minus sign (relative phase) is invisible to Z-basis measurement but fully visible to X-basis measurement. This demonstrates that **phase information is only accessible through appropriate basis choice.**

</details>

---

## Key Takeaways

- **Measurement is irreversible:** Unlike gates, you cannot undo a measurement. The superposition collapses to a definite state and that quantum information is gone permanently.

- **The Born rule connects amplitudes to probabilities:** P(outcome i) = |amplitude of state i|². This is the fundamental postulate linking the quantum world to classical observation.

- **Measurement basis matters:** Measuring in the Z-basis, X-basis, or Y-basis reveals different aspects of the qubit state. Always choose the basis that matches the information you want to extract.

- **Partial measurement collapses the whole system:** Measuring one qubit in an entangled pair instantly determines the other, regardless of distance. No information travels — the correlation was encoded in the joint quantum state.

- **Post-selection is a filtering tool:** Discarding runs that give unwanted outcomes lets you study specific quantum states. It is powerful in research and simulation but costly in practice (fewer usable shots).

- **Mid-circuit measurement enables adaptive circuits:** Measuring partway through a circuit and using that result to control later gates is essential for quantum error correction, quantum teleportation, and fault-tolerant quantum computing.

- **Classical feed-forward bridges quantum and classical:** The outcome of a mid-circuit measurement is a classical bit that can control subsequent quantum gates. This is how quantum computers make real-time decisions during execution.

- **POVMs generalize measurement:** Beyond the standard Z-basis projective measurement, POVMs allow partial or indirect information extraction. Any POVM can be implemented by adding ancilla qubits and measuring them.

---

## Preview for Tomorrow (Day 6)

**Day 6 – Quantum Error Correction (Intro):** We'll explore how to protect fragile quantum information using the three-qubit bit-flip code. You'll learn what a stabilizer is, why quantum errors are more complex than classical bit errors, and build your first error-detecting circuit in Qiskit.

Mid-circuit measurement and classical feed-forward (from today!) are the core ingredients of every quantum error correction protocol — so today's material is directly foundational for Day 6.

---

## How to Use This File

- **Review table:** Read now, then revisit Day 1 and Day 2 rows tomorrow, Day 3 in two days, Day 4 in three days.
- **Exercises:** Run all three code exercises. The X-basis exercise (Exercise 2) is the most revealing.
- **Hand calculations:** Complete all three problems on paper before checking solutions.
- **Key connection:** Mid-circuit measurement + feed-forward (section 8) is the bridge to error correction on Day 6. Make sure you understand Exercise 3 before moving on.
