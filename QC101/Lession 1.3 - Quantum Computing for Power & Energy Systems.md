# Quantum Computing for Power & Energy Systems - Comprehensive Notes

**Presenter:** Dr. Sayonsom Chanda, Senior Chief Engineer, Samsung R&D Institute India
**Presentation Type:** Womanium Keynote Presentation
**Duration:** 28 minutes

***

## 1. INTRODUCTION: SOCIETAL PERSPECTIVES ON ENERGY

### Energy Expectations Pyramid
The video begins by discussing societal expectations for energy across three levels:

**Our Own Lives:**
- Reliable
- Cheap
- 24/7 availability
- Smart appliances
- Energy efficiency
- Convenience

**Our Communities:**
- Resilient
- Energy Secure
- Energy Justice

**Our Environment:**
- Net zero emissions
- Do not add to climate burden

### The Challenge
While these expectations are met "99% of the time," the critical 1% gap appears during extreme events like wildfires, leading to power outages and grid failures.

***

## 2. COMPUTATIONAL CHALLENGES IN POWER SYSTEMS

### Historical Context
- Utility industry: ~150 years old
- Computational use in utilities: ~80 years
- First computational work: 1940s-1950s with early Univac computers for load flow problems

### Typical Heavy Computation Use-Cases in Utilities

| Use Case | Size of Study | Data Generated | Resources Used | Time Taken per Solution | Quantum Worthy |
|----------|---------------|-----------------|-----------------|------------------------|----------------|
| **Monte-Carlo for scenario planning** | 3,000 transmission nodes | 25 TB daily | Fourteen 192GB EC2s on AWS | 48 hours | ✓ YES |
| **Hourly Load Forecast with multiple DERs** | 2,000 Feeders (NY) | 1 TB | Ten 96GB EC2 on AWS | 10-12 days per scenario | ✓ YES |
| **Unit Commitment** | 28,000 nodes | Depends | Cloud-deployed PowerWorld | Minutes-hours | ✗ NO |
| **Power Flow** | 50,000 buses | 500 GB | PSSE software | Few minutes | ✗ NO |

### Key Observations
- Monte Carlo simulations can take nearly a week just to run
- Cloud computing costs (e.g., Amazon bills) can reach tens of thousands of dollars
- Some utility companies run simulations on 50,000 buses with massive computational requirements

***

## 3. FIVE UNSOLVED/OVERLOOKED CHALLENGES IN POWER SYSTEMS

### Problems with No Current Solutions

1. **Too Many Devices to Manage and Optimize**
   - IoT proliferation creating exponentially complex management problems
   - Problem space becomes intractable with classical approaches

2. **Grid Partitioning and Microgrid Islanding**
   - Not solved by traditional computing software despite billions invested
   - Optimization problem (QAOA) not well-handled by classical computers
   - Government encouraging microgrids to increase resilience

3. **Customer Level Energy Behavior/Usage Modeling**
   - Smart meter data from all customers not even considered in current models
   - Time-of-use pricing requires understanding customer behavior patterns

4. **Cybersecurity and Vulnerability Research**
   - Power grid attacked by malicious actors ~1,000 times daily
   - No way to perform "Gain of Function" research on cyber-vulnerabilities
   - Current speculation not scientifically validated

5. **Scenario Planning with Diverse Climate Scenarios**
   - Limited number of scenarios currently planned (assumes stability)
   - Need to consider multiple futures (wildfires, carbon neutrality, new climate scenarios)
   - Different initial conditions require comprehensive modeling

***

## 4. WHERE QUANTUM COMPUTING ADDS VALUE

### The NISO-ERA Framework
A Venn diagram showing:
- **Left Circle (Not Done):** Grid Partitioning, Cybersecurity, Customer Modeling
- **Intersection (NISO-ERA):** Problems requiring quantum approaches
- **Right Circle (Done Today):** Large-scale optimization problems already solved

