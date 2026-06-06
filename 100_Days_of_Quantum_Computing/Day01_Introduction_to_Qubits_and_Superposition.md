# Day 1: Introduction to Qubits and Superposition

Welcome to your quantum computing journey! Today we're starting from the very beginning — no prior knowledge assumed. By the end of this lesson, you'll understand the fundamental building blocks of quantum computing and why they're so powerful.

---

## 📚 Theoretical Foundation: What is a Qubit?

**Classical Bits vs. Quantum Qubits**

In classical computing, information is stored in bits that can be either **0** or **1** — like a light switch that's either off or on. But quantum computing uses something fundamentally different: **qubits**.

A qubit is a quantum bit that can exist in a **superposition** of states. Think of it like a spinning coin — while it's spinning, it's neither heads nor tails, but both simultaneously in a quantum sense. When you catch it (measure it), the superposition "collapses" and becomes either heads **or** tails. Before measurement, the qubit genuinely exists in both states at once.

**The Math (Don't Worry, We'll Break It Down!)**

A qubit state is described using probability amplitudes (α and β):

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$$

Let's decode this step-by-step:

- **|0⟩ and |1⟩** are the **basis states** — the "pure" quantum states that represent classical 0 and 1. The angle brackets are just standard quantum notation.
- **α (alpha) and β (beta)** are **probability amplitudes** — numbers that tell us "how much" of each basis state exists in the superposition
- **The key rule:** The probabilities of measuring 0 or 1 are $|\alpha|^2$ and $|\beta|^2$ (we square the amplitudes)
- **The normalization constraint:** These probabilities must always add up to 1: $|\alpha|^2 + |\beta|^2 = 1$

*Note: The amplitudes can technically be complex numbers, but for this lesson, you can think of them as regular positive numbers.*

**Why Superposition Matters**

Superposition allows a qubit to hold **both values simultaneously** until measured. This is the gateway to quantum parallelism — the ability to explore many possibilities at once. While a classical bit can only be 0 or 1, a qubit can be a blend of both, and this blend is what gives quantum computers their power.

The normalization constraint ($|\alpha|^2 + |\beta|^2 = 1$) is crucial because it ensures that when we measure, we'll always get exactly one outcome — either 0 or 1 — with probabilities that add up to 100%.

---

## 🔍 Walkthrough: Visualizing Superposition

Let me walk you through a concrete example:

**Example: The Balanced Superposition**

Imagine a qubit in an equal superposition of 0 and 1:
- α = 1/√2 ≈ 0.707
- β = 1/√2 ≈ 0.707

*Before measurement:* The qubit is "both 0 and 1" simultaneously
- It's in a 50/50 blend of the two states
- It exists in both states at once in the quantum sense

*After measurement:* 
- 50% chance of getting 0 (probability = |1/√2|² = 0.5)
- 50% chance of getting 1 (probability = |1/√2|² = 0.5)
- Once measured, you get **one definitive answer**

**Visual Representation (Text Diagram)**

```
Before Measurement:
         ╱╲
        ╱  ╲
   0 → ●    ● ← 1
        ╲  ╱
         ╲╱
  "Both states" (superposition)

After Measurement:
         │
    0 →  ●    OR    ● ← 1
         │
  "One outcome" (collapsed)
```

---

## 💻 Practical Exercise: Simulating Qubits

Let's write some code to simulate qubit behavior! You can run this with Python and NumPy (no quantum libraries needed yet).

```python
import numpy as np
import random

def simulate_qubit(alpha, beta):
    """
    Simulate measuring a qubit in superposition.
    
    Args:
        alpha: Probability amplitude for |0⟩
        beta: Probability amplitude for |1⟩
    
    Returns:
        0 or 1 based on measurement
    """
    # Normalize the amplitudes to ensure they form a valid quantum state
    norm = np.sqrt(np.abs(alpha)**2 + np.abs(beta)**2)
    alpha, beta = alpha/norm, beta/norm
    
    # Calculate probabilities (square of the amplitudes)
    prob_0 = np.abs(alpha)**2
    prob_1 = np.abs(beta)**2
    
    print(f"Qubit state: α={alpha:.3f}|0⟩ + β={beta:.3f}|1⟩")
    print(f"Probability of measuring 0: {prob_0:.1%}")
    print(f"Probability of measuring 1: {prob_1:.1%}")
    
    # Simulate measurement: randomly pick an outcome based on probabilities
    if random.random() < prob_0:
        return 0
    else:
        return 1

# Example 1: Equal superposition (50-50 split)
print("=== Equal superposition ===")
result = simulate_qubit(1/np.sqrt(2), 1/np.sqrt(2))
print(f"Measurement result: {result}\n")

# Example 2: Biased superposition (70% likely to measure 0)
print("=== Biased superposition (70% likely to measure 0) ===")
result = simulate_qubit(np.sqrt(0.7), np.sqrt(0.3))
print(f"Measurement result: {result}\n")

# Example 3: Classical state (definitely 0)
print("=== Classical state |0⟩ ===")
result = simulate_qubit(1, 0)
print(f"Measurement result: {result}")
```

**Your Turn – Try These Experiments:**

1. **Run the code:** Save it as `qubit_sim.py` and run it with `python qubit_sim.py`
2. **Experiment with amplitudes:** Try different values for α and β (remember: probabilities must sum to 1!)
3. **Explore edge cases:** What happens when you set α=1, β=0? What about α=0, β=1?
4. **Check consistency:** Run the equal superposition example multiple times. Does it really give ~50% 0s and ~50% 1s on average?

---

## 🧮 Hand Calculation Exercise

Let's practice calculating probabilities by hand — this is essential for understanding quantum mechanics:

**Problem:** A qubit is in the state $|\psi\rangle = \frac{1}{2}|0\rangle + \frac{\sqrt{3}}{2}|1\rangle$

**Questions:**
1. What is the probability of measuring 0?
2. What is the probability of measuring 1?
3. Verify that these probabilities sum to 1 (check the normalization constraint).

<details>
<summary>Click for solution</summary>

**Solution:**

1. **Probability of measuring 0:**
   - $P(0) = |\alpha|^2 = \left|\frac{1}{2}\right|^2 = \frac{1}{4} = 0.25 = 25\%$

2. **Probability of measuring 1:**
   - $P(1) = |\beta|^2 = \left|\frac{\sqrt{3}}{2}\right|^2 = \frac{3}{4} = 0.75 = 75\%$

3. **Verify normalization:**
   - $25\% + 75\% = 100\%$ ✓
   - The probabilities sum to 1, confirming this is a valid quantum state

</details>

---

## 📝 Key Takeaways

- **Qubits are fundamentally different from bits:** Unlike classical bits (0 or 1), qubits can exist in superposition—a blend of both 0 and 1 simultaneously until measured
  
- **Probability amplitudes control the measurement outcome:** α and β are probability amplitudes. To get the actual probability, we square them: $P(0) = |\alpha|^2$ and $P(1) = |\beta|^2$

- **Measurement collapses the superposition:** Before measurement, the qubit is in superposition; after measurement, it's definitively 0 or 1. The superposition is gone.

- **Normalization is a hard constraint:** The probabilities must always add up to 1. This ensures physical validity: $|\alpha|^2 + |\beta|^2 = 1$

- **This is the foundation:** Superposition, combined with other quantum properties we'll explore (entanglement, interference), is what gives quantum computers their computational advantage

---

## 🔮 Preview for Tomorrow

Tomorrow we'll explore **quantum entanglement** — a phenomenon where two or more qubits become mysteriously connected, allowing them to influence each other's states instantly, no matter how far apart they are. This is where quantum mechanics gets truly bizarre and powerful!

---

**Ready for tomorrow?** Come back and we'll dive into one of the most counterintuitive yet powerful concepts in quantum computing: entanglement!
