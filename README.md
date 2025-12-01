
  ◇    D ' H A W K - L A B S   ◇         


<h3 align="center">WPE & TME Semantic Calculus Languages</h3>

<h3 align="center">Text-Native Languages for Structural and Temporal Reasoning</h3>

<p align="center">
  <a href="https://doi.org/10.13140/RG.2.2.28299.55844"><img src="https://img.shields.io/badge/Paper-ResearchGate-00CCBB?style=for-the-badge" alt="Paper"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache 2.0-blue?style=for-the-badge" alt="License"/></a>
  <a href="https://github.com/Heimdall-Organization/wpe-tme-language/stargazers"><img src="https://img.shields.io/github/stars/Heimdall-Organization/wpe-tme-language?style=for-the-badge" alt="Stars"/></a>
</p>

<p align="center">
  <strong>Explicit structure</strong> · <strong>Geometric coupling</strong> · <strong>Temporal semantics</strong> · <strong>LLM-ready</strong>
</p>

---

## What Are WPE & TME?

**WPE (Whole-Process Encoding)** and **TME (Temporal-Mode Encoding)** are two complementary languages for encoding semantic structure and temporal relationships explicitly.

Think of them as **mathematical notation for systems** instead of for equations.

### The Problem

Large language models reason statistically. Structure exists implicitly in billions of parameters, but:
- ❌ Can't inspect internal representations
- ❌ Can't debug reasoning chains
- ❌ Can't modify relationships explicitly
- ❌ Temporal relationships are inferred, not encoded

### The Solution

WPE/TME make structure explicit through **4-parameter geometric encoding**:

```wpe
Component:Domain:Shell@Phase|Curvature
```

**Every relationship is visible. Nothing hidden.**

---

## Quick Example

### Feedback Control Loop

```wpe
# Define components with geometric parameters
Sensor:P:2@0|-3.0        # Physics domain, shell 2, 0°, curvature -3.0
Controller:C:3@90|-2.5    # Cognition domain, shell 3, 90°, curvature -2.5
Actuator:P:4@180|-2.0     # Physics domain, shell 4, 180°, curvature -2.0

# Coupling relationships (derived from geometry)
Sensor <-> Controller     # cos(90° - 0°) = 0.0 (orthogonal, no interference)
Controller <-> Actuator   # cos(180° - 90°) = 0.0 (orthogonal)
Actuator <-> Sensor       # cos(0° - 180°) = -1.0 (opposition, feedback!)
```

**The coupling strengths are automatic** from phase geometry. Opposition creates feedback.

### Multi-Agent System

```wpe
# Three agents in symmetric configuration
Agent1:C:2@0|-2.5
Agent2:C:2@120|-2.5
Agent3:C:2@240|-2.5

# Automatic coupling from 120° spacing
# Each pair: cos(120°) = -0.5 (balanced interaction)
# All at same shell = peer relationships (no hierarchy)
```

### Temporal Sequence (TME)

```tme
@temporal_scale α=1.0

# Left-to-right = forward in time
T1: Initialize:P:1@0|-3.0 [duration=5]
T2: Process:C:2@45|-2.5 [duration=10] 
T3: Output:O:3@90|-2.0 [duration=3]

# Sequential flow encoded syntactically
T1 -> T2 -> T3
```

---

## Language Specification

### The 4 Parameters

| Parameter | Symbol | Type | Range | Meaning |
|-----------|--------|------|-------|---------|
| **Domain** | Φ | Letter | P, C, B, M, S... | Substrate/field type |
| **Shell** | λ | Integer | 1-9 | Hierarchical level |
| **Phase** | θ | Float | 0-359° | Angular position |
| **Curvature** | κ | Float | ℝ⁻ | Stability (negative = well) |

### Domain Codes

```
P  = Physics       (forces, fields, energy)
C  = Cognition     (information, attention, reasoning)
B  = Biology       (populations, concentrations, metabolism)
M  = Memory        (storage, retrieval, persistence)
S  = Social        (relationships, influence, economics)
Ph = Philosophy    (abstract concepts, logic)
O  = Output        (results, outputs, conclusions)
NX = Nexus         (connections, interfaces)
MT = Meta          (reasoning about reasoning)
```

Custom domains allowed: `Ch` (chemistry), `L` (language), etc.

### Coupling Rules

**Automatic from phase geometry:**

```
Coupling_strength = cos(θᵢ - θⱼ)
```

| Phase Difference | Coupling | Interpretation |
|------------------|----------|----------------|
| 0° | +1.0 | Maximum (aligned) |
| 45° | +0.707 | Strong positive |
| 90° | 0.0 | None (orthogonal) |
| 135° | -0.707 | Strong negative |
| 180° | -1.0 | Opposition (feedback) |

**No coupling matrix needed.** Geometry defines interactions.

### Hierarchical Influence

**Automatic from shell levels:**

```
Influence = 1/λ_low - 1/λ_high
```

| Shells | Influence | Interpretation |
|--------|-----------|----------------|
| 7 → 1 | 0.857 | Strong top-down |
| 5 → 3 | 0.267 | Moderate downward |
| 3 → 2 | 0.167 | Weak peer influence |

Higher shells constrain lower shells. Distance reduces influence naturally.

### Energy Functional

System energy (guides optimization):

```
E = ∫[|∇Ψ|² + κΨ² + Σγⱼₖ ΨⱼΨₖ + Σαᵢⱼ⟨Ψᵢ|Ψⱼ⟩] dV

where:
  |∇Ψ|²        = Gradient energy (spatial variation)
  κΨ²          = Curvature energy (stability)
  γⱼₖ ΨⱼΨₖ     = Coupling energy (interactions)
  αᵢⱼ⟨Ψᵢ|Ψⱼ⟩  = Hierarchical energy (shell influences)
```

Minimum energy = optimal configuration.

---

## Syntax Reference

### WPE Syntax

**Basic component:**
```wpe
Label:Domain:Shell@Phase|Curvature
```

**With amplitude:**
```wpe
Label:Domain:Shell@Phase|Curvature amplitude=1.0
```

**Coupling (explicit):**
```wpe
ComponentA <-> ComponentB
```

**Coupling with strength:**
```wpe
ComponentA <-0.707-> ComponentB  # Override geometric coupling
```

**Grouping:**
```wpe
System = {
  Component1:P:2@0|-3.0
  Component2:C:3@90|-2.5
  Component3:P:4@180|-2.0
}
```

### TME Syntax

**Temporal frame:**
```tme
@temporal_scale α=1.0

Event1 [duration=5, phase=0]
Event2 [duration=10, phase=45]
Event3 [duration=3, phase=90]
```

**Sequential flow:**
```tme
Event1 -> Event2 -> Event3
```

**Parallel processes:**
```tme
// Same temporal layer = parallel
@layer L1:
  ProcessA [duration=10]
  ProcessB [duration=10]
```

**Conditional branching:**
```tme
Event1 -> {
  [condition=C1] Event2A
  [condition=C2] Event2B
} -> Event3
```

---

## Installation

```bash
git clone https://github.com/[user]/wpe-tme-language
cd wpe-tme-language
```

### View Language Specification

```bash
# Core WPE specification
cat specification/wpe-core.md

# TME temporal specification
cat specification/tme-core.md

# Full syntax reference
cat specification/syntax-reference.md

# Formal semantics
cat specification/formal-semantics.md
```

### Try Examples

```bash
# Feedback loop
cat examples/feedback-loop.wpe

# Multi-agent system
cat examples/multi-agent-system.wpe

# Hierarchical reasoning
cat examples/hierarchical-reasoning.wpe

# Temporal sequences
cat examples/temporal-sequences.tme
```

### Python Reference Implementation

```bash
cd reference-implementation/python
pip install -r requirements.txt

# Parse WPE file
python wpe_parser.py ../../examples/feedback-loop.wpe

# Evaluate coupling strengths
python wpe_evaluator.py ../../examples/multi-agent-system.wpe

# Visualize geometry
python wpe_visualizer.py ../../examples/feedback-loop.wpe --output diagram.png
```

---

## Use Cases

### 1. LLM Scaffolding

**Problem:** LLMs lose track of structure in long reasoning chains.

**Solution:** Encode reasoning steps explicitly in WPE.

```wpe
# Research paper analysis
Introduction:C:1@0|-4.0
Methods:C:2@45|-3.5
Results:C:3@90|-3.0
Discussion:C:4@135|-3.5
Conclusion:C:5@180|-4.0

# Hierarchical influences
Conclusion:C:5 <- Discussion:C:4 <- Results:C:3 <- Methods:C:2
```

LLM can now reference the explicit structure.

### 2. Multi-Agent Systems

**Problem:** Complex agent interactions hard to specify.

**Solution:** Use phase geometry for automatic coupling.

```wpe
# Three competing agents
Competitor1:S:2@0|-2.5
Competitor2:S:2@120|-2.5
Competitor3:S:2@240|-2.5

# Regulator above them
Regulator:S:5@180|-3.0

# Automatic:
# - Peers compete (cos(120°) = -0.5)
# - Regulator influences all (1/2 - 1/5 = 0.3)
```

### 3. Temporal Logic

**Problem:** Event sequences and durations need explicit representation.

**Solution:** TME makes time syntactic.

```tme
@temporal_scale α=1.0

Startup [duration=5, phase=0]
  -> Initialize [duration=2, phase=10]
  -> Load [duration=8, phase=15]
  -> Ready [duration=1, phase=25]
  
// Left-to-right = forward in time
// Phase = temporal ordering
// Duration = explicit length
```

### 4. System Modeling

**Problem:** Complex systems with feedback, hierarchy, and temporal dynamics.

**Solution:** WPE + TME capture all relationships.

```wpe
# Economic system
Consumers:S:2@0|-3.0
Producers:S:2@90|-2.5
Regulators:S:5@180|-4.0
Market:S:3@45|-2.0

# Feedback loops
Consumers <-> Market <-> Producers
Regulators -> Consumers
Regulators -> Producers
```

---

## Examples Gallery

### Example 1: Neural Attention Mechanism

```wpe
# Transformer attention
Query:C:2@0|-3.0
Key:C:2@90|-2.5
Value:C:3@45|-2.0
Output:C:4@135|-1.5

# Interactions
Query <-> Key      # cos(90°) = 0 (orthogonal match)
Query * Value      # Creates attention score
Output <- Value    # Hierarchical: 1/3 - 1/4 = 0.083
```

### Example 2: Predator-Prey System

```wpe
# Lotka-Volterra dynamics
Prey:B:2@0|-2.5
Predator:B:2@180|-2.0

# Opposition creates oscillation
# cos(180°) = -1.0 (maximum opposition)
Prey <-> Predator

# Environment constraints
Environment:B:5@90|-3.5
Environment -> Prey
Environment -> Predator
```

### Example 3: Information Processing Pipeline

```tme
@temporal_scale α=1.0

// Data flow pipeline
Ingest:P:1@0|-3.0 [duration=2]
  -> Parse:C:2@30|-2.5 [duration=5]
  -> Transform:C:3@60|-2.0 [duration=8]
  -> Store:M:4@90|-1.5 [duration=3]
  -> Output:O:5@120|-1.0 [duration=1]

// Phases encode temporal ordering
// Shells encode processing hierarchy
```

### Example 4: Decision-Making Hierarchy

```wpe
# Corporate decision structure
Executives:C:7@0|-4.0
Managers:C:5@45|-3.5
Workers:C:2@90|-3.0
Customers:S:1@135|-2.5

# Hierarchical influences
Executives -> Managers   # 1/5 - 1/7 = 0.057 (strategic guidance)
Managers -> Workers      # 1/2 - 1/5 = 0.300 (tactical direction)
Customers <-> Workers    # cos(45°) = 0.707 (direct interaction)
```

---

## Documentation

### Language Specification

- 📘 [WPE Core Specification](https://github.com/Heimdall-Organization/wpe-tme-language/blob/main/WPE_FORMAL_v5.0.md) - Formal syntax, semantics, type system
- 📙 [TME Core Specification](https://github.com/Heimdall-Organization/wpe-tme-language/blob/main/TME_FORMAL_v1.0.md) - Temporal encoding rules
- 📔 [Quick Reference Guide](https://github.com/Heimdall-Organization/wpe-tme-language/blob/main/WPE_TME_QUICK_REFERENCE.md) - Reference Doc

### Book

- 📘 [WPE/TME Book](https://github.com/Heimdall-Organization/wpe-tme-language/blob/main/wpe_tme_book.md) - The Complete Book on Semantic Calculus 
  
---

## Python Reference Implementation

The `reference-implementation/python/` directory contains:

### Core Modules

**`wpe_parser.py`** - Parses WPE syntax into AST
```python
from wpe_parser import parse_wpe

ast = parse_wpe("Sensor:P:2@0|-3.0")
print(ast.domain)    # "P"
print(ast.shell)     # 2
print(ast.phase)     # 0.0
print(ast.curvature) # -3.0
```

**`wpe_evaluator.py`** - Computes coupling strengths
```python
from wpe_evaluator import evaluate_coupling

c1 = parse_wpe("A:P:2@0|-3.0")
c2 = parse_wpe("B:C:3@90|-2.5")

coupling = evaluate_coupling(c1, c2)
print(coupling)  # 0.0 (cos(90°))
```

**`tme_interpreter.py`** - Evaluates temporal sequences
```python
from tme_interpreter import TMEInterpreter

code = """
@temporal_scale α=1.0
Event1 [duration=5] -> Event2 [duration=10]
"""

interp = TMEInterpreter()
timeline = interp.execute(code)
print(timeline.total_duration)  # 15
```

**`wpe_visualizer.py`** - Generates diagrams
```python
from wpe_visualizer import visualize_system

system = parse_wpe_file("examples/feedback-loop.wpe")
visualize_system(system, output="diagram.png")
# Creates phase-space diagram with coupling lines
```

### Command-Line Tools

```bash
# Parse and validate
wpe-parse examples/feedback-loop.wpe

# Compute all couplings
wpe-eval examples/multi-agent-system.wpe

# Generate visualization
wpe-viz examples/feedback-loop.wpe --output diagram.png --format png

# Check energy
wpe-energy examples/feedback-loop.wpe
```

---

## FAQ

**Q: Is this a library or a language?**  
A: A language with formal syntax and semantics. The Python code is a reference implementation, not "the product."

**Q: Can I use this without Python?**  
A: Yes! WPE/TME are text-based notations. Write implementations in any language. We provide Python as reference.

**Q: Do I need to learn physics?**  
A: No. The physics is in the design. Users just specify 4 parameters per component.

**Q: How does this compare to graph representations?**  
A: Graphs show topology. WPE adds coupling strength, hierarchy, stability, and temporal order—all from geometry.

**Q: Can I modify the coupling rules?**  
A: Yes, but default geometric rules (cosine coupling, hierarchical influence) work well for most systems.

**Q: What about performance?**  
A: WPE/TME are specifications, not runtime systems. Performance depends on your implementation.

**Q: Is this for LLM fine-tuning?**  
A: No fine-tuning needed. Just include WPE/TME notation in prompts. Models parse it as structured text.

**Q: Can I create custom domains?**  
A: Yes! Define any single-letter or two-letter domain code. E.g., `Ch` for chemistry, `L` for linguistics.

---

## Research Paper

📄 **WPE & TME: A Geometric Calculus for Structural and Temporal Reasoning**

**Abstract:** We present WPE (Whole-Process Encoding) and TME (Temporal-Mode Encoding), two complementary languages for encoding structural and temporal semantics explicitly. Both use 4-parameter geometric representations where coupling strengths emerge from phase relationships via cosine rules, hierarchical influences follow from shell levels, and temporal ordering is syntactic. The languages are text-native (pure ASCII), model-agnostic (no fine-tuning required), and compositional (complex systems from simple primitives). We demonstrate applications to LLM scaffolding, multi-agent systems, temporal logic, and system modeling.

**[Read on ResearchGate →](https://doi.org/10.13140/RG.2.2.28299.55844)**  
**[Download PDF →](https://github.com/Heimdall-Organization/wpe-tme-language/blob/main/WPE_TME_Semantic_Calculus__A_Geometric_Framework_for_Structural_and_Temporal_Reasoning_in_AI_Systems%20(1).pdf)**

---

## Citation

```bibtex
@article{yourname2025wpe,
  title={WPE \& TME: A Geometric Calculus for Structural and Temporal Reasoning},
  author={Your Name},
  year={2025},
  journal={arXiv preprint arXiv:[id]},
  url={https://github.com/[user]/wpe-tme-language},
  note={Programming language specification with Python reference implementation}
}
```

---

## Contributing

We welcome contributions! 
- Adding examples to the library
- Implementing in other languages
- Improving documentation
- Extending the specification
- Creating tools and IDE support

**Good first issues:**
- Add syntax highlighting for editors
- Implement WPE parser in Rust/Julia
- Create more worked examples
- Write tutorial content
- Add visualization options

---

## Community

- 💬 [GitHub Discussions](https://github.com/Heimdall-Organization/wpe-tme-language/discussions/1) - Questions and ideas
- 🐛 [Issues](https://github.com/Heimdall-Organization/wpe-tme-language/issues) - Bug reports and feature requests
- 📧 [Email](mailto:[theheimdallorganization@gmail.com) - Direct Contact

---

## Related Projects

- [Crystalline Language](https://github.com/Heimdall-Organization/crystalline-language) - Code synthesis using same geometric foundation
- [BioGenerative Crystal](https://github.com/Heimdall-Organization/biogenerative-architecture) - Biological modeling with WPE/TME

---

## License

Apache 2.0 License - see [LICENSE](LICENSE)

---

<p align="center">
  <strong>Make structure explicit. Make reasoning visible.</strong>
</p>


<p align="center">
  ⭐ Star this repo if you're interested in explicit reasoning structures!
</p>