### Two Key Opportunities for Quantum Computing

**1. Transmission Planning with Microgrids**
- Optimization problem solvable via QAOA (Quantum Approximate Optimization Algorithm)
- Determine how to island microgrids to maintain overall grid resilience
- Classical computers either solve inefficiently or don't attempt it

**2. Large-Scale Optimization Problems with Slow Convergence**
- Classical solutions exist but convergence takes excessive time
- Quantum speedup could be transformative
- Important to validate these problems NOW before quantum hardware matures

### Critical Insight
- Start validating problems today so they can scale with emerging quantum hardware
- Focus on problems where quantum fundamentally differs from classical approaches

***

## 5. THE FUNDAMENTAL PARADIGM SHIFT

### Historical Evolution of Computing
From Charles Babbage (1885) to present:
- Invented electric switches that replaced mechanical relays
- For 150+ years: focus has been on making better switches or packing more switches
- Classical computing = improvements to switches or miniaturization (Moore's Law)

### Nature's Computing Model
**Richard Feynman's Insight:** "Nature isn't classical, goddamit!"

Key distinctions:
- Nature operates continuously without binary switches (0 or 1)
- Universe operates on quantum mechanics, not boolean logic
- Nature performs amazing computations and chemical processes constantly
- Binary operations cannot model the universe's operation

### Why This Matters for Energy Systems
Algorithm designers must understand that:
- Quantum algorithms will fundamentally differ from classical ones
- Binary 0/1 logic cannot capture problems like customer behavior modeling or grid dynamics
- Quantum's natural operations align with energy system complexity

***

## 6. ENERGY SECTOR APPLICATIONS BEYOND POWER SYSTEMS

### Quantum Chemistry Applications
More realistic near-term benefits through quantum computing:

1. **Convert Methane to Methanol**
   - Chemical reaction modeling
   - Material efficiency in conversion

2. **Development of New Materials**
   - Battery materials and components
   - Energy storage systems
   - Electric vehicle components

### Technical Challenges for Materials
- **Solving Navier-Stokes Equation:** Velocity, pressure, viscosity, density relationships in fluid dynamics
- **Molecular Vibrational Calculations:** Quantum simulations of molecular behavior
- Applications to energy sources and storage

### Strategic Impact
- Solutions impossible with generative AI
- Tremendous use cases in addressing climate change
- Reducing global emissions through material innovation
- Design chain: QuantumATK → Battery materials → Battery cells → Battery packs → OEM manufacturing

***

## 7. KEY TAKEAWAYS AND RECOMMENDATIONS

### For Power & Energy Companies
1. **Understand the limitations of classical computing** in solving emerging grid challenges
2. **Invest in understanding quantum applications** to power systems
3. **Focus on the intersection of unsolved problems and quantum advantage**
4. **Start validating quantum approaches now** rather than waiting for perfect hardware

### For Algorithm Designers
1. **Embrace the paradigm shift** - quantum computing is fundamentally different
2. **Stop trying to optimize binary processes** - leverage quantum parallelism
3. **Address problems with natural quantum structure** (optimization, simulation, chemistry)

### The Bridge Between Today and Tomorrow
- Grid partitioning for resilience
- Large-scale optimization with quantum speedup
- Quantum chemistry for advanced materials
- Cybersecurity research with quantum simulations

***

## CONCLUSION

The convergence of quantum computing and power/energy systems represents an opportunity to solve previously intractable problems in:
- Grid resilience and optimization
- Advanced material development  
- Climate change mitigation
- Renewable energy integration
- Cybersecurity

However, this requires:
- Honest problem framing
- Understanding where quantum truly adds value (not everywhere)
- Early validation of quantum approaches
- A fundamental mindset shift from classical to quantum thinking

The presenter emphasizes that quantum computing will show greater value in energy systems if we work on problems at the intersection of quantum capabilities and real-world energy challenges.