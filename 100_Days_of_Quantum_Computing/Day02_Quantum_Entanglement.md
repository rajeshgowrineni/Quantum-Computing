# Day 2: Quantum Entanglement

Welcome back! Today we're diving into one of the most fascinating and mysterious phenomena in quantum physics: **entanglement**. This concept is so counterintuitive that even Einstein called it "spooky action at a distance."

---

## 📚 Theoretical Foundation: What is Quantum Entanglement?

**The Basics**

When two qubits are entangled, their states become **fundamentally linked** — measuring one instantly reveals information about the other, no matter the distance between them.

Think of it like two magical coins:
- **Each coin individually:** When you flip one coin alone, it's 50% heads, 50% tails (random)
- **But when entangled together:** If you flip both, they always land on the **same side**
- **The "magic" moment:** If you see one coin is heads, you **instantly know** the other is also heads, even if it's on the other side of the universe
- **Critically:** Neither coin "decided" its outcome beforehand — the outcome only becomes definite when you look at it

This is fundamentally different from the coins being predetermined (that would be a classical correlation). The quantum correlation emerges only at measurement.

**Mathematical Description**

For two entangled qubits, we cannot describe them as two independent qubits. Instead, they share a **joint quantum state**:

$$|\psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$

This is called a **Bell state** (or **EPR pair**). Let's break down what this means:

- **|00⟩ and |11⟩** represent the two possible measurement outcomes:
  - |00⟩ means "both qubits measure to 0"
  - |11⟩ means "both qubits measure to 1"

- **The 1/√2 factors:** Each outcome has probability = (1/√2)² = 1/2 = 50%

- **The key insight:** The qubits are linked. If the first qubit is 0, the second **must** be 0. If the first is 1, the second **must** be 1. They're not independent!

**The "Spooky" Part: Why Einstein Was Puzzled**

What makes entanglement so strange:

1. **Non-locality (no faster-than-light signaling):** When you measure qubit A and get 0, qubit B's measurement outcome is instantly determined — even if they're light-years apart. However, you can't use this to send information faster than light, because the outcome you measure is random.

2. **Not predetermined:** The outcomes aren't secretly decided in advance (this violates the Bell inequality, which has been experimentally tested)

3. **It's real:** Entanglement has been confirmed by thousands of experiments. The correlation is genuine, not just apparent.

---

## 🔍 Walkthrough: Bell States Explained

There are four standard entangled states, called the **Bell basis**. Let's explore them:

**1. Φ⁺ State (Phi-plus):**
$$|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$
- **Measurement outcomes:** Both qubits are the **same** (either 00 or 11)
- **Probability:** Each outcome has 50%
- **Correlation:** "Same-same" state
- **Real-world analogy:** The "always matching" magical coins

**2. Φ⁻ State (Phi-minus):**
$$|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$$
- **Measurement outcomes:** Both qubits are the **same** (either 00 or 11)
- **What's different?** The minus sign introduces a "phase difference" (a quantum property that affects interference, but not individual measurements)
- **For measurement purposes:** Also produces 00 or 11 with 50% each
- **Why mention it?** In quantum circuits, phase matters for interference patterns

**3. Ψ⁺ State (Psi-plus):**
$$|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$$
- **Measurement outcomes:** The qubits are **opposite** (either 01 or 10)
- **Probability:** Each outcome has 50%
- **Correlation:** "Always different" state
- **Real-world analogy:** The magical coins that always show opposite sides

**4. Ψ⁻ State (Psi-minus):**
$$|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$$
- **Measurement outcomes:** The qubits are **opposite** (either 01 or 10)
- **What's different?** Again, a phase difference (minus sign)
- **For measurement:** Also produces 01 or 10 with 50% each

**Quick Reference:**

| Bell State | Outcomes | Pattern |
|-----------|----------|---------|
| Φ⁺ | 00 or 11 | Same |
| Φ⁻ | 00 or 11 | Same (phase) |
| Ψ⁺ | 01 or 10 | Opposite |
| Ψ⁻ | 01 or 10 | Opposite (phase) |

**Visualizing Entanglement**

```
Entangled Pair in Φ⁺ State:
     Qubit A        Qubit B
       │              │
       │              │
     ──●══════════════●──
       │              │
     Shared quantum state
       │              │
       │              │

After Measurement:
     Qubit A        Qubit B
       │              │
       0 ─────────────0   (correlated outcome)
       │              │
       OR
       │              │
       1 ─────────────1   (correlated outcome)
       │              │
     (Never see 01 or 10!)
```

---

## 💻 Practical Exercise: Simulating Entangled Qubits

Let's create a simulation to see entanglement in action:

```python
import numpy as np
import random

def bell_state(bell_type='Phi+'):
    """
    Generate a Bell state vector.
    
    A Bell state is represented as a 4-element array corresponding to:
    [|00⟩, |01⟩, |10⟩, |11⟩]
    
    The amplitude at each position tells us the probability of that outcome.
    """
    if bell_type == 'Phi+':
        # |Φ⁺⟩ = (1/√2)(|00⟩ + |11⟩)
        return np.array([1/np.sqrt(2), 0, 0, 1/np.sqrt(2)])
    
    elif bell_type == 'Phi-':
        # |Φ⁻⟩ = (1/√2)(|00⟩ - |11⟩)
        return np.array([1/np.sqrt(2), 0, 0, -1/np.sqrt(2)])
    
    elif bell_type == 'Psi+':
        # |Ψ⁺⟩ = (1/√2)(|01⟩ + |10⟩)
        return np.array([0, 1/np.sqrt(2), 1/np.sqrt(2), 0])
    
    elif bell_type == 'Psi-':
        # |Ψ⁻⟩ = (1/√2)(|01⟩ - |10⟩)
        return np.array([0, 1/np.sqrt(2), -1/np.sqrt(2), 0])

def measure_entangled_pair(state):
    """
    Measure both qubits in an entangled state.
    
    This simulates measuring both qubits at once.
    The result shows that the measurements are correlated!
    """
    # The four possible outcomes when measuring two qubits
    outcomes = ['00', '01', '10', '11']
    
    # Calculate probabilities for each outcome
    # (we square the amplitudes to get probabilities)
    probs = np.abs(state)**2
    
    # Randomly choose an outcome based on the probabilities
    outcome_idx = np.random.choice(4, p=probs)
    return outcomes[outcome_idx]

# Example 1: Test the Φ⁺ Bell State (same-same correlation)
print("=== Testing Φ⁺ Bell State (Same-Same) ===")
state_phi_plus = bell_state('Phi+')
print(f"State vector: {state_phi_plus}")
print(f"This represents: (1/√2)|00⟩ + (1/√2)|11⟩")
print(f"\nExpected: You should only see '00' or '11' outcomes.\n")

for i in range(10):
    result = measure_entangled_pair(state_phi_plus)
    print(f"Measurement {i+1}: {result}")

# Example 2: Test the Ψ⁺ Bell State (opposite-opposite correlation)
print("\n=== Testing Ψ⁺ Bell State (Opposite-Opposite) ===")
state_psi_plus = bell_state('Psi+')
print(f"State vector: {state_psi_plus}")
print(f"This represents: (1/√2)|01⟩ + (1/√2)|10⟩")
print(f"\nExpected: You should only see '01' or '10' outcomes.\n")

for i in range(10):
    result = measure_entangled_pair(state_psi_plus)
    print(f"Measurement {i+1}: {result}")

# Example 3: Compare with a non-entangled pair
print("\n=== For Comparison: Non-Entangled Pair (Independent) ===")
print("If both qubits were independent 50-50 superpositions,")
print("you'd see all four outcomes: 00, 01, 10, 11 equally often.")
independent = np.array([0.5, 0.5, 0.5, 0.5])
print(f"\nExpected: All outcomes appear ~25% of the time.\n")

for i in range(10):
    result = measure_entangled_pair(independent)
    print(f"Measurement {i+1}: {result}")
```

**Your Challenge:**

1. **Run the code and observe:** Notice how Φ⁺ only produces 00 or 11, while Ψ⁺ only produces 01 or 10. This is entanglement!

2. **Try different Bell states:** Run the same measurement code for 'Phi-' and 'Psi-'. Do you see the same correlation patterns? (Yes! The minus sign doesn't change measurement outcomes.)

3. **Count the outcomes:** Modify the code to count how many times each outcome appears. Φ⁺ should split ~50% between 00 and ~50% between 11 (never 01 or 10).

---

## 🧮 Hand Calculation Exercise

**Problem:** Two qubits are in the Bell state $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$

**Questions:**
1. What is the probability of measuring 00?
2. What is the probability of measuring 01?
3. If you measure the first qubit and get 0, what will the second qubit definitely be?

<details>
<summary>Click for solution</summary>

**Solution:**

1. **Probability of measuring 00:**
   - The amplitude for |00⟩ is 1/√2
   - Probability = |1/√2|² = 1/2 = **50%**

2. **Probability of measuring 01:**
   - The amplitude for |01⟩ is 0 (this term doesn't appear in the state)
   - Probability = |0|² = **0%** (this outcome never occurs!)
   - This is the key to entanglement: certain outcomes are impossible

3. **If you measure the first qubit as 0:**
   - The state must collapse to |00⟩ (since |01⟩ is not in the superposition)
   - The second qubit **must be 0**
   - This is instantaneous correlation at a distance

**Why this matters:** The instant knowledge that the second qubit is 0 (without measuring it) is what bothered Einstein. Yet no information travels between them — the outcome was only determined when you measured the first qubit.

</details>

---

## 📝 Key Takeaways

- **Entangled qubits are fundamentally linked:** Their measurement outcomes are correlated in ways that cannot be explained classically

- **Bell states are the standard entangled states:** Four specific states (Φ⁺, Φ⁻, Ψ⁺, Ψ⁻) that form the basis for quantum communication and computing

- **Measurement reveals correlation:** Before measurement, both outcomes are possible. After measuring one qubit, the state of the other is instantly determined (even at a distance)

- **Phase differences matter (sometimes):** The minus sign in Φ⁻ and Ψ⁻ doesn't change individual measurement outcomes, but creates quantum interference patterns that can be detected with the right measurements

- **No information transfer:** While the correlation is instantaneous, you cannot use entanglement to send information faster than light (the outcomes are random from your perspective)

- **Applications everywhere:** Entanglement enables quantum teleportation, superdense coding, quantum cryptography, and distributed quantum computing

---

## 🔮 Preview for Tomorrow

Tomorrow we'll learn about **quantum gates** — the tools we use to manipulate qubits and create entanglement. You'll discover how to construct Bell states from scratch and build the quantum circuits that power real quantum algorithms.

---

**Ready for tomorrow?** We'll start with the basic quantum gates (the quantum version of NOT, AND, OR gates) and build up to creating your first quantum circuit!
