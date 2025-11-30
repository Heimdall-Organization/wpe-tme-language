# The WPE & TME Semantic Calculus
## A Framework for AI-Native Structural and Temporal Reasoning

---

# PART I — FOUNDATIONS

## Chapter 1 — The Problem Space

Large language models excel at pattern recognition. They learn associations across vast corpora, recognize linguistic structures, generate contextually appropriate text. Ask them about facts, and they often get it right. Ask them to write code, and they produce working solutions. Ask them to explain concepts, and they deliver coherent explanations.

But ask them to maintain consistent internal structure across a long reasoning chain, or to represent temporal relationships explicitly, and they struggle.

The issue isn't capability—it's representation. Models predict the next token based on statistical patterns learned from training data. There's no stable internal scaffold that persists across the conversation. No explicit encoding of "this happens before that" or "these components influence each other with this specific strength."

Consider a simple question: "If A influences B, and B influences C, what happens when you change A?" The model might answer correctly through learned patterns. But it's not maintaining an explicit directed graph. It's not tracking coupling strengths. The structure exists implicitly in billions of parameters, diffused across the network, but it's not written down anywhere.

This creates predictable failure modes. Reasoning chains drift as context windows fill. Temporal relationships become muddled when multiple events interleave. Multi-step problems that require maintaining precise structural relationships produce inconsistent results. Ask a model to track five interconnected variables through ten steps of reasoning, and watch it lose track of which influences which.

The problem becomes clearer when you try to debug these failures. There's no explicit structure to inspect. You can't look at the model's representation and see "ah, it's treating A→B coupling as 0.5 when it should be 0.8." The computation happens in a black box.

What if we could give models an explicit geometric language? Not to replace their statistical learning, but to scaffold it. A way to write down "A couples to B with strength 0.707 because they're separated by 45 degrees in phase space." A notation where temporal relationships emerge from syntactic ordering rather than learned associations.

That's what WPE and TME provide. Not a replacement for neural networks, but a complementary representation. A way to make structure and time explicit. A scaffold for reasoning.

When you encode a system in WPE, you specify exactly how components interact. The coupling strengths follow from geometric relationships. The hierarchical influences are defined by shell levels. The stability characteristics come from curvature values. Everything is explicit.

When you add TME, temporal ordering becomes syntactic. Left-to-right reading is forward in time. Phase values determine when things happen. Duration emerges from curvature. Parallel processes occupy separate layers. The temporal structure is visible in the notation itself.

This doesn't solve all problems. It doesn't make models smarter. But it gives them something to reason about explicitly. A structure they can manipulate symbolically. A temporal framework they can traverse sequentially.

The rest of this book explains how.

## Chapter 2 — Design Goals

The core principle drives everything: **encoding equals computation**. The syntax rules themselves define the behavior. There are no separate formula evaluations. When you write a WPE expression, the geometric relationships in the notation directly specify how components interact.

Consider this example:
```
P:1@0|-3 * C:2@45|-2
```

Two components. P at phase 0°, C at phase 45°. The phase difference is 45°. The coupling strength is cos(45°) = 0.707. That's it. No additional calculation needed. The syntax defines the coupling.

This creates several design requirements that shape the entire framework.

**Text-based only.** Language models work with tokens. Any framework for AI reasoning must fit within that constraint. No external data structures, no hidden vector representations. Just text that models can read and generate.

This seems obvious but has deep implications. It means we can't rely on matrix operations happening behind the scenes. We can't use floating-point arrays. Everything must be representable as characters that encode into tokens.

The advantage: models already know how to manipulate text. They're trained on it. They generate it fluently. We're working with their native medium.

**Model-agnostic.** The representation shouldn't require fine-tuning or weight modification. It should work through prompting alone. A model that understands the syntax rules can use the framework immediately.

Why this matters: we want portability. An encoding written for one model should work with another. We want rapid iteration—no training cycles. We want to scaffold existing models, not build new ones.

The test: can you explain the syntax in a system prompt and have the model use it correctly? If yes, the design succeeds.

**Compositional.** Complex systems build from simple primitives. The notation must support hierarchical composition—subsystems within systems, patterns that recur, operators that combine structures predictably.

This enables scaling. You start with simple components. You combine them into subsystems. You nest subsystems into larger systems. Each level maintains clear interfaces to the levels above and below.

It also enables reuse. Once you've encoded a feedback control loop, you can use that pattern everywhere—neural regulation, thermostats, economic markets. The structure remains the same even as the substrate changes.

**Explicit temporal relationships.** Time shouldn't be implicit. When one thing happens before another, the notation should make that visible. When processes run in parallel, that should be structurally apparent.

Current approaches often treat time as a parameter that varies continuously. Events occur at t=0.5 or t=1.7. But in many systems, what matters is ordering and duration, not absolute timestamps.

WPE/TME makes this explicit. Sequential phase ordering creates temporal progression. The syntax itself encodes "first this, then that." You can read the temporal structure directly from the expression.

**Visible computation.** All information processing should occur at the representation level. No hidden state. If a model can read the encoding, it should be able to trace how information flows through the system.

This is crucial for debugging and verification. When reasoning fails, you need to see where it went wrong. With visible computation, you can inspect the structural representation and identify the error.

It also helps models reason about their own reasoning. If the structure is explicit, models can examine it, modify it, optimize it. Meta-reasoning becomes possible.

These constraints aren't arbitrary. They emerge from the reality of working with language models as they exist today. We need representations that models can manipulate as text, that don't require architectural changes, and that make all reasoning steps explicit.

The result is a geometric calculus. Components have positions in phase space. Their angular separation determines coupling strength via a simple cosine rule. Higher hierarchical levels influence lower ones with strength proportional to the shell gap. Curvature determines whether components form stable wells or unstable peaks. Time emerges from sequential phase ordering.

No hidden complexity. The notation is the computation. That's the design.

---

# PART II — WPE: STRUCTURAL SEMANTICS

## Chapter 3 — The Four Parameters

Every component in WPE has exactly four parameters. Together they specify everything about how that component behaves and interacts with others.

**Φ (Domain)** specifies the field type. This is a single letter or short code that tells you what kind of substrate the component represents:

- `P` = Physics (electromagnetic fields, forces, particles, energy)
- `C` = Cognition (neural activity, information processing, attention)
- `B` = Biology (populations, chemical concentrations, metabolic rates)
- `S` = Social (influence, relationships, economic quantities)
- `Ph` = Philosophy (abstract concepts, logical structures)
- `O` = Output (final results, system outputs)
- `NX` = Unknown (unspecified or exploratory)
- `MT` = Meta (reasoning about the system itself)
- `M` = Memory (stored information, persistent state)

You can also define custom domain codes. If you're modeling a chemical reaction network, you might use `Ch` for chemistry. If you're working with linguistic structures, maybe `L` for language.

The domain determines physical interpretation. In physics, a component might represent electromagnetic field energy at a point in space. In biology, it might represent a population density. In cognition, information content in a neural assembly.

But here's the key insight: the geometric relationships stay the same across domains. A triangular structure with 120° spacing behaves the same way whether it's representing three-phase electrical power, a predator-prey-resource ecosystem, or attention-memory-processing trade-offs.

What changes is the physical substrate—the actual quantity being measured. The coupling mechanisms—how interactions physically occur. The constraints—what physical laws must be satisfied. But the geometric structure remains invariant.

**λ (Shell)** specifies hierarchical level from 1 to 9:

Level 1 (foundation): The concrete, physical layer. Direct measurements. Raw sensory input. Base-level phenomena.

Levels 2-3 (processing): Operational transformations. Data processing. Feature extraction. Computation happens here.

Levels 4-6 (integration): Synthesis and combination. Bringing together multiple streams. Building coherent representations from pieces.

Levels 7-9 (abstraction): Meta-level reasoning. Abstract concepts. Reasoning about reasoning. The highest conceptual layers.

Higher shells influence lower shells. The influence strength is proportional to:
```
1/λ_low - 1/λ_high
```

For example, shell 7 influencing shell 1:
```
1/1 - 1/7 = 1.0 - 0.143 = 0.857
```

Strong influence. High-level abstract goals heavily constrain low-level concrete actions.

Shell 3 influencing shell 2:
```
1/2 - 1/3 = 0.500 - 0.333 = 0.167
```

Weaker influence. Adjacent processing levels have moderate coupling.

This creates natural hierarchies. Top-level intentions shape everything below them. But the influence decreases with distance—level 9 doesn't micromanage level 1. Each level has autonomy proportional to its distance from controlling levels.

**θ (Phase)** specifies angular position from 0 to 359 degrees. This is the geometric heart of WPE.

Components interact based on their phase difference. The coupling strength equals:
```
cos(θ_i - θ_j)
```

Let's see what this means:

Two components at the same phase (Δθ = 0°):
```
cos(0°) = 1.0
```
Maximum positive coupling. They reinforce each other completely.

30° apart:
```
cos(30°) = 0.866
```
Strong positive coupling. Typical for sequential processing stages.

45° apart:
```
cos(45°) = 0.707
```
Moderate positive coupling. Information flows forward with some transformation.

60° apart:
```
cos(60°) = 0.5
```
Partial coupling. Components cooperate but maintain significant independence.

90° apart (orthogonal):
```
cos(90°) = 0.0
```
Zero coupling. Completely independent. No interaction.

120° apart:
```
cos(120°) = -0.5
```
Moderate negative coupling. Opposition. Competition. Inhibition.

180° apart (opposite):
```
cos(180°) = -1.0
```
Maximum negative coupling. Complete opposition. Perfect cancellation.

Why cosine? Because it captures geometric alignment. When two vectors point in the same direction, they're maximally aligned. When perpendicular, they're independent. When opposite, they cancel.

This isn't arbitrary mathematics. It matches how physical fields interact (electromagnetic superposition), how neural populations couple (excitation-inhibition), how social influences combine (support-opposition).

The beauty: once you choose phases, coupling strengths follow automatically. No manual specification needed. The geometry computes.

**κ (Curvature)** specifies field strength from -10 to +10.

Negative curvature creates stable configurations—wells that attract and hold information:

κ < -3.0: Deep well. Strong attraction. Information persists. Use for long-term memory, core beliefs, stable attractors.

-3.0 ≤ κ ≤ -1.0: Standard well. Stable processing. Information is retained but can flow onward. Most processing components fall here.

-1.0 < κ < 0: Shallow well. Weak attraction. Transient states. Temporary buffers that information passes through briefly.

κ = 0: Critical point. Unstable equilibrium. Balanced on a knife edge. Decision points, bifurcations, phase transitions.

κ > 0: Peak. Repulsion. Information is actively pushed away. The system avoids this state. Use for inhibited or suppressed states.

In temporal systems (TME), curvature takes on a dual meaning. It still represents stability, but it also determines duration. The time constant is:
```
τ = 1/|κ|
```

High magnitude curvature (large |κ|): Short duration. Fast, brief events.
Low magnitude curvature (small |κ|): Long duration. Slow, prolonged processes.

For example:
- κ = -5.0 → τ = 0.2 (fast event, like a neural spike)
- κ = -2.0 → τ = 0.5 (moderate processing)
- κ = -1.0 → τ = 1.0 (slow integration)

The sign determines stability. The magnitude determines strength (in spatial systems) or speed (in temporal systems).

**Putting it together: Component syntax**

```
Φ:λ@θ|κ:'role'
```

The role field is optional documentation. It doesn't affect computation—it just helps humans understand what the component represents.

Example:
```
P:1@0|-2.5:'input'
```

Reading this: Physics domain, shell level 1 (foundation), phase 0° (reference point), curvature -2.5 (stable well), serving as system input.

Another example:
```
C:4@90|-1.8:'integration'
```

Cognition domain, shell 4 (integration level), phase 90° (orthogonal to 0° reference), curvature -1.8 (moderately stable), integration function.

Default values if parameters are omitted:
- θ defaults to 0°
- κ defaults to -2.0 (standard stable well)
- λ defaults to 2 (processing level)

So `C` alone would mean `C:2@0|-2.0`.

These four parameters—domain, shell, phase, curvature—define everything about a component's behavior. How it couples to others (phase). Where it sits in the hierarchy (shell). How stable it is (curvature). What substrate it represents (domain).

The rest is composition.

## Chapter 4 — Operators

WPE provides five operators that define how components combine and interact. Each operator has clear semantics that follow from its structure.

**Sequence (`*`)** — information flows left to right:

```
P:1@0|-3 * C:2@30|-2.5 * B:3@60|-2
```

This creates a pipeline. P feeds into C, which feeds into B. The coupling strength between each pair comes from their phase difference.

P to C coupling:
```
cos(30° - 0°) = cos(30°) = 0.866
```

C to B coupling:
```
cos(60° - 30°) = cos(30°) = 0.866
```

Strong forward flow. Information transfers with high fidelity but some transformation at each stage.

The sequence operator is transitive. If you write `A * B * C * D`, information flows A→B→C→D in order. Each connection's strength depends on the phase relationship between adjacent components.

**Parallel (`+`)** — independent branches:

```
[P:1@0|-3 * C:2@30|-2] + [B:1@90|-3 * S:2@120|-2]
```

Two processing paths that operate independently. The first branch processes P→C. The second processes B→S. They don't interact during processing.

Why independent? Look at the phase relationship between branches. P (0°) and B (90°):
```
cos(90° - 0°) = cos(90°) = 0.0
```

Zero coupling. The branches don't interfere with each other.

Results combine at the output:
```
[P:1@0|-3 * C:2@30|-2] + [B:1@90|-3 * S:2@120|-2] => O:2@180|-2
```

Both branches feed into O, but during processing they remain separate.

**Compose (`[ ]`)** — group subsystems:

```
[P:1@0|-3 * C:2@30|-2 * B:3@60|-2]
```

The brackets create a unit. Everything inside is treated as a single subsystem. It has internal structure (the sequence P→C→B) but presents a unified interface to the outside.

This enables hierarchical organization. You can nest compositions:

```
[[P:1@0|-3 * C:2@30|-2] * [B:3@60|-2 * S:4@90|-1.8]] => O:2@120|-2
```

Two subsystems in sequence, each containing its own internal sequence.

Composition also aids readability. Complex systems become manageable when broken into logical units.

**Output (`=>`)** — extract final results:

```
P:1@0|-3 * C:2@45|-2 * B:3@90|-1.5 => O:2@180|-2
```

Everything before the `=>` feeds into the output component. The output typically sits at high phase—often 180°, 270°, or 360°—representing system completion.

The output component can be any domain. Often it's `O` (output domain), but it could be the same domain as the processing components:

```
C:1@0|-3 * C:2@45|-2 * C:3@90|-1.5 => C:2@180|-2
```

Cognitive processing from input through to cognitive output.

**Couple (`<->`)** — bidirectional influence:

```
P:1@0|-2 <-> C:1@180|-2
```

P and C influence each other mutually. The coupling strength still comes from phase relationship:
```
cos(180° - 0°) = cos(180°) = -1.0
```

Strong negative coupling. They oppose each other.

Bidirectional coupling creates feedback:

```
P:1@0|-3 * C:2@90|-2 * B:3@180|-1.5 => O:2@270|-2 <-> P:1@0
```

Linear processing with output feeding back to input. Creates sustained oscillation or regulation.

**Operator composition**

These operators compose naturally. A complete system might look like:

```
[
  [P:1@0|-3 * C:2@30|-2.5] +
  [B:1@90|-2.5 * S:2@120|-2]
] * C:4@150|-1.8 => O:2@180|-2 <-> P:1@0
```

Reading this:
1. Two parallel branches process P→C and B→S independently
2. Their results compose into a unit
3. That unit feeds into integration component C:4
4. Integration produces output O
5. Output couples back to input P

The phase relationships determine all coupling strengths automatically:
- P→C: cos(30°) = 0.866
- B→S: cos(30°) = 0.866
- Branch interaction: cos(90°) = 0.0 (independent)
- Integration coupling depends on how the parallel results combine
- Feedback coupling: cos(360° - 0°) = cos(0°) = 1.0 (or explicit override)

The operators give you the building blocks. Phase geometry provides the coupling. Shell hierarchy adds vertical influence. Curvature determines stability.

Together, they let you encode complex structural relationships in a compact, readable notation.

## Chapter 5 — Phase Geometry and Coupling

Phase geometry is how WPE encodes relationships between components. The angular separation in phase space directly determines interaction strength. This isn't metaphorical—it's computational.

Let's explore the full range of phase relationships and what they mean.

**Perfect alignment (Δθ = 0°)**

```
C:1@45|-2 * C:2@45|-2.5
```

Both components at phase 45°. Phase difference is zero. Coupling strength:
```
cos(0°) = 1.0
```

What this means: maximum positive coupling. Information transfers with perfect fidelity. No transformation, no resistance. The components are essentially continuous.

Use cases:
- Maintaining state across hierarchical levels
- Direct transmission without processing
- Parallel activations that should move together

Example:
```
P:1@0|-3 * P:2@0|-2.5 * P:3@0|-2
```

Three hierarchical levels all at phase 0°. They track together perfectly. When one activates, all activate proportionally.

**Small angular separation (Δθ = 15-30°)**

```
P:1@0|-3 * C:2@15|-2.5 * B:2@30|-2
```

Phase differences of 15° and 15°. Coupling strengths:
```
cos(15°) = 0.966
cos(15°) = 0.966
```

Very strong positive coupling, but not quite perfect. This is typical sequential processing where each stage builds on the previous with high fidelity but some transformation.

The small phase shift indicates that information is being processed but not radically altered. Feature extraction from raw sensory data. Incremental refinement. Successive approximations.

Example - visual processing pipeline:
```
P:1@0|-5 * C:2@15|-4 * C:3@30|-3.5 * C:4@45|-3
```

Photoreceptors (0°) → edge detection (15°) → feature extraction (30°) → object recognition (45°)

Each step closely coupled to the previous (cos(15°) = 0.966), but with increasingly abstract representations.

**Moderate angular separation (Δθ = 45-60°)**

```
P:1@0|-3 * C:2@45|-2 * B:3@90|-1.5
```

Phase differences of 45° and 45°. Coupling strengths:
```
cos(45°) = 0.707
cos(45°) = 0.707
```

Moderate positive coupling. Significant transformation occurs between stages. Information flows forward but undergoes substantial processing.

This is the sweet spot for multi-stage reasoning. Each stage builds on the previous but adds significant new computation. Not mere transmission, but genuine transformation.

Example - decision making:
```
P:1@0|-4 * C:2@45|-3 * C:3@90|-2 * C:4@135|-1.5 => O:2@180|-2
```

Perception (0°) → evaluation (45°) → deliberation (90°) → choice (135°) → action (180°)

Each stage does substantial work, transforming the representation significantly.

**Orthogonal (Δθ = 90°)**

```
C:1@0|-2.5 * C:1@90|-2.5
```

Phase difference 90°. Coupling strength:
```
cos(90°) = 0.0
```

Zero coupling. Complete independence. The components don't interact at all.

Use cases:
- Parallel sensory modalities (vision, audition, touch)
- Independent processing streams
- Separate memory systems
- Concurrent activities that shouldn't interfere

Example - multi-modal perception:
```
[P:1@0|-3 * C:2@30|-2.5] +
[P:1@90|-3 * C:2@120|-2.5] +
[P:1@180|-3 * C:2@210|-2.5] =>
C:4@270|-2
```

Three sensory modalities at 90° spacing (0°, 90°, 180°). Each processes independently (cos(90°) = 0). Integration at 270° combines them.

**Triangular structure (Δθ = 120°)**

```
P:1@0|-2 * P:1@120|-2 * P:1@240|-2
```

Three components evenly spaced around the circle. Each pair has phase difference 120°:
```
cos(120°) = -0.5
```

Moderate negative coupling. Each component opposes the other two equally.

This creates frustrated optimization. The system can't satisfy all bonds simultaneously. If A and B align, C opposes both. If A and C align, B opposes both.

The result: dynamic tension, oscillation, or competitive balance.

Example patterns across domains:

**Physics** - three-phase electrical power:
```
P:1@0|-3.5 * P:1@120|-3.5 * P:1@240|-3.5 => O:2@180|-2.5
```

Phase A, B, C at 120° spacing. The -0.5 coupling creates perfect cancellation: V_A + V_B + V_C = 0 at all times.

**Biology** - predator-prey-resource:
```
B:1@0|-1.5 * B:1@120|-2.0 * B:1@240|-2.5 => O:2@180|-1.8
```

Resources, herbivores, predators at 120°. Each population opposes (competes with or consumes) the others. Creates oscillating dynamics.

**Cognition** - attention-memory-processing:
```
C:1@0|-2.0 * C:1@120|-2.2 * C:1@240|-2.4 => O:2@180|-1.5
```

Three cognitive resources competing for limited capacity. Increasing one reduces the others (zero-sum allocation).

**Social** - leader-follower-mediator:
```
S:1@0|-2.5 * S:1@120|-2.0 * S:1@240|-2.3 => O:2@180|-1.8
```

Three-way power dynamic. Each actor's influence is partially opposed by the other two.

The geometric structure is identical across all these domains. What changes is the physical substrate and the resulting equations.

**Opposition structure (Δθ = 180°)**

```
C:1@0|-2 <-> C:1@180|-2
```

Maximum separation. Phase difference 180°. Coupling strength:
```
cos(180°) = -1.0
```

Maximum negative coupling. Perfect opposition. Complete cancellation when combined.

Use cases:
- Excitation-inhibition balance
- Supply-demand equilibrium
- Error signals with opposite sign
- Complementary forces

Example - neural excitation-inhibition:
```
C:1@0|-3 <-> C:1@180|-3 => O:2@90|-2
```

Excitatory population (0°) and inhibitory population (180°) in mutual opposition. Output at 90° (orthogonal to both) represents the balanced activity state.

The -1.0 coupling means they cancel perfectly when equal. The system naturally seeks equilibrium.

Example - economic supply-demand:
```
S:1@0|-2.5 * S:1@180|-2.5 => O:2@90|-1.8
```

Supply (0°) and demand (180°) in opposition. Price (output at 90°) adjusts to balance them.

**Auto-coupling behavior**

By default, components within 90° couple automatically. Their interaction strength follows the cosine rule.

Beyond 90°, coupling becomes negative. Components still interact, but the influence is oppositional rather than supportive.

You can override auto-coupling with flags:
```
C:1@0|-2 * C:1@45|-2 @no_auto_coupling
```

Now they don't couple despite being within 90°. Useful for representing independent components that happen to have similar phases.

**Phase optimization strategies**

When designing a system, choose phases based on desired coupling:

**For sequential processing:** Space phases 15-45° apart. This gives strong positive coupling (0.866-0.707). Information flows forward with high fidelity.

**For parallel operations:** Use 60-120° spacing. This gives weak to zero coupling (0.5-0.0 or negative). Processes remain independent.

**For opposition/balance:** Place components 180° apart. Maximum negative coupling creates balance or cancellation.

**For three-way competition:** Use 120° spacing. Triangular structure creates frustrated optimization.

**For hierarchical levels at same phase:** Use 0° spacing between shells. They track together vertically.

The geometry does the computation. Once you set the phases, all coupling strengths follow automatically from the cosine rule.

This is why WPE is called "semantic calculus." The geometric relationships aren't just metaphorical—they're computational. The syntax directly determines the behavior.

## Chapter 6 — Curvature and Stability

Curvature determines whether a component forms a stable well that attracts and holds information, or an unstable peak that repels it. This isn't just a label—it's computational. Curvature values directly affect how information flows and persists in the system.

Let's examine the full spectrum of curvature values and their meanings.

**Deep wells (κ < -3.0)**

```
M:4@90|-4.5
```

Strong negative curvature creates a deep potential well. Information that enters this component gets strongly attracted and deeply trapped.

Characteristics:
- High persistence - information stays for long periods
- Strong attraction - nearby information gets pulled in
- Resistant to perturbation - stable against noise
- Difficult to update - once set, hard to change

Use cases:
- Long-term memory storage
- Core beliefs and values
- Fundamental system parameters
- Permanent structural features

Example - episodic memory:
```
@temporal_scale α=86400

[Memory] =
  P:1@0|-5 *
  C:2@45|-3.5 *
  M:4@120|-4.5 =>
  O:2@180|-2
```

Sensory input (κ=-5, fast) → consolidation (κ=-3.5, moderate) → long-term storage (κ=-4.5, very stable).

The memory component has deep curvature. Once information gets stored there, it persists. The time constant τ = 1/4.5 = 0.22 days with α=86400, meaning memories last months to years.

**Standard wells (-3.0 ≤ κ ≤ -1.0)**

```
C:2@45|-2.0
```

This is the most common range. Stable enough to hold information during processing, but not so deep that it can't update.

Characteristics:
- Moderate persistence - information stays while needed
- Balanced attraction - holds without trapping
- Responsive to updates - can be modified
- Good for processing - stable but flexible

Use cases:
- Working memory
- Active processing stages
- Intermediate representations
- Computational buffers

Example - sequential processing:
```
P:1@0|-3 * C:2@30|-2.5 * C:3@60|-2.0 * C:4@90|-1.8 => O:2@120|-2
```

Input (κ=-3, more stable) → early processing (κ=-2.5) → mid processing (κ=-2.0) → late processing (κ=-1.8)

Curvature gradually becomes less negative, making later stages more flexible. Early stages hold information more firmly.

**Shallow wells (-1.0 < κ < 0)**

```
C:2@30|-0.8
```

Weak negative curvature creates a shallow well. Information passes through but doesn't stick around.

Characteristics:
- Low persistence - transient states
- Weak attraction - easy to enter and exit
- Highly responsive - updates rapidly
- Unstable to noise - easily perturbed

Use cases:
- Temporary buffers
- Short-term activation
- Transient states
- Fast-changing variables

Example - working memory buffer:
```
[ShortTerm] =
  P:1@0|-5 *
  C:2@30|-0.8 *
  C:3@90|-2.5 =>
  O:2@180|-2
```

Fast input (κ=-5) → transient buffer (κ=-0.8) → consolidation (κ=-2.5) → output

The buffer at κ=-0.8 holds information briefly (τ = 1.25 time units) before it either consolidates or fades.

**Critical points (κ = 0)**

```
C:3@90|0
```

Zero curvature creates an unstable equilibrium. The system is balanced on a knife edge.

Characteristics:
- No inherent stability - equally likely to go either way
- Sensitive to perturbations - small changes have large effects
- Decision points - where the system "chooses"
- Phase transitions - where qualitative changes occur

Use cases:
- Decision nodes
- Bifurcations
- Threshold crossing
- State transitions

Example - decision making:
```
[Decision] =
  P:1@0|-3 *
  C:2@45|-2.5 *
  C:3@90|0 *
  C:4@135|-4.5 =>
  O:2@180|-2.5
```

Evidence accumulation (κ=-3, -2.5) → critical decision point (κ=0) → commitment (κ=-4.5, very stable)

At the critical point (90°, κ=0), the system is unstable. Small evidence differences push it toward one choice or another. Once past the critical point, commitment (κ=-4.5) makes the decision sticky.

**Peaks (κ > 0)**

```
C:2@180|+2.0
```

Positive curvature creates a potential peak. Information is actively repelled.

Characteristics:
- Anti-stability - system pushes away
- Repulsion - information doesn't accumulate
- Avoided states - system actively escapes
- Suppression - inhibited configurations

Use cases:
- Suppressed states
- Prohibited configurations
- Actively inhibited patterns
- Taboo states

Example - cognitive suppression:
```
[CognitiveControl] =
  C:1@0|-3 *
  C:2@90|+2.0 *
  C:3@180|-2.5 =>
  O:2@270|-2
```

Desired thought (κ=-3, stable) → suppressed thought (κ=+2.0, repelled) → alternative thought (κ=-2.5, stable)

The suppressed state at κ=+2.0 is actively avoided. If activity accidentally enters that state, it rapidly escapes.

Positive curvature is rare but useful. It represents things the system should not do, states it should avoid, configurations that are forbidden.

**Curvature in temporal systems (TME)**

In TME, curvature takes on a dual meaning. It still represents stability (negative = stable, positive = unstable), but it also determines duration.

The time constant is:
```
τ = 1/|κ|
```

Let's see what this means for different curvature values:

κ = -8.0 → τ = 0.125 (ultra-fast)
κ = -5.0 → τ = 0.200 (very fast)
κ = -3.0 → τ = 0.333 (fast)
κ = -2.0 → τ = 0.500 (moderate)
κ = -1.0 → τ = 1.000 (slow)
κ = -0.5 → τ = 2.000 (very slow)

Example - fast-slow temporal cascade:
```
@temporal_scale α=0.001

[NeuralSpike] =
  P:1@0|-8 *
  C:1@30|-5 *
  B:1@60|-1 =>
  O:1@120|-3
```

With α=0.001 (millisecond scale):
- Spike initiation (κ=-8): τ = 0.125 ms (ultra-fast)
- Propagation (κ=-5): τ = 0.200 ms (very fast)
- Recovery (κ=-1): τ = 1.000 ms (slow)
- Output (κ=-3): τ = 0.333 ms (fast)

The curvature values create the temporal profile of an action potential. Fast depolarization, slower recovery.

**Choosing curvature values: practical guidelines**

For spatial systems (WPE):
- Long-term stable states: κ < -3.5
- Active processing: κ ≈ -2.0 to -2.5
- Working memory: κ ≈ -1.5 to -2.5
- Transient buffers: κ ≈ -0.5 to -1.0
- Decision points: κ ≈ 0 to -0.5
- Suppressed states: κ > 0

For temporal systems (TME):
- Ultra-fast events (< 0.2τ): κ ≈ -5 to -10
- Fast events (0.2-0.5τ): κ ≈ -3 to -5
- Moderate events (0.5-1.0τ): κ ≈ -1.5 to -3
- Slow events (1.0-2.0τ): κ ≈ -0.5 to -1.5
- Very slow events (> 2.0τ): κ ≈ -0.5 or less

The sign determines stability. The magnitude determines strength (spatial) or speed (temporal). Together they define how the component behaves.

## Chapter 7 — Shell Hierarchy

Shells organize components vertically. Higher shells provide context and constraints for lower shells. This creates a natural hierarchy where abstract goals shape concrete actions, but without micromanagement.

The influence strength from higher to lower shell is:
```
Influence(λ_high → λ_low) ∝ 1/λ_low - 1/λ_high
```

Let's see what this means across the full range.

**Shell definitions**

**Level 1 (Foundation):**
Raw, concrete, physical layer. Direct measurements. Sensory input. Base-level phenomena. The actual substrate.

Examples:
- Photoreceptor activations
- Temperature measurements
- Population counts
- Raw market data

**Levels 2-3 (Processing):**
Operational transformations. Data processing. Feature extraction. Computation on the raw input.

Level 2: Basic operations (filtering, normalization, simple transforms)
Level 3: Complex operations (pattern detection, classification, computation)

Examples:
- Edge detection from raw pixels
- Frequency analysis from time series
- Rate calculations from counts
- Price derivatives from market data

**Levels 4-6 (Integration):**
Synthesis and combination. Bringing together multiple streams. Building coherent representations.

Level 4: Combining within a domain
Level 5: Cross-domain integration
Level 6: High-level synthesis

Examples:
- Object recognition from features
- Scene understanding from objects
- Market regime identification from indicators
- Ecological state from population dynamics

**Levels 7-9 (Abstraction):**
Meta-level reasoning. Abstract concepts. Reasoning about the system itself.

Level 7: Domain-specific abstraction
Level 8: Cross-domain abstract concepts
Level 9: Fundamental principles, meta-reasoning

Examples:
- Strategic planning
- Theoretical frameworks
- Philosophical principles
- Universal laws

**Influence calculations**

Let's compute influence strengths for various shell combinations:

**L9 → L1** (highest to lowest):
```
1/1 - 1/9 = 1.000 - 0.111 = 0.889
```
Very strong influence. Fundamental principles heavily constrain concrete actions.

**L7 → L1** (high to low):
```
1/1 - 1/7 = 1.000 - 0.143 = 0.857
```
Strong influence. Strategic goals shape ground-level behavior.

**L5 → L1** (mid-high to low):
```
1/1 - 1/5 = 1.000 - 0.200 = 0.800
```
Significant influence. Integration level guides foundation.

**L3 → L1** (mid to low):
```
1/1 - 1/3 = 1.000 - 0.333 = 0.667
```
Moderate influence. Processing guides measurement.

**L2 → L1** (adjacent):
```
1/1 - 1/2 = 1.000 - 0.500 = 0.500
```
Moderate influence. Immediate processing guides input.

**L5 → L4** (adjacent at higher levels):
```
1/4 - 1/5 = 0.250 - 0.200 = 0.050
```
Weak influence. Adjacent high-level shells have minimal coupling.

**L9 → L8** (adjacent at highest levels):
```
1/8 - 1/9 = 0.125 - 0.111 = 0.014
```
Very weak influence. Highest levels maintain independence.

**Pattern:** Influence is strongest when going from high to low levels. It's weakest between adjacent high levels. This creates a natural hierarchy where top-down constraint is strong but top levels don't micromanage each other.

**Example: Visual perception hierarchy**

```
[VisualSystem] =
  P:1@0|-5 *                    // L1: Photoreceptors
  C:2@15|-4 *                   // L2: Edge detection
  C:3@30|-3.5 *                 // L3: Feature extraction
  C:4@60|-3 *                   // L4: Object recognition
  C:5@90|-2.5 *                 // L5: Scene understanding
  C:6@120|-2 *                  // L6: Semantic interpretation
  C:7@150|-1.5 =>               // L7: Conceptual meaning
  O:2@180|-2
```

Influence strengths:
- L7 → L1: 0.857 (semantic meaning strongly influences what we see)
- L6 → L3: 0.583 (interpretation guides feature extraction)
- L5 → L4: 0.050 (scene guides objects weakly - mostly bottom-up)
- L4 → L3: 0.083 (objects guide features weakly)

The hierarchy creates top-down influence (expectations shape perception) while allowing bottom-up flow (data drives recognition).

**Example: Motor control hierarchy**

```
[MotorControl] =
  C:9@0|-1.0 *                  // L9: Intention ("reach for cup")
  C:7@30|-1.2 *                 // L7: Goal ("hand to location")
  C:5@60|-1.5 *                 // L5: Trajectory (path planning)
  C:3@90|-2.0 *                 // L3: Muscle synergies
  P:1@120|-3 =>                 // L1: Muscle activations
  O:2@150|-2.5
```

Influence strengths:
- L9 → L1: 0.889 (intention strongly constrains muscles)
- L7 → L1: 0.857 (goals constrain execution)
- L5 → L1: 0.800 (trajectory constrains activations)
- L3 → L1: 0.667 (synergies moderate constrain muscles)

High-level intentions shape every level below, but each level has autonomy proportional to its distance from the top.

**Example: Organizational hierarchy**

```
[Organization] =
  S:9@0|-0.8 *                  // L9: Mission/vision
  S:7@45|-1.0 *                 // L7: Strategic goals
  S:5@90|-1.5 *                 // L5: Department objectives
  S:3@135|-2.0 *                // L3: Team plans
  S:1@180|-2.5 =>               // L1: Individual actions
  O:2@225|-2
```

Influence strengths:
- L9 → L1: 0.889 (mission strongly influences everyone)
- L7 → L3: 0.583 (strategy moderately influences teams)
- L5 → L3: 0.083 (departments weakly influence teams directly)
- L3 → L1: 0.667 (team plans moderate influence individuals)

The formula creates realistic organizational dynamics. Vision matters everywhere. Strategy shapes broad patterns. But teams have significant autonomy.

**Why this formula?**

The influence formula `1/λ_low - 1/λ_high` isn't arbitrary. It emerges from the geometry of hierarchical influence.

Consider the "span" from high to low level:
```
Span = λ_low - λ_high
```

This is the number of levels between them. But raw span doesn't capture influence—going from L9 to L8 (span=1) shouldn't have the same influence as L2 to L1 (span=1).

The reciprocal formula accounts for both span and absolute level. It gives:
- Strong influence for large spans
- Stronger influence at lower absolute levels
- Weak influence between adjacent high levels

This matches how real hierarchies work. CEOs influence janitors more than they influence VPs. Physical laws constrain particles more than they constrain abstract concepts.

**Practical guidelines**

When assigning shells:
- Put raw data at L1
- Put basic operations at L2-3
- Put integration at L4-6
- Put abstraction at L7-9

Use the influence formula to check if your hierarchy makes sense:
- Should high levels constrain low? Then the 1/λ_low - 1/λ_high should be large (> 0.5)
- Should adjacent levels be loosely coupled? Then the difference should be small (< 0.1)

Shell hierarchy isn't just organizational. It's computational. Higher shells provide context that shapes how lower shells process information.

## Chapter 8 — Extended Syntax

Beyond basic components, WPE provides extended syntax for more complex patterns. These extensions don't change the fundamental rules—they're syntactic conveniences that expand into standard WPE.

**Multi-phase weighted presence**

```
C:2@[90:0.7+95:0.3]|-1.8
```

The component is active at multiple phases with different weights:
- 70% present at phase 90°
- 30% present at phase 95°

Total effect is the weighted sum of contributions at each phase.

When does this coupling to other components? The weighted average of phase contributions.

For example, coupling to a component at phase 0°:
```
Coupling = 0.7·cos(90° - 0°) + 0.3·cos(95° - 0°)
         = 0.7·0.0 + 0.3·(-0.087)
         = 0.0 + (-0.026)
         = -0.026
```

Very weak negative coupling (mostly orthogonal with slight opposition).

Use cases:
- Components that span multiple functional roles
- Distributed representations
- States with ambiguous phase
- Smooth transitions between phases

Example - attention shifting:
```
[AttentionShift] =
  C:2@[0:0.8+30:0.2]|-2 *           // Mostly old focus, some new
  C:2@[0:0.5+30:0.5]|-2 *           // Equal old and new
  C:2@[0:0.2+30:0.8]|-2 =>          // Mostly new focus, some old
  O:2@60|-2
```

Attention gradually shifts from phase 0° to phase 30° through weighted intermediate states.

**Temporal evolution**

```
C:2@Θ(t)|-2
```

Phase changes over time following function Θ(t). The component moves through phase space dynamically.

Common forms:

**Linear motion:**
```
Θ(t) = θ₀ + ω·t
```
Constant angular velocity ω. Component sweeps through phases at steady rate.

**Oscillation:**
```
Θ(t) = θ₀ + A·sin(ω·t)
```
Sinusoidal oscillation around θ₀ with amplitude A and frequency ω.

**Damped oscillation:**
```
Θ(t) = θ₀ + A·exp(-γ·t)·sin(ω·t)
```
Oscillation that decays with damping constant γ.

**Relaxation:**
```
Θ(t) = θ_final + (θ₀ - θ_final)·exp(-t/τ)
```
Exponential approach from θ₀ to θ_final with time constant τ.

Use cases:
- Oscillating processes
- Phase locking
- Entrainment
- Cyclic behaviors

Example - circadian rhythm:
```
@temporal_scale α=86400

[Circadian] =
  B:1@Θ(t)|-1.5 =>
  O:2@180|-1.8

where Θ(t) = 180·sin(2π·t)
```

With α=86400 (one day), the phase oscillates through a full cycle every 24 hours. At t=0.25 days (6 AM), Θ=180° (maximum). At t=0.75 days (6 PM), Θ=-180° (minimum, equivalent to 180° on opposite side).

**Pattern shorthand**

```
M:5@recurring_structure|-4.0
```

High-level abstract pattern compressed into single component. The pattern can be complex internally but presents as a unit.

For example, you might define:
```
RecurringStructure = [P:1@0|-3 * C:2@30|-2.5 * B:3@60|-2]
```

Then reference it:
```
M:5@RecurringStructure|-4.0
```

The parser expands this when needed, but in the encoding it stays compact.

Use cases:
- Frequently occurring motifs
- Compressed representations
- Hierarchical abstractions
- Learned patterns

**Range notation**

```
C:1@[0:30:60:90]|-2.5
```

Shorthand for equal weights at 0°, 30°, 60°, and 90°. Equivalent to:
```
C:1@[0:0.25+30:0.25+60:0.25+90:0.25]|-2.5
```

The component is distributed uniformly across the phase range.

Use cases:
- Broadly tuned responses
- Distributed representations
- Multiple activation modes
- Redundant coverage

Example - population coding:
```
[PopulationCode] =
  C:1@[0:30:60:90:120:150:180:210:240:270:300:330]|-2.5 =>
  O:2@180|-2
```

12 components evenly distributed around the circle (every 30°). Together they can represent any phase through their combined activation pattern.

**Combining extensions**

You can combine these extensions:

```
C:3@[Θ₁(t):0.6+Θ₂(t):0.4]|-κ(t)
```

A component with:
- Two time-varying phases (Θ₁(t) and Θ₂(t))
- Weighted 60/40
- Time-varying curvature κ(t)

This is complex but sometimes necessary for highly dynamic systems.

Example - adaptive oscillator:
```
@temporal_scale α=1.0

[AdaptiveOsc] =
  C:2@[Θ₁(t):w(t)+Θ₂(t):(1-w(t))]|-κ(t) =>
  O:2@180|-2

where:
  Θ₁(t) = 45 + 30·sin(ω₁·t)
  Θ₂(t) = 135 + 30·sin(ω₂·t)
  w(t) = 0.5 + 0.5·tanh(signal(t))
  κ(t) = -2 - 2·tanh(stability(t))
```

The oscillator adapts its phase weighting and curvature based on external signals. When signal(t) is high, w(t) → 1 (mostly Θ₁). When low, w(t) → 0 (mostly Θ₂). When stability(t) is high, κ(t) → -4 (very stable). When low, κ(t) → 0 (critical).

These extensions provide flexibility without changing the core semantics. Everything still reduces to phase coupling (cos(Δθ)), shell influence (1/λ_low - 1/λ_high), and curvature stability (negative = well, positive = peak).

The fundamental rules remain. The extensions just make complex patterns more expressible.

---

# PART III — TME: TEMPORAL SEMANTICS

## Chapter 9 — Time from Syntax

TME (Temporal Modulation Encoding) extends WPE with explicit temporal interpretation. Instead of adding time as a separate parameter, time emerges from the structure itself.

**Core principle:** Sequential phase ordering equals temporal progression.

Read a TME encoding left to right, and you're reading forward in time. Components with lower phases happen earlier. Components with higher phases happen later. Monotonic phase increase within a layer defines temporal sequence.

**Basic temporal structure:**

```
@temporal_scale α=1.0

System =
  P:1@0|-3 *
  C:1@45|-2 *
  B:1@90|-1.5 =>
  O:1@180|-2
```

Component P happens first (phase 0°). Then C (phase 45°). Then B (phase 90°). Finally output (phase 180°).

How much time elapses? That depends on curvature and temporal scale.

Each component has a time constant:
```
τᵢ = 1/|κᵢ|
```

For P (κ=-3): τ = 1/3 = 0.333 time units
For C (κ=-2): τ = 1/2 = 0.5 time units
For B (κ=-1.5): τ = 1/1.5 = 0.667 time units
For O (κ=-2): τ = 1/2 = 0.5 time units

The system timing is approximately:
```
T_total ≈ Σ(Δθᵢ/360°) × τᵢ
```

Where Δθᵢ is the phase coverage of component i.

P covers 0° to 45° (12.5% of circle): 0.125 × 0.333 = 0.042
C covers 45° to 90° (12.5%): 0.125 × 0.5 = 0.063
B covers 90° to 180° (25%): 0.25 × 0.667 = 0.167
O covers 180° to completion (depends on total): ~0.25 × 0.5 = 0.125

Total time ≈ 0.397 time units (call it 0.4)

The temporal scale α multiplies everything:
- If α = 1.0 (default): Total time = 0.4 time units
- If α = 2.0 (2× slower): Total time = 0.8 time units
- If α = 0.5 (2× faster): Total time = 0.2 time units

**Why this works**

Phases represent positions around a circle. If we traverse the circle clockwise, each phase marks when that component becomes active.

Phase differences translate to temporal separation. A component at 90° happens one quarter of the way through the cycle. A component at 180° happens halfway through.

But unlike clock time, TME captures both duration (via curvature) and ordering (via phase sequence). You don't need absolute timestamps. The structure itself encodes temporal relationships.

This has practical advantages:
1. Temporal relationships stay explicit even if total duration changes
2. You can scale everything uniformly (adjust α) without changing structure
3. Parallel processes use separate layers with independent timelines
4. Feedback loops create natural temporal cycles

**Key insight:** Time isn't a separate dimension in TME. It's already present in WPE's phase geometry. TME simply makes the temporal interpretation explicit through ordering constraints.

## Chapter 10 — Phase as Time, Curvature as Duration

In TME, phase and curvature have dual meanings. Spatially (in WPE), phase determines coupling and curvature determines stability. Temporally (in TME), phase determines when and curvature determines how long.

**Phase spacing determines timing**

Small spacing (15-30°) means rapid succession:

```
P:1@0|-5 * C:1@15|-6 * B:1@30|-4
```

Events happen in quick sequence. Tight temporal coupling. One thing triggers the next almost immediately.

If each component has typical curvatures (κ ≈ -5):
- P: phase 0°, τ = 0.2
- C: phase 15° (4.2% into cycle), τ = 0.167
- B: phase 30° (8.3% into cycle), τ = 0.25

Total time from P to B: about 0.1-0.2 time units (depending on α).

Medium spacing (45-90°) means distinct sequential stages:

```
P:1@0|-3 * C:1@60|-2 * O:1@120|-2
```

Clear separation between stages. Each has time to complete before the next begins.

Timing:
- P: phase 0°, τ = 0.333
- C: phase 60° (16.7% into cycle), τ = 0.5
- O: phase 120° (33.3% into cycle), τ = 0.5

Total time: about 0.5-0.7 time units.

Large spacing (120-180°) means separated phases:

```
P:1@0|-2 * C:1@120|-1.5 * B:1@240|-1.8
```

Long intervals between events. Different temporal windows with significant gaps.

Timing:
- P: phase 0°, τ = 0.5
- C: phase 120° (33.3%), τ = 0.667
- B: phase 240° (66.7%), τ = 0.556

Total time: 1.0-1.5 time units (events well separated).

**Phase coupling as temporal correlation**

The cosine coupling rule has temporal interpretation:

cos(15°) = 0.966 → Events strongly correlated in time (happen close together)
cos(45°) = 0.707 → Events moderately correlated (sequential but separated)
cos(90°) = 0.0 → Events temporally independent (can happen in parallel)
cos(180°) = -1.0 → Events temporally anticorrelated (opposite phases)

This creates coherence. Events that should coordinate have similar phases (strong positive coupling). Events that should separate have large phase differences (weak or negative coupling).

**Curvature as duration**

Curvature magnitude determines how long each component's activity lasts:

```
τ = 1/|κ|
```

Fast events (high |κ|):

```
P:1@0|-8
```

τ = 1/8 = 0.125 time units. Brief, rapid event.

Examples:
- Neural spike initiation
- Chemical transition states
- Network packet transmission
- Threshold crossing

With α=0.001 (millisecond scale): 0.125 ms
With α=1 (second scale): 0.125 s

Moderate events (medium |κ|):

```
C:1@45|-2.5
```

τ = 1/2.5 = 0.4 time units. Standard processing duration.

Examples:
- Feature extraction
- Pattern matching
- Memory retrieval
- Decision evaluation

With α=1: 0.4 s
With α=60: 24 s (minutes)

Slow events (low |κ|):

```
B:1@90|-1.0
```

τ = 1/1.0 = 1.0 time unit. Prolonged process.

Examples:
- Memory consolidation
- Learning
- Adaptation
- Strategic planning

With α=1: 1 s
With α=3600: 1 hour
With α=86400: 1 day

**System timing example**

```
@temporal_scale α=0.001

[ActionPotential] =
  P:1@0|-8 *        // Depolarization (τ=0.125)
  C:1@30|-5 *       // Spike peak (τ=0.2)
  B:1@60|-1 *       // Repolarization (τ=1.0)
  B:1@150|-2 =>     // Recovery (τ=0.5)
  O:1@180|-3        // Complete (τ=0.333)
```

With α=0.001 (millisecond scale):

Phase coverage and durations:
- Depolarization: 0-30° (8.3%), 0.083 × 0.125 ms = 0.010 ms
- Spike: 30-60° (8.3%), 0.083 × 0.2 ms = 0.017 ms
- Repolarization: 60-150° (25%), 0.25 × 1.0 ms = 0.25 ms
- Recovery: 150-180° (8.3%), 0.083 × 0.5 ms = 0.042 ms
- Complete: 180° onward, 0.333 ms

Total: ~0.7 ms for full action potential

The curvature values create the characteristic temporal profile:
- Fast depolarization (κ=-8)
- Moderately fast spike (κ=-5)
- Slow repolarization (κ=-1)
- Moderate recovery (κ=-2)

**Choosing phase and curvature together**

For sequential fast events:
- Small phase spacing (15-30°)
- High curvature magnitude (κ = -5 to -8)
- Result: Rapid succession with brief durations

For parallel slow processes:
- Large phase spacing (90° or separate layers)
- Low curvature magnitude (κ = -0.5 to -1.5)
- Result: Independent prolonged processes

For cascading timescales:
- Moderate phase spacing (30-60°)
- Decreasing curvature magnitude (κ: -5 → -3 → -1)
- Result: Fast-to-slow cascade

The phase determines when. The curvature determines how long. Together they define the complete temporal structure.

## Chapter 11 — Layers and Ordering Rules

TME uses layers to represent parallel timelines. Each layer is an independent temporal sequence. Within a layer, phases must increase monotonically. Across layers, any phase relationship is allowed.

**Layer syntax:**

```
L1: P:1@0|-3 * C:1@45|-2 * B:1@90|-1.5
L2: S:1@30|-2.5 * S:1@120|-2
```

Layer 1 has its own timeline: P→C→B. Phases must increase: 0° ≤ 45° ≤ 90°.

Layer 2 has its own timeline: S→S. Phases must increase: 30° ≤ 120°.

But the layers are independent. L1 starts at 0°. L2 starts at 30°. They overlap in time but don't interfere (unless explicitly coupled).

**Why layers matter**

Real systems have parallel processes. Your visual system processes images while your auditory system processes sounds. Your brain consolidates memories while you sleep. Markets trade stocks while bonds while commodities simultaneously.

Layers make this concurrency explicit. Each layer is a separate temporal stream.

**Parallel start:**

```
L1: P:1@0|-3 * C:1@60|-2
L2: B:1@0|-3 * S:1@60|-2
```

Both L1 and L2 start at phase 0°. Both process to phase 60°. They run concurrently from the same start time.

**Offset timelines:**

```
L1: P:1@0|-5 * C:1@30|-3
L2: B:1@60|-1 * S:1@180|-2
```

L1 starts immediately (phase 0°) and completes at phase 30°.
L2 starts later (phase 60°) and completes much later (phase 180°).

They overlap in time:
- At phase 60°, L1 has finished but L2 is just starting
- At phase 180°, L1 finished long ago and L2 is completing

**Layer interaction:**

Layers can couple if you explicitly connect them:

```
L1: P:1@0|-3 * C:1@60|-2 => O1:2@120|-2
L2: B:1@30|-3 * S:1@90|-2 => O2:2@150|-2
L3: C:3@180|-1.5 <= O1 + O2
```

L1 and L2 process independently, each producing output. L3 waits until both outputs are ready (phase 180° is past both 120° and 150°), then combines them.

**Temporal scale parameter**

```
@temporal_scale α=value
```

This multiplies all time constants uniformly. It doesn't change phase relationships or layer structure. It just speeds up or slows down everything.

Examples:

α = 0.0001 (microsecond scale):
```
@temporal_scale α=0.0001

[Photon] = P:1@0|-8 * P:1@90|-10 => O:1@180|-6
```

Ultra-fast electromagnetic process. With κ ≈ -8 to -10, durations are τ ≈ 0.1-0.125. With α=0.0001, real times are 10-12.5 microseconds.

α = 1.0 (second scale - default):
```
@temporal_scale α=1.0

[Cognition] = C:1@0|-4 * C:2@60|-2 * C:3@120|-1.5 => O:2@180|-2
```

Cognitive process. Durations τ = 0.25, 0.5, 0.667 seconds. Total time ~1-2 seconds.

α = 86400 (day scale):
```
@temporal_scale α=86400

[Circadian] = B:1@0|-1.5 * B:1@120|-1.2 * B:1@240|-1.0 => O:2@360|-1.5
```

Daily rhythm. Each component lasts τ = 0.67-1.0 days. Total cycle ~1 day.

α = 3.156×10^7 (year scale):
```
@temporal_scale α=31560000

[Seasonal] = B:1@0|-2 * B:1@90|-1.5 * B:1@180|-2 * B:1@270|-1.5 => O:2@360|-1.8
```

Seasonal ecology. Four seasons (90° spacing), each lasting τ = 0.5-0.67 years (6-8 months). Total cycle ~1 year.

**Ordering rules**

TME enforces four rules that prevent temporal paradoxes:

**R1: Phase Monotonicity**

Within each layer, phases must increase:
```
θ₁ ≤ θ₂ ≤ θ₃ ≤ ... ≤ θₙ
```

✓ Valid:
```
L1: P:1@0 * C:1@30 * B:1@60 * S:1@90
```

✗ Invalid:
```
L1: P:1@0 * C:1@90 * B:1@30  // Violates monotonicity (90° then 30°)
```

Why? Decreasing phases would mean later events happen before earlier ones. That's acausal. TME prevents it syntactically.

**R2: Layer Independence**

Different layers can have any phase ordering:

✓ Valid:
```
L1: P:1@0 * C:1@90
L2: B:1@60 * S:1@120
L3: Ph:1@30 * Ph:1@180
```

L1 goes 0→90. L2 goes 60→120. L3 goes 30→180. Each layer has its own monotonic sequence, but layers start and end at different times.

**R3: Output Placement**

Output components should be at maximum phase within their layer:

✓ Typical:
```
L1: P:1@0 * C:1@45 * B:1@90 => O:1@180
```

Output at 180° marks completion. Everything before 180° has finished.

✗ Problematic:
```
L1: P:1@0 * C:1@90 => O:1@30  // Output before processing completes?
```

Output at 30° happens before C at 90°. This suggests the output is produced before processing finishes. Usually wrong.

Exception: Sometimes you want intermediate outputs:
```
L1: P:1@0 * C:1@45 => O1:1@60 * C:1@90 => O2:1@120
```

O1 is intermediate output at 60°. O2 is final output at 120°. Both are at local maxima for their stage.

**R4: No Wraparound**

Cannot go from 359° back to 0° within the same layer:

✗ Invalid:
```
L1: P:1@350 * C:1@10  // Wraps around from 350° to 10°
```

This would violate monotonicity. Phase must increase, not wrap.

If you need cyclic behavior, use a new layer:

✓ Valid:
```
L1: P:1@350 * C:1@360 => O:1@360
L2: P:1@10 * C:1@50    // New cycle in new layer
```

Or use explicit feedback:

✓ Valid:
```
P:1@0 * C:1@90 * B:1@180 * S:1@270 => O:1@360 <-> P:1@0
```

The `<->` creates a loop from output (360°) back to input (0°), representing the next cycle.

**Practical layering strategies**

**Independent parallel processes:**
```
L1: Vision:1@0|-4 * Vision:2@60|-3 => VisionOut:2@120|-2
L2: Audio:1@0|-4 * Audio:2@60|-3 => AudioOut:2@120|-2
L3: Integration:4@150|-2 <= VisionOut + AudioOut
```

Vision and audio process independently (L1, L2). Integration (L3) waits until both finish (phase 150° > 120°).

**Offset cascades:**
```
L1: Fast:1@0|-8 * Fast:1@15|-7 => FastOut:1@30|-6
L2: Slow:1@60|-1 * Slow:1@180|-0.8 => SlowOut:1@240|-1
L3: Combine:3@270|-2 <= FastOut + SlowOut
```

Fast process (L1) completes quickly (0→30°). Slow process (L2) starts later and takes longer (60→240°). Combination (L3) waits for both.

**Hierarchical coordination:**
```
L1: Low:1@0|-3 * Low:1@30|-3 * Low:1@60|-3
L2: Mid:3@45|-2 * Mid:3@90|-2  
L3: High:5@120|-1.5 => Out:5@180|-1.8
```

Low-level processes (L1) run continuously. Mid-level (L2) coordinates them. High-level (L3) integrates everything.

The layers don't just organize code. They define independent temporal streams with explicit synchronization points.

---

# PART IV — CROSS-DOMAIN TRANSLATION

## Chapter 12 — The Translation Principle

The same geometric structure produces different conventional equations across domains. This isn't a limitation—it's a feature. WPE/TME provides universal geometric relationships. Domain-specific physics fills in the details.

Why do domains differ? Four reasons:

**1. Substrates differ**

What do the variables represent?

Physics: Electromagnetic fields E(x,t), B(x,t), particle positions, momenta
Biology: Population densities N(t), chemical concentrations [X], metabolic rates
Cognition: Neural activations, information content, attention allocation
Social: Influence measures, economic quantities, relationship strengths

The same geometric coupling (cos(Δθ)=-0.5) means different things:
- Physics: Destructive electromagnetic interference
- Biology: Competitive inhibition between populations
- Cognition: Resource trade-off in attention allocation
- Social: Opposition in influence dynamics

**2. Constraints differ**

What physical laws must be satisfied?

Physics: Maxwell equations, conservation of energy, Lorentz invariance, thermodynamics
Biology: Non-negativity (populations can't be negative), carrying capacity, mass conservation, fitness optimization
Cognition: Metabolic limits, information bounds (Shannon), processing capacity constraints
Social: Rationality (utility maximization), market clearing, Nash equilibrium, power conservation

These constraints determine which configurations are physically realizable.

**3. Coupling mechanisms differ**

How do interactions physically occur?

Physics: Field superposition, particle collisions, electromagnetic forces, gravitational attraction
Biology: Predation (one population eats another), competition (for resources), mutualism, chemical reactions
Cognition: Synaptic transmission, neural competition, attention selection, memory interference
Social: Influence propagation, information flow, economic transactions, power dynamics

The mechanism determines how the geometric coupling translates to physical effects.

**4. Optimization criteria differ**

What does the system minimize/maximize?

Physics: Action (principle of least action), energy, entropy production
Biology: Fitness, survival probability, reproductive success
Cognition: Metabolic cost, information gain, prediction error
Social: Utility, social welfare, tension, transaction cost

The optimization criterion determines the equilibrium state.

**The translation process:**

1. **Parse geometry:** Extract all phases, curvatures, shells from WPE encoding
2. **Build coupling matrix:** Calculate cos(Δθ) for all component pairs
3. **Apply domain constraints:** Impose physical laws specific to the domain
4. **Setup optimization:** Define substrate-specific energy/cost functional
5. **Solve variational problem:** Minimize the functional subject to constraints
6. **Extract equations:** Derive conventional domain-specific equations
7. **Determine coefficients:** Geometric relationships provide numerical values

The geometry scaffolds. The physics specifies. Together they produce conventional mathematics.

Let's see this in detail with the canonical example: triangular structure across four domains.

## Chapter 13 — Triangular Structure: Universal Geometry

```
Triangle = [P:1@0|-2 * P:1@120|-2 * P:1@240|-2]
```

Three components at 120° spacing. This is geometrically special—maximum separation for three points on a circle.

**Coupling matrix:**
```
         0°      120°    240°
0°      1.0     -0.5    -0.5
120°   -0.5      1.0    -0.5
240°   -0.5     -0.5     1.0
```

Perfect symmetry. Each component couples to itself with +1.0 (identity). Each couples to the other two with -0.5 (moderate opposition).

Phase differences:
- 0° to 120°: Δθ = 120°, cos(120°) = -0.5
- 0° to 240°: Δθ = 240°, cos(240°) = cos(-120°) = -0.5
- 120° to 240°: Δθ = 120°, cos(120°) = -0.5

All pairs equally coupled with negative sign.

**What does -0.5 coupling mean?**

Geometrically: Moderate opposition. Not completely opposed (that would be -1.0 at 180°), but definitely not aligned.

Physically: Depends on domain. Let's see.

## Chapter 14 — Physics Domain: Three-Phase Power

```
@temporal_scale α=0.0000167  // 60 Hz = 1/60 second per cycle

[ElectricalGrid] = [
  P:1@0|-3.5 *
  P:1@120|-3.5 *
  P:1@240|-3.5
] => O:2@180|-2.5
```

**Substrate:** Electromagnetic field energy E(x,t)

Three voltage sources at phases 0°, 120°, 240°. They oscillate sinusoidally with the same frequency but different phases.

**Domain constraints:**

Maxwell equations:
```
∇·E = ρ/ε₀          (Gauss)
∇×E = -∂B/∂t        (Faraday)
∇·B = 0             (No monopoles)
∇×B = μ₀J + μ₀ε₀∂E/∂t  (Ampere-Maxwell)
```

Energy conservation:
```
∫(E² + B²)dV = constant
```

Lorentz invariance (for relativistic formulation)

**Optimization:** Minimize electromagnetic energy subject to Maxwell constraints.

**Solution:**

Phase 0° voltage:
```
V₀(t) = V_peak·sin(ωt)
```

Phase 120° voltage:
```
V₁₂₀(t) = V_peak·sin(ωt + 120°)
```

Phase 240° voltage:
```
V₂₄₀(t) = V_peak·sin(ωt + 240°)
```

**Check the geometric coupling:**

At any time t, sum the three voltages:
```
V_total(t) = V₀ + V₁₂₀ + V₂₄₀
           = V_peak[sin(ωt) + sin(ωt + 120°) + sin(ωt + 240°)]
```

Using trigonometric identities:
```
sin(ωt) + sin(ωt + 120°) + sin(ωt + 240°)
= sin(ωt) + sin(ωt)cos(120°) + cos(ωt)sin(120°)
           + sin(ωt)cos(240°) + cos(ωt)sin(240°)
= sin(ωt)[1 + cos(120°) + cos(240°)]
  + cos(ωt)[sin(120°) + sin(240°)]
= sin(ωt)[1 + (-0.5) + (-0.5)]
  + cos(ωt)[0.866 + (-0.866)]
= sin(ωt)[0] + cos(ωt)[0]
= 0
```

Perfect cancellation! The three voltages sum to zero at all times.

This comes directly from the -0.5 coupling. Three vectors with equal magnitude at 120° spacing sum to zero.

**Emergent properties:**

**1. Zero neutral current**

If the loads are balanced, the three phases cancel. You don't need a return path (neutral wire). This saves conductor material.

**2. √3 voltage multiplication**

Line-to-line voltage (between any two phases):
```
V_AB = V_A - V_B = V_peak[sin(ωt) - sin(ωt + 120°)]
```

Using trigonometric identities:
```
V_AB = V_peak·2·sin((0° - 120°)/2)·cos((0° + 120°)/2)
     = V_peak·2·sin(-60°)·cos(60°)
     = V_peak·2·(-√3/2)·(0.5)
     = V_peak·(-√3/2)
```

Magnitude: √3·V_peak

The line-to-line voltage is √3 times the phase voltage. This factor emerges from the 120° geometry.

**3. Rotating magnetic field**

Three coils at 120° spacing carrying these currents create a magnetic field that rotates in space. This drives electric motors.

**4. Constant power delivery**

For balanced loads:
```
P_total = V_A·I_A + V_B·I_B + V_C·I_C
        = 3·V_peak·I_peak·cos(φ)
```

Time-independent! The instantaneous power is constant, even though individual phases oscillate.

**Key insight:** The -0.5 coupling (from 120° geometry) creates cancellation, √3 multiplication, rotation, and constant power. All emerge from the geometric structure.

## Chapter 15 — Biology Domain: Predator-Prey-Resource

```
@temporal_scale α=31557600  // 1 year = 3.156×10^7 seconds

[Ecosystem] = [
  B:1@0|-1.5 *
  B:1@120|-2.0 *
  B:1@240|-2.5
] => O:2@180|-1.8
```

**Substrate:** Population densities R(t), H(t), P(t)

Resources (plants), Herbivores (prey), Predators at phases 0°, 120°, 240°.

**Same coupling matrix** (120° → -0.5)

**Domain constraints:**

Non-negativity:
```
R, H, P ≥ 0
```

Populations can't be negative.

Carrying capacity:
```
R ≤ K
```

Resources limited by environment.

Conservation (closed system):
```
Total biomass approximately conserved (matter cycles)
```

Growth rates determined by biology:
```
Birth rates > 0
Death rates > 0
Predation rates > 0
```

**Optimization:** Maximize population stability subject to ecological constraints.

**Solution (Lotka-Volterra variant):**

Resources:
```
dR/dt = r·R·(1 - R/K) - a·R·H
```

Growth (logistic) minus consumption by herbivores.

Herbivores:
```
dH/dt = b·R·H - c·H·P - d·H
```

Growth from eating resources, minus predation, minus natural death.

Predators:
```
dP/dt = e·H·P - f·P
```

Growth from eating herbivores minus natural death.

**Where do the coefficients come from?**

The -0.5 coupling appears in the interaction terms:

The coefficient on R·H in dR/dt represents negative impact (herbivores consume resources). The -0.5 geometric coupling contributes to determining how strong this effect is.

Similarly for H·P in dH/dt (predators consume herbivores) and the feedback from P to H dynamics.

The exact values (a, b, c, d, e, f) emerge from:
- Curvature values (κ_R=-1.5, κ_H=-2.0, κ_P=-2.5)
- Coupling strength (-0.5 from 120°)
- Domain-specific parameters (birth rates, death rates, capture efficiencies)

**Emergent properties:**

**1. Oscillating populations**

The system doesn't reach static equilibrium. Instead, populations oscillate:
- Resources increase when predators are low
- Herbivores increase following resource abundance
- Predators increase following herbivore abundance
- Predators deplete herbivores, which depletes resources, which depletes herbivores further
- Cycle repeats

**2. Phase delays**

Resources peak first. Then herbivores peak (delayed by ~120° in the cycle). Then predators peak (delayed another ~120°). The 120° geometric spacing creates temporal phase delays in the oscillations.

**3. Stable coexistence**

Despite predation, all three populations persist. The -0.5 coupling creates frustrated optimization—no single population can dominate completely. This leads to stable cycling rather than extinction.

**4. Resilience**

Perturbations (removing some predators, adding resources) don't destroy the cycle. The system returns to oscillation, possibly with different amplitude but same structure.

**Key insight:** Same -0.5 coupling, now representing ecological interactions (consumption, predation). Creates oscillations instead of cancellation. Different substrate, different physics, same geometry.

## Chapter 16 — Cognition Domain: Resource Competition

```
@temporal_scale α=1.0  // Seconds

[CognitiveResources] = [
  C:1@0|-2.0 *
  C:1@120|-2.2 *
  C:1@240|-2.4
] => O:2@180|-1.5
```

**Substrate:** Information processing capacity

Attention (A), Memory (M), Processing (P) at phases 0°, 120°, 240°.

**Same coupling matrix** (120° → -0.5)

**Domain constraints:**

Total capacity limit:
```
A + M + P ≤ C_max
```

You can't use more cognitive resources than you have.

Metabolic cost:
```
E(A,M,P) ≤ E_max
```

Cognition requires energy (glucose metabolism). Limited supply.

Information theory:
```
I(A,M,P) ≤ H_max
```

Information processing bounded by Shannon limits.

Non-negativity:
```
A, M, P ≥ 0
```

Can't have negative attention.

**Optimization:** Maximize information processing subject to capacity constraints.

**Solution:**

Attention:
```
A = A_max·[1 - 0.5(M/M_max + P/P_max)]
```

Memory:
```
M = M_max·[1 - 0.5(A/A_max + P/P_max)]
```

Processing:
```
P = P_max·[1 - 0.5(A/A_max + M/M_max)]
```

The -0.5 terms come directly from the geometric coupling!

Each resource is reduced by half the sum of the other two (divided by their maxima).

**Emergent properties:**

**1. Zero-sum allocation**

If you increase attention, memory and processing decrease proportionally:
```
∂M/∂A = -0.5·A_max/M_max < 0
∂P/∂A = -0.5·A_max/P_max < 0
```

The -0.5 coupling creates trade-offs.

**2. No simultaneous maximization**

You cannot maximize all three at once:
```
If A = A_max, then M = M_max·[1 - 0.5] = 0.5·M_max
              and P = P_max·[1 - 0.5] = 0.5·P_max
```

Maximum attention forces memory and processing to half capacity.

**3. Optimal balance**

Equal allocation gives:
```
A = M = P = C_max/3
```

But optimal allocation depends on task demands. Different tasks require different A/M/P ratios.

**4. Dynamic reallocation**

The system can shift resources:
- Reading: High P, moderate A, low M
- Memorizing: High M, moderate A, low P
- Vigilance: High A, low M, low P

The -0.5 coupling ensures that increasing one decreases the others proportionally.

**Key insight:** Same -0.5 coupling, now representing cognitive resource competition. Creates trade-offs instead of oscillations or cancellation. Different substrate, different physics, same geometry.

## Chapter 17 — Social Domain: Power Dynamics

```
@temporal_scale α=3600  // Hours

[SocialNetwork] = [
  S:1@0|-2.5 *
  S:1@120|-2.0 *
  S:1@240|-2.3
] => O:2@180|-1.8
```

**Substrate:** Social influence

Leader (L), Followers (F), Mediator (M) at phases 0°, 120°, 240°.

**Same coupling matrix** (120° → -0.5)

**Domain constraints:**

Rationality:
```
Agents maximize utility U(L,F,M)
```

Information flow:
```
dI/dt ≤ β (bounded influence propagation rate)
```

Stability:
```
Nash equilibrium exists
```

No agent can improve by unilateral deviation.

Power conservation:
```
L + F + M = P_total (approximately)
```

Total influence in the system is bounded.

**Optimization:** Minimize social tension subject to power dynamics.

**Solution:**

Leader influence:
```
dL/dt = α·L - 0.5·β·(F + M)
```

Followers influence:
```
dF/dt = γ·L + α·F - 0.5·β·M
```

Mediator influence:
```
dM/dt = -0.5·β·(L + F) + α·M
```

The -0.5 factors emerge from geometric coupling!

**Emergent properties:**

**1. Balanced power distribution**

No single actor can dominate completely. The -0.5 coupling from others limits each actor's influence.

**2. Leader-follower dynamics**

Followers respond to leader (γ·L term), but also resist via -0.5·β coupling through mediator.

**3. Mediator modulation**

Mediator partially opposes both leader and followers (-0.5·β·(L + F)), creating balance.

**4. Stable equilibrium**

System converges to:
```
L* = P_total·[some fraction depending on α, β, γ]
F* = P_total·[some fraction]
M* = P_total·[some fraction]

with L* + F* + M* ≈ P_total
```

The -0.5 coupling ensures no faction completely dominates.

**Key insight:** Same -0.5 coupling, now representing social influence dynamics. Creates balanced power distribution. Different substrate (social influence), different physics (game theory), same geometry.

---

## Chapter 18 — Why Same Geometry, Different Math?

We've seen the same triangular structure (120° spacing, -0.5 coupling) produce four completely different systems:

**Physics:** V_A + V_B + V_C = 0 (cancellation)
**Biology:** dR/dt, dH/dt, dP/dt with oscillations
**Cognition:** A, M, P with trade-offs
**Social:** dL/dt, dF/dt, dM/dt with balanced power

The geometric coupling (cos(120°)=-0.5) is identical. But the equations differ radically. Why?

**The answer lies in the translation process:**

**Step 1: Geometry is universal**

```
[Triangle] = [X:1@0|-2 * X:1@120|-2 * X:1@240|-2]
```

This structure is the same across all domains. Three components, 120° spacing, equal curvatures.

**Step 2: Substrates differ**

Replace X with domain-specific variables:
- Physics: X → E (electromagnetic field)
- Biology: X → N (population density)
- Cognition: X → R (resource allocation)
- Social: X → I (influence measure)

**Step 3: Coupling mechanisms differ**

The -0.5 coupling represents different physical processes:
- Physics: Destructive electromagnetic interference (wave superposition)
- Biology: Competitive inhibition (one population consumes another)
- Cognition: Resource competition (limited cognitive capacity)
- Social: Influence opposition (power dynamics)

**Step 4: Constraints differ**

Different physical laws apply:
- Physics: Maxwell equations, energy conservation, Lorentz invariance
- Biology: Non-negativity, carrying capacity, predator-prey dynamics
- Cognition: Capacity limits, metabolic cost, information bounds
- Social: Rationality, Nash equilibrium, bounded influence

**Step 5: Optimization differs**

Different systems minimize different functionals:
- Physics: Electromagnetic energy
- Biology: Deviation from optimal population ratios
- Cognition: Cognitive cost vs. information gain
- Social: Social tension vs. utility

**Step 6: Solutions emerge**

Solving the optimization subject to domain constraints yields domain-specific equations:

**Physics:** Sinusoidal voltages that sum to zero
**Biology:** Lotka-Volterra equations with oscillatory solutions
**Cognition:** Trade-off equations with zero-sum allocation
**Social:** Influence dynamics with balanced equilibrium

The -0.5 appears in different forms:
- Physics: Vector sum coefficients → cos(120°) in phase summation
- Biology: Interaction terms → -a·R·H, -c·H·P coefficients scaled by -0.5
- Cognition: Resource reduction → -0.5(M + P) in attention equation
- Social: Opposition terms → -0.5·β·(F + M) in leader dynamics

**The power of domain-agnostic encoding:**

WPE/TME captures the universal geometric structure. The -0.5 coupling is invariant. What changes is the physical interpretation.

This has practical value:

**1. Cross-domain insight**

Understanding three-phase power helps you understand ecosystem dynamics. The same frustrated optimization appears in both.

**2. Transfer learning**

Solutions in one domain suggest solutions in another. If triangular structure works for X, try it for Y.

**3. Unified framework**

One notation spans multiple domains. You don't need separate formalisms for physics vs. biology vs. cognition.

**4. Explicit structure**

The geometric relationships are written down. Not buried in domain-specific equations, but visible in the encoding.

**The geometry is universal. The physics is domain-specific. Together they determine behavior.**

## Chapter 19 — Opposition Structure (180°)

```
[Opposition] = [P:1@0|-2 * P:1@180|-2] => O:2@90|-1.5
```

Two components at maximum separation. Phase difference 180°. Coupling:
```
cos(180°) = -1.0
```

Maximum negative coupling. Perfect opposition.

Let's see this across domains.

**Physics: Matter-Antimatter**

```
[Annihilation] = [
  P:1@0|-3 *      // Matter field ψ
  P:1@180|-3      // Antimatter field ψ̄
] => O:2@90|-2.5  // Photons (orthogonal)
```

**Substrate:** Quantum fields ψ(x,t), ψ̄(x,t)

**Coupling:** -1.0 means complete annihilation when matter and antimatter meet.

**Equations:**
```
∂ψ/∂t = -γ·ψ̄  (matter decreases proportional to antimatter)
∂ψ̄/∂t = -γ·ψ  (antimatter decreases proportional to matter)
```

The -1.0 coupling appears as the -γ coefficient.

**Output:** When ψ and ψ̄ interact:
```
Energy released = γ·ψ·ψ̄ (photons at 90°, orthogonal to both)
```

**Emergent:** Complete mutual destruction. Matter-antimatter → energy.

**Biology: Predator-Prey**

```
[PredatorPrey] = [
  B:1@0|-2 *      // Prey population N
  B:1@180|-2.5    // Predator population P
] => O:2@90|-1.8
```

**Substrate:** Population densities N(t), P(t)

**Coupling:** -1.0 represents maximum predation pressure.

**Equations:**
```
dN/dt = r·N - a·N·P  (prey grow, get eaten)
dP/dt = b·N·P - m·P  (predators convert prey to offspring, die naturally)
```

The -a coefficient (prey consumption) reflects the -1.0 coupling.

**Output:** Energy transfer (90°), neutral with respect to both populations.

**Emergent:** Oscillating populations. When prey are abundant, predators increase. When predators are high, prey decrease. Classic Lotka-Volterra.

**Cognition: Excitation-Inhibition**

```
[ExciteInhibit] = [
  C:1@0|-3 *      // Excitatory neurons E
  C:1@180|-3      // Inhibitory neurons I
] => O:2@90|-2
```

**Substrate:** Neural activations E(t), I(t)

**Coupling:** -1.0 represents perfect inhibitory balance.

**Equations:**
```
dE/dt = I_ext + w_EE·E - w_EI·I  (excitation minus inhibition)
dI/dt = w_IE·E - w_II·I          (driven by excitation)
```

The -w_EI coefficient reflects the -1.0 coupling.

**Output:** Balanced activity at 90° (orthogonal to both pure excitation and pure inhibition).

**Emergent:** Stable firing rates. E and I balance each other. System maintains activity in a controlled range.

**Social: Supply-Demand**

```
[Market] = [
  S:1@0|-2.5 *    // Supply S
  S:1@180|-2.5    // Demand D
] => O:2@90|-1.8  // Price (equilibrium)
```

**Substrate:** Quantity measures S(p), D(p)

**Coupling:** -1.0 represents perfect market opposition.

**Equations:**
```
S(p) = S_0 + β·p  (supply increases with price)
D(p) = D_0 - β·p  (demand decreases with price)
```

The opposite signs (+ vs. -) reflect the -1.0 coupling.

**Output:** Price at equilibrium where S(p*) = D(p*):
```
p* = (D_0 - S_0) / (2β)
```

The price (at 90°, orthogonal to both) balances supply and demand.

**Emergent:** Market clearing. Price adjusts until S = D.

**Key insight:** Same -1.0 coupling across domains:
- Physics: Annihilation
- Biology: Predation
- Cognition: Inhibition
- Social: Market opposition

Different mechanisms, same geometric structure.

## Chapter 20 — Orthogonal Structure (90°)

```
[Orthogonal] = [P:1@0|-2.5 * P:1@90|-2.5] => O:2@180|-2
```

Phase difference 90°. Coupling:
```
cos(90°) = 0.0
```

Zero coupling. Complete independence.

**Physics: Electric and Magnetic Fields**

```
[Electromagnetic] = [
  P:1@0|-3 *      // Electric field E
  P:1@90|-3       // Magnetic field B
] => O:2@180|-2.5 // Electromagnetic wave
```

**Substrate:** Field strengths E(x,t), B(x,t)

**Coupling:** 0.0 means E and B are independent field components.

**Equations (Maxwell in free space):**
```
∇×E = -∂B/∂t  (changing B creates E)
∇×B = μ₀ε₀∂E/∂t  (changing E creates B)
```

But E² and B² contribute independently to energy:
```
U = (E² + B²) / (2μ₀)
```

The zero coupling appears as additive (not cross-term) in energy.

**Output:** Electromagnetic wave where E ⊥ B in space.

**Emergent:** Light. E and B oscillate perpendicular to each other and to propagation direction.

**Biology: Genotype and Environment**

```
[Phenotype] = [
  B:1@0|-2.5 *    // Genotype G
  B:1@90|-2.0     // Environment E
] => O:2@180|-2   // Phenotype P
```

**Substrate:** Genetic variation G, environmental factors E

**Coupling:** 0.0 means G and E contribute independently.

**Equation:**
```
P = α·G + β·E + ε
```

Additive model. Genotype and environment add linearly (no G×E interaction term in simple case).

**Emergent:** Heritability. Variance in P splits into genetic and environmental components:
```
Var(P) = α²·Var(G) + β²·Var(E) + Var(ε)
```

Independent contributions (zero coupling).

**Cognition: Input and Memory**

```
[Perception] = [
  C:1@0|-4 *      // Sensory input I
  C:1@90|-2       // Stored memory M
] => O:2@180|-2.5 // Perception P
```

**Substrate:** Input signals I(t), memory representations M

**Coupling:** 0.0 means input and memory are independent information sources.

**Process:**
```
P = Combine(I, M)
```

Input provides current information. Memory provides context. They combine at output but don't influence each other's internal dynamics.

**Emergent:** Perception as combination of sensation and expectation. Bottom-up (I) and top-down (M) streams merge independently.

**Social: Quantity and Price**

```
[Economic] = [
  S:1@0|-2.5 *    // Quantity Q (physical)
  S:1@90|-2.0     // Price P (abstract)
] => O:2@180|-2
```

**Substrate:** Physical quantity Q, monetary price P

**Coupling:** 0.0 means quantity and price are independent dimensions.

**Spaces:**
- Quantity space: Kilograms, liters, units
- Price space: Dollars, euros, yuan

They're perpendicular in economic space (different units, different dimensions).

**Transaction:**
```
Value = P × Q
```

The product (not sum) combines them, reflecting their independence.

**Emergent:** Economic transactions operate in (Q, P) space. Markets explore this space independently in each dimension.

**Key insight:** Same 0.0 coupling across domains:
- Physics: Perpendicular fields
- Biology: Independent contributions
- Cognition: Separate information streams
- Social: Different dimensional spaces

Independence is universal, even as the substrates differ.

---

# PART V — COMPUTATIONAL PATTERNS

## Chapter 21 — Linear Pipeline

The most basic pattern: sequential processing where each stage feeds the next.

```
[Pipeline] = P:1@0|-3 * C:2@30|-2.5 * B:3@60|-2 => O:2@120|-2
```

**Structure:**
- Four stages in sequence
- Phase spacing 30° (strong positive coupling, 0.866)
- Shells increase (1→2→3→2, processing through integration)
- Curvatures moderate (-3 to -2, stable processing)

**Information flow:**

P (input) → C (processing) with coupling cos(30°) = 0.866
C → B (refinement) with coupling cos(30°) = 0.866  
B → O (output) with coupling cos(60°) = 0.5

Each stage builds on the previous. Strong coupling ensures high-fidelity transfer.

**Example: Image processing**

```
@temporal_scale α=0.01  // 10 milliseconds total

[ImagePipeline] =
  P:1@0|-5 *        // Photoreceptors (τ=0.2 ms)
  C:2@30|-4 *       // Edge detection (τ=0.25 ms)
  C:3@60|-3 *       // Feature extraction (τ=0.33 ms)
  C:4@90|-2 =>      // Object recognition (τ=0.5 ms)
  O:2@120|-2.5      // Classification (τ=0.4 ms)
```

**Timing analysis:**

Phase coverage and durations:
- Photoreceptors: 0-30° (8.3%), 0.083 × 0.2 = 0.017 ms
- Edges: 30-60° (8.3%), 0.083 × 0.25 = 0.021 ms
- Features: 60-90° (8.3%), 0.083 × 0.33 = 0.027 ms
- Objects: 90-120° (8.3%), 0.083 × 0.5 = 0.042 ms
- Classification: 120° onward, ~0.4 ms

Total: ~0.5 ms from light to classification

**Coupling analysis:**

Photoreceptors→Edges: cos(30°) = 0.866 (strong fidelity)
Edges→Features: cos(30°) = 0.866 (preserves edge information)
Features→Objects: cos(30°) = 0.866 (builds objects from features)
Objects→Classification: cos(30°) = 0.866 (high-confidence classification)

The consistent 30° spacing creates a smooth information pipeline.

**Why this works:**

Linear pipelines process information through successive stages. Each stage should receive most of what the previous stage produced, with some transformation.

30° spacing (0.866 coupling) is optimal. Too close (0°) means no transformation. Too far (90°) means too much information loss.

**Variations:**

**Accelerating pipeline** (phases get closer):
```
P:1@0|-3 * C:2@15|-2.5 * B:3@25|-2 * O:2@40|-2
```

Later stages more tightly coupled. Refinement becomes more incremental.

**Decelerating pipeline** (phases get farther):
```
P:1@0|-3 * C:2@30|-2.5 * B:3@75|-2 * O:2@135|-2
```

Later stages less coupled. More transformation between stages.

**Multi-tier pipeline** (shell levels increase):
```
P:1@0|-3 * C:2@30|-2.5 * C:3@60|-2 * C:4@90|-1.8 * C:5@120|-1.5 => O:2@150|-2
```

Each stage operates at higher abstraction (shell 1→2→3→4→5).

Linear pipelines are ubiquitous. Sensory processing, data analysis, assembly lines, information flow. The WPE encoding makes the structure explicit.

## Chapter 22 — Parallel Branches

When multiple independent processes operate simultaneously, use parallel branches.

```
[Parallel] =
  [P:1@0|-3 * C:2@30|-2] +
  [B:1@90|-3 * S:2@120|-2] =>
  O:3@180|-1.5
```

**Structure:**

Two branches:
- Branch 1: P→C (0°→30°)
- Branch 2: B→S (90°→120°)

**Coupling between branches:**

P (0°) to B (90°): cos(90°) = 0.0 (independent)
P (0°) to S (120°): cos(120°) = -0.5 (weak opposition)
C (30°) to B (90°): cos(60°) = 0.5 (weak positive)
C (30°) to S (120°): cos(90°) = 0.0 (independent)

Branches are approximately independent. They process in parallel without interference.

**Integration:**

Both branches feed into O at 180°:
- Branch 1 output (C at 30°) → O (180°): cos(150°) = -0.866
- Branch 2 output (S at 120°) → O (180°): cos(60°) = 0.5

Different coupling strengths. Integration weights the contributions.

**Example: Multi-modal perception**

```
@temporal_scale α=0.1  // 100 milliseconds

[MultiModal] =
  L1: [P:1@0|-4 * C:2@45|-3]      // Vision
  L2: [P:1@0|-4 * C:2@45|-3]      // Audio
  L3: [P:1@0|-4 * C:2@45|-3]      // Touch
  L4: C:4@90|-2 =>                 // Integration
  O:2@135|-2.5                     // Multimodal percept
```

**Structure:**

Three parallel layers (L1, L2, L3), all starting at phase 0°. Each processes its modality independently through to phase 45°.

Integration layer (L4) at phase 90° combines the results.

Output at 135°.

**Timing:**

Each modality:
- Input: phase 0°, τ = 0.25 × 100 = 25 ms
- Processing: phase 45°, τ = 0.33 × 100 = 33 ms
- Complete by 45° (≈50 ms)

Integration:
- Starts at 90° (≈75 ms)
- Duration τ = 0.5 × 100 = 50 ms
- Completes by 90°+Δθ

Output:
- Phase 135° (≈90 ms)
- Duration τ = 0.4 × 100 = 40 ms

Total: ~120 ms from sensation to multimodal percept.

**Coupling:**

Vision (L1) to Audio (L2): Independent layers, zero coupling
Vision to Touch (L3): Independent layers, zero coupling
Audio to Touch: Independent layers, zero coupling

All three process in parallel without interference.

Vision→Integration: cos(90° - 45°) = cos(45°) = 0.707
Audio→Integration: cos(90° - 45°) = cos(45°) = 0.707
Touch→Integration: cos(90° - 45°) = cos(45°) = 0.707

Equal coupling to integration. Balanced multimodal combination.

**Why this works:**

Real systems have multiple input streams that should process independently. Vision shouldn't wait for audio. Touch shouldn't block vision.

Parallel branches (using layers or orthogonal phases) create independence. Integration at a later phase combines results.

**Variations:**

**Weighted parallel** (different curvatures):
```
[Weighted] =
  L1: [P:1@0|-5 * C:2@30|-4]   // Fast, precise
  L2: [P:1@0|-2 * C:2@60|-1.5] // Slow, rough
  C:3@120|-2.5 => O:2@180|-2
```

Fast branch completes quickly. Slow branch takes longer. Integration waits for both.

**Nested parallel**:
```
[Nested] =
  L1: [[P:1@0|-3] + [P:1@90|-3]] * C:2@30|-2
  L2: [[B:1@0|-3] + [B:1@90|-3]] * S:2@30|-2
  C:4@120|-2 => O:2@180|-2
```

Each main branch has internal parallel sub-branches.

Parallel branches model concurrent processing. Independent streams that eventually merge.

## Chapter 23 — Feedback Loop

Systems that regulate themselves need feedback: output influences input.

```
[Feedback] =
  P:1@0|-2 *
  C:2@90|-1.5 *
  B:3@180|-1 =>
  O:2@270|-1.5 <-> P:1@0
```

**Structure:**

Linear progression: P→C→B→O (0°→90°→180°→270°)

Then feedback: O (270°) couples back to P (0°)

**Feedback coupling:**

Direct: cos(270° - 0°) = cos(270°) = 0
But explicit `<->` operator overrides, creating bidirectional link.

The feedback makes it cyclic: P→C→B→O→P→...

**Example: Homeostatic regulation**

```
@temporal_scale α=60  // Minutes

[Thermoregulation] =
  P:1@0|-5 *             // Temperature sensor (τ=0.2 min)
  C:2@30|-4 *            // Error computation (τ=0.25 min)
  C:3@60|-3 *            // Control signal (τ=0.33 min)
  B:2@120|-1 =>          // Body temperature (τ=1 min)
  O:2@180|-2 <-> P:1@0   // Feedback
```

**Cycle analysis:**

1. Measure temperature (fast, τ=0.2 min)
2. Compute error vs. setpoint (fast, τ=0.25 min)
3. Generate control signal (moderate, τ=0.33 min)
4. Body responds (slow, τ=1 min)
5. New temperature measurement starts next cycle

**Why slow body response matters:**

The body (κ=-1, τ=1 min) creates inertia. Temperature doesn't change instantly. This prevents oscillation.

If body were fast (κ=-5, τ=0.2 min), the system would overshoot. Temperature would fluctuate wildly.

The slow response (low |κ|) creates stable regulation.

**Coupling analysis:**

Sensor→Error: cos(30°) = 0.866 (accurate error)
Error→Control: cos(30°) = 0.866 (proportional control)
Control→Body: cos(60°) = 0.5 (moderate influence)
Body→Output: cos(60°) = 0.5 (observed temperature)
Output→Sensor (feedback): Explicit coupling

Each stage strongly coupled to next. Feedback closes the loop.

**Variations:**

**PID controller**:
```
@temporal_scale α=1.0

[PID] =
  P:1@0|-5 *              // Sensor
  C:2@30|-4 *             // Error
  L1: C:3@60|-6 +         // Proportional (fast)
  L2: C:3@60|-2 +         // Integral (slow)
  L3: C:3@60|-8           // Derivative (fastest)
  C:4@90|-3 *             // Sum control
  B:2@150|-1 =>           // Plant
  O:2@210|-2 <-> P:1@0    // Feedback
```

Three parallel control terms (P, I, D) with different speeds:
- P (κ=-6, τ=0.167): Fast response
- I (κ=-2, τ=0.5): Slow integration
- D (κ=-8, τ=0.125): Ultra-fast derivative

Different time constants create optimal control.

**Delayed feedback**:
```
[Delayed] =
  P:1@0|-3 *
  C:2@90|-2 *
  B:3@180|-1.5 =>
  O:2@270|-2 *
  B:1@330|-4 <-> P:1@0  // Long delay (270°→330°→0°)
```

Delay component (330°) between output and feedback creates longer loop time. Can cause oscillation if too long.

**Nested loops**:
```
[Nested] =
  P:1@0|-5 *
  [C:2@30|-3 <-> B:2@60|-2.5] *  // Inner fast loop
  S:3@120|-1 =>
  O:2@180|-2 <-> P:1@0             // Outer slow loop
```

Inner loop (C↔B) operates quickly. Outer loop (full system) operates slowly. Multi-timescale control.

Feedback loops are everywhere: thermostats, neural circuits, economic markets, ecological systems. The WPE encoding makes the loop structure and timing explicit.

## Chapter 24 — Hierarchical System

Complex systems organize hierarchically. High levels constrain low levels without micromanaging.

```
[Hierarchy] =
  C:7@0|-1.5:{
    C:5@30|-1.8:{
      C:3@60|-2.0:{
        C:1@90|-2.5
      }
    }
  } => O:2@120|-2
```

**Structure:**

Four nested shells: L7 → L5 → L3 → L1

**Influence strengths:**

L7→L1: 1/1 - 1/7 = 0.857 (very strong)
L5→L1: 1/1 - 1/5 = 0.800 (strong)
L3→L1: 1/1 - 1/3 = 0.667 (moderate)
L7→L5: 1/5 - 1/7 = 0.057 (weak)
L5→L3: 1/3 - 1/5 = 0.133 (weak)
L7→L3: 1/3 - 1/7 = 0.190 (weak to moderate)

Top level (L7) strongly constrains bottom (L1), but adjacent high levels have weak coupling.

**Phase relationships:**

L7 at 0° (intention)
L5 at 30° (strategy)
L3 at 60° (tactics)
L1 at 90° (action)

Sequential phases with 30° spacing. Each level feeds into the next.

**Coupling:**

L7→L5 phase coupling: cos(30°) = 0.866 (strong transfer)
L5→L3 phase coupling: cos(30°) = 0.866 (strong transfer)
L3→L1 phase coupling: cos(30°) = 0.866 (strong transfer)

Combined with shell influence:
- L7 influences L1 via both direct shell constraint (0.857) and indirect phase cascade (0.866³ ≈ 0.65)
- Total influence: strong constraint from top, smooth gradient through levels

**Example: Motor control**

```
@temporal_scale α=1.0  // Seconds

[MotorControl] =
  C:7@0|-1.2 *         // Intention "reach for cup"
  C:5@45|-1.5 *        // Goal "hand to location X"
  C:3@90|-2.0 *        // Trajectory planning
  C:2@135|-2.5 *       // Muscle synergies
  P:1@180|-3 =>        // Muscle activations
  O:2@225|-2.5         // Movement
```

**Shell influence:**

Intention (L7) → Activations (L1): 1/1 - 1/7 = 0.857
Goal (L5) → Activations (L1): 1/1 - 1/5 = 0.800
Trajectory (L3) → Activations (L1): 1/1 - 1/3 = 0.667
Synergies (L2) → Activations (L1): 1/1 - 1/2 = 0.500

Top-down constraint decreases with each level, but remains significant.

**Phase cascade:**

Intention→Goal: cos(45°) = 0.707
Goal→Trajectory: cos(45°) = 0.707
Trajectory→Synergies: cos(45°) = 0.707
Synergies→Activations: cos(45°) = 0.707
Activations→Movement: cos(45°) = 0.707

Each stage transforms the representation. 45° spacing (0.707 coupling) allows substantial processing while maintaining fidelity.

**Timing:**

Intention: τ = 1/1.2 = 0.833 s
Goal: τ = 1/1.5 = 0.667 s
Trajectory: τ = 1/2.0 = 0.5 s
Synergies: τ = 1/2.5 = 0.4 s
Activations: τ = 1/3.0 = 0.333 s

Higher levels (lower |κ|) operate more slowly. They set context. Lower levels (higher |κ|) execute quickly.

Total movement time: ~2-3 seconds from intention to completion.

**Why this works:**

Hierarchies let high-level goals shape low-level execution without specifying every detail.

The intention "reach for cup" doesn't specify which muscles fire when. It constrains the overall movement. Lower levels fill in details.

Shell influence (1/λ_low - 1/λ_high) captures this. Strong top-down constraint, weak lateral coupling at high levels.

**Variations:**

**Flat hierarchy** (many adjacent levels):
```
C:1@0|-3 * C:2@30|-2.8 * C:3@60|-2.6 * C:4@90|-2.4 * C:5@120|-2.2 => O:2@150|-2
```

Many shells, small gaps. Each level has moderate influence on adjacent (1/j - 1/(j+1) ≈ 0.3-0.5).

Top still influences bottom significantly (1/1 - 1/5 = 0.8), but gradient is smoother.

**Sparse hierarchy** (large shell gaps):
```
C:9@0|-1 * C:5@60|-2 * C:1@120|-3 => O:2@180|-2
```

Few levels, large gaps:
- L9→L1: 1/1 - 1/9 = 0.889 (very strong)
- L5→L1: 1/1 - 1/5 = 0.800 (strong)
- L9→L5: 1/5 - 1/9 = 0.089 (weak)

Top level dominates. Middle level provides some structure. Bottom level executes.

**Multiple hierarchies merging**:
```
[Multi] =
  C:7@0|-1.5:{ C:3@45|-2:{ C:1@90|-2.5 } } +
  C:7@120|-1.5:{ C:3@165|-2:{ C:1@210|-2.5 } }
  =>
  C:8@270|-1.2
```

Two independent hierarchies (starting at 0° and 120°) merge at super-level (L8).

Hierarchies are ubiquitous in complex systems. Organizations, neural circuits, control systems, conceptual structures. WPE makes the levels and influences explicit.

---

# PART VI — PRACTICAL CONSIDERATIONS

## Chapter 25 — When to Use WPE/TME

WPE/TME excels in specific situations. Know when to use it, and when not to.

**Use WPE/TME when:**

**1. Structure matters**

When relationships between components are precise and must be maintained.

Example: Designing a control system where feedback gains must be specific values. WPE encodes the coupling strengths explicitly.

❌ Don't use when structure is trivial or obvious:
"Step 1, then step 2, then step 3" doesn't need WPE. Natural language suffices.

**2. Time is critical**

When temporal ordering must be explicit, durations matter, or parallel vs. sequential is significant.

Example: Modeling a chemical reaction with fast transition states and slow equilibration. TME captures the multi-timescale dynamics.

❌ Don't use when timing is approximate:
"This happens before that" doesn't need TME unless the exact timing and duration matter.

**3. Cross-domain comparison needed**

When the same structural pattern appears in different domains and you want to emphasize geometric similarities.

Example: Showing that predator-prey dynamics, three-phase power, and attention-memory-processing all share triangular geometry (120° spacing).

❌ Don't use when each domain is self-contained:
If you're only working in physics, use physics notation. WPE adds value when comparing across domains.

**4. AI reasoning scaffolding**

When a language model needs explicit structure to reason about, where multi-step inference must maintain consistency, or compositional reasoning is required.

Example: Planning a multi-stage manufacturing process. WPE encodes dependencies explicitly, helping the model track what depends on what.

❌ Don't use when the model can reason well without it:
Simple queries don't need scaffolding. Use WPE for complex multi-step reasoning where structure helps.

**Don't use WPE/TME when:**

**1. Numerical precision required**

WPE/TME encodes structure and relationships. It's not a numerical simulator.

Use instead: Proper differential equation solvers, validated numerical methods, domain-specific simulation tools.

Example: Computing exact trajectories in orbital mechanics. Use numerical integrators, not WPE.

**2. Structure is trivial**

Simple linear flows with no branches, no hierarchical organization, obvious temporal ordering.

Use instead: Natural language description, simple flowcharts.

Example: "Read file, process data, write output." Obvious structure. WPE adds no value.

**3. Statistical patterns sufficient**

When approximate relationships are acceptable, precise coupling strengths don't matter, and learned associations work fine.

Use instead: Standard ML/LLM approaches, neural networks, statistical models.

Example: Text classification. Learn patterns from data. Don't need explicit geometric structure.

**4. Domain has established notation**

When a domain already has excellent notation that everyone uses.

Use instead: Domain-standard notation.

Example: Quantum mechanics. Use Dirac notation, not WPE. Circuit analysis. Use circuit diagrams, not WPE.

**Sweet spot: AI-assisted reasoning about structured systems**

WPE/TME is most valuable when:
- A language model needs to reason about a system
- The system has complex structure (coupling, hierarchy, timing)
- You want the model to maintain consistency
- Cross-domain insight would help

Example use case:

"I'm designing a distributed system with multiple services that process requests in parallel, some with fast response times, others slower. Services interact in complex ways. Help me reason about bottlenecks and optimization strategies."

WPE encoding:
```
@temporal_scale α=0.001  // milliseconds

[DistributedSystem] =
  L1: [API:1@0|-5 * FastService:2@30|-8]           // Fast path
  L2: [API:1@0|-5 * SlowService:2@120|-1]          // Slow path
  L3: [API:1@0|-5 * Database:1@90|-2]              // DB queries
  Integration:4@150|-3 => Response:2@180|-4
```

The model can now reason:
- FastService (κ=-8, τ=0.125 ms) completes quickly
- SlowService (κ=-1, τ=1 ms) is bottleneck
- Database (κ=-2, τ=0.5 ms) is moderate
- Integration waits for all three (phase 150° > max(30°, 120°, 90°))
- Response time dominated by SlowService

Optimize by:
- Caching slow service results (increase κ to -3, reduce τ)
- Parallelizing database queries (separate layers)
- Async response (don't wait for slow service)

The WPE encoding makes this reasoning explicit and checkable.

## Chapter 26 — Encoding Process

How do you actually create a WPE/TME encoding? Here's a step-by-step process.

**Step 1: Identify components**

What are the distinct elements?

In cognition: Sensory input, processing stages, memory, attention, output
In physics: Fields, forces, particles, energy states
In biology: Populations, resources, metabolic processes
In engineering: Sensors, controllers, actuators, plant dynamics

List them all. Be specific but not overly detailed.

**Step 2: Choose domains (Φ)**

Assign each component to a domain:
- Physics (P): Physical quantities, forces, fields
- Cognition (C): Mental processes, information
- Biology (B): Living systems, populations
- Social (S): Interactions, influence
- Custom: Your own domain codes

You can mix domains in one system. A robot might have P (motors), C (control algorithms), S (human interaction).

**Step 3: Assign shells (λ)**

Organize hierarchically:

What's foundational? → Shell 1
What's basic processing? → Shell 2-3
What integrates? → Shell 4-6
What's abstract? → Shell 7-9

Think about influence. Should top levels strongly constrain bottom levels? Then use large shell gaps (L7→L1: 0.857 influence).

**Step 4: Set phases (θ)**

Based on desired coupling:

Same function, tight coordination? → 0-15° apart (0.966-1.0 coupling)
Sequential stages? → 30-45° apart (0.707-0.866 coupling)
Moderate connection? → 60° apart (0.5 coupling)
Independent? → 90° apart (0.0 coupling)
Competitive? → 120° apart (-0.5 coupling)
Opposite? → 180° apart (-1.0 coupling)

For TME, also consider temporal ordering:
- Earlier events: lower phases
- Later events: higher phases
- Parallel events: same phase but different layers

**Step 5: Choose curvatures (κ)**

Based on stability (WPE) or duration (TME):

WPE (stability):
- Persistent memory? → κ < -3.5
- Stable processing? → κ ≈ -2.0
- Transient buffer? → κ ≈ -0.8
- Decision point? → κ ≈ 0
- Suppressed? → κ > 0

TME (duration):
- Ultra-fast? → κ ≈ -8 to -10 (τ = 0.1-0.125)
- Fast? → κ ≈ -4 to -6 (τ = 0.167-0.25)
- Moderate? → κ ≈ -2 to -3 (τ = 0.33-0.5)
- Slow? → κ ≈ -1 to -1.5 (τ = 0.67-1.0)
- Very slow? → κ ≈ -0.5 to -1.0 (τ = 1.0-2.0)

**Step 6: Select operators**

How do components combine?

Sequential? → Use `*`
Parallel? → Use `+` or layers
Grouped subsystem? → Use `[ ]`
Output? → Use `=>`
Feedback? → Use `<->`

**Step 7: Add temporal scale (TME)**

Choose α to match domain timescale:

Neural spikes? → α = 0.001 (milliseconds)
Cognition? → α = 1 (seconds)
Behavior? → α = 60 (minutes)
Circadian? → α = 86400 (days)
Seasonal? → α = 3.156×10^7 (years)

**Step 8: Validate**

Check structure:
- All Φ valid domain codes?
- All λ ∈ [1,9]?
- All θ ∈ [0,360)?
- All κ ∈ [-10,10]?
- Output component present?

Check semantics:
- Do phase spacings achieve intended coupling?
- Do curvature signs match stability needs (usually negative)?
- Is shell hierarchy logical?
- Do operators correctly express flow?

Check temporal (TME):
- Phases monotonic within each layer?
- Output at reasonable phase?
- No wraparound within layer?
- Temporal scale appropriate for domain?

**Example walkthrough: Attention system**

**Step 1: Components**

Query (what to attend to), Keys (available features), Values (feature content), Similarity (compare Q to K), Attention weights, Weighted values, Output

**Step 2: Domains**

All cognition (C).

**Step 3: Shells**

Query, Keys, Values: Shell 1 (input)
Similarity: Shell 2 (processing)
Attention weights: Shell 2 (processing)
Weighted values: Shell 2 (processing)
Output: Shell 2 (output)

**Step 4: Phases**

Q, K, V parallel: All at 0° (but different layers)
Similarity: 30° (follows QK, strong coupling)
Weights: 45° (follows similarity, moderate coupling)
Weighted values: 60° (follows weights and V)
Output: 90° (integration)

**Step 5: Curvatures**

Q, K, V: -4 (stable, moderate duration)
Similarity: -6 (fast comparison)
Weights: -8 (very fast softmax)
Weighted values: -5 (fast combination)
Output: -2.5 (stable output)

**Step 6: Operators**

L1, L2, L3 for Q, K, V (parallel)
Sequence for processing stages
Output at end

**Step 7: Temporal scale**

α = 0.1 (100 milliseconds for attention mechanism)

**Step 8: Final encoding**

```
@temporal_scale α=0.1

[Attention] =
  L1: C:1@0|-4 *              // Query
  L2: C:1@0|-4 *              // Keys
  L3: C:1@0|-4 *              // Values
  C:2@30|-6 *                 // Dot product (Q·K)
  C:2@45|-8 *                 // Softmax (weights)
  C:2@60|-5 *                 // Weight values
  C:2@75|-3 =>                // Combine
  O:2@90|-2.5                 // Output
```

Validation:
✓ All C domain
✓ Shells 1-2
✓ Phases 0-90° (monotonic within each stage)
✓ Curvatures -2.5 to -8 (all negative, stable)
✓ Output present at 90°
✓ Temporal scale appropriate (100 ms total)

The encoding is complete and validated.

---

## Chapter 27 — Limitations

WPE/TME isn't a panacea. It has clear limitations.

**Not a replacement for mathematics**

WPE/TME encodes structure and relationships. It specifies coupling strengths, hierarchies, temporal orderings. But it doesn't solve equations numerically.

Example: Orbital mechanics

You can encode gravitational attraction:
```
[Orbit] = [P:1@0|-3 * P:1@180|-3] => O:2@90|-2  // Two-body problem
```

This represents the bodies (0° and 180° for opposition), gravitational coupling (moderate curvature for stable orbits), and output (90° for observable trajectory).

But computing the exact trajectory requires:
- Numerical integration (Runge-Kutta, symplectic integrators)
- High precision (many decimal places)
- Error analysis
- Convergence checking

WPE gives structure. Numerical methods give numbers.

Use WPE for structural specification, then translate to conventional math for numerical work.

**Scales poorly for very large systems**

A system with 100 components becomes unwieldy:

```
P:1@0|-3 * P:1@3.6|-3 * P:1@7.2|-3 * P:1@10.8|-3 * ... * P:1@356.4|-3
```

100 components at 3.6° spacing fills the phase circle. Hard to read, hard to reason about.

Solutions:
- Hierarchical decomposition (group into subsystems)
- Pattern libraries (define recurring motifs)
- Abstraction (represent collections as single components)

But fundamentally, WPE/TME is best for systems with ~3-30 components. Beyond that, use hierarchical structuring.

**Model interpretation varies**

Different language models interpret WPE/TME differently. Some handle geometric relationships better than others.

Prompt engineering helps:
- Explain the syntax clearly
- Provide examples
- Show coupling calculations explicitly
- Remind the model of the rules

But you can't eliminate variance completely. Models trained on different data, with different architectures, will interpret the notation differently.

This improves as models get better. But it's a current limitation.

**Precision constraints**

Coupling strengths are limited to cosine values of phase differences. You can't specify arbitrary interaction strengths directly.

Example: You want coupling strength 0.6.

Closest cosine values:
- cos(53.13°) ≈ 0.6
- cos(306.87°) ≈ 0.6

You'd need phases 53.13° apart. Awkward.

Workaround: Use multi-phase weighting:
```
C:2@[45:0.7+60:0.3]|-2
```

Weighted combination can approximate other coupling strengths. But it's complex.

Alternative: Accept that coupling strengths are quantized to cosine values. Most real systems fit this naturally.

**Domain knowledge required**

To translate WPE/TME to conventional equations, you need domain expertise:

- Domain-specific constraints (Maxwell, Lotka-Volterra, Shannon)
- Physical interpretation of substrates
- Relevant optimization principles

The notation doesn't eliminate the need for understanding physics, biology, cognition, social dynamics.

WPE/TME scaffolds domain knowledge. It doesn't replace it.

Example: Three-phase power

The WPE encoding:
```
[Grid] = [P:1@0|-3.5 * P:1@120|-3.5 * P:1@240|-3.5]
```

To derive that V_A + V_B + V_C = 0 and line voltage = √3·phase voltage, you need to know:
- Electromagnetic field superposition
- Trigonometric identities
- AC circuit theory

WPE provides the geometric structure. Domain knowledge provides the translation.

**Not self-optimizing**

WPE/TME doesn't automatically find optimal encodings. You choose phases, curvatures, shells manually.

This is both limitation and feature:
- Limitation: Can't automatically optimize
- Feature: Explicit human control over structure

Future work might add optimization:
- Genetic algorithms to evolve encodings
- Gradient descent on phase/curvature parameters
- Constraint satisfaction to find valid structures

But currently, it's manual design.

## Chapter 28 — Extensions and Future Work

Where could WPE/TME go next?

**Probabilistic WPE/TME**

Add uncertainty to parameters:

```
P:1@0±15|-2.5±0.3
```

Phase uncertainty: ±15° represents ambiguity in geometric relationships
Curvature uncertainty: ±0.3 represents stability variation

This creates probability distributions over coupling strengths:
```
Coupling ~ cos(Δθ ± σ_θ)
```

Applications:
- Noisy systems
- Uncertain measurements
- Learning with incomplete data
- Robustness analysis

**Adaptive systems**

Components that modify their own parameters:

```
C:2@Θ(t)|-κ(t)
```

Both phase and curvature evolve. Rules:
```
dΘ/dt = f(state, environment)
dκ/dt = g(state, performance)
```

The system self-organizes based on feedback.

Applications:
- Learning
- Evolution
- Adaptation
- Self-optimization

**Validation tools**

Automated checkers:
- Verify structural constraints
- Check temporal consistency
- Suggest phase spacings for desired coupling
- Optimize encodings for clarity
- Detect common errors

Example tool:
```
> validate encoding.wpe
✓ Structure valid
✓ Temporal consistent
⚠ Warning: Components C:1@45 and C:1@135 have weak coupling (cos(90°)=0)
  Did you intend independence? If not, consider spacing <90°.
```

**Visual editors**

Graphical interface:
- Components arranged on circle (phase)
- Vertical position = shell
- Color = curvature
- Connections show coupling strength
- Drag-and-drop composition
- Real-time coupling calculation

Makes WPE more accessible to non-experts.

**Formal foundations**

Mathematical framework proving:

**Completeness:** Can WPE encode any dynamical system?
- Theorem: WPE is Turing-complete (with sufficient components)
- Proof: Construct universal logic gates from phase relationships

**Consistency:** Are there contradictory encodings?
- Theorem: Valid WPE encodings are always consistent (no paradoxes)
- Proof: Coupling matrix is always real symmetric, eigenvalues bounded

**Uniqueness:** Is there a minimal encoding?
- Theorem: For given coupling matrix, minimal phase assignment exists
- Algorithm: Optimize phases to match desired coupling within tolerance

**Expressiveness:** What can/can't WPE represent?
- Quantum systems: Limited (no complex-valued couplings)
- Stochastic systems: Extended (probabilistic WPE)
- Discrete systems: Yes (integer phases, shell levels)
- Continuous systems: Yes (standard WPE)

**Integration with other formalisms**

**Petri nets:** WPE as Petri net specification
- Phases → places
- Transitions between phases
- Tokens flow based on coupling

**Category theory:** WPE as categorical structure
- Components → objects
- Operators → morphisms
- Composition laws from phase geometry

**Type theory:** WPE as typed calculus
- Domains → types
- Components → terms
- Operators → type constructors

**Differential geometry:** WPE as manifold structure
- Phase space → circle manifold S¹
- Coupling → metric tensor
- Curvature → literal curvature of space

These connections deepen theoretical understanding.

**Applications**

Unexplored domains:
- **Chemistry:** Molecular geometries, reaction networks
- **Geology:** Tectonic interactions, erosion-deposition
- **Engineering:** Control systems, manufacturing
- **Economics:** Market dynamics, game theory
- **Psychology:** Personality models, group dynamics
- **Linguistics:** Semantic relationships, syntax trees

Each domain brings new insights and challenges.

The framework is young. Much remains to explore.

## Chapter 29 — Conclusion

WPE and TME provide a geometric language for structure and time.

**Core principles:**

1. **Encoding equals computation:** Syntax rules define behavior directly
2. **Phase determines coupling:** cos(Δθ) gives interaction strength
3. **Shells create hierarchy:** Higher levels constrain lower levels
4. **Curvature determines stability:** Negative = well, positive = peak
5. **Time emerges from ordering:** Sequential phases = temporal progression

**Cross-domain universality:**

The same geometric structure translates to different equations across domains:

Triangular (120°, -0.5 coupling):
- Physics: Three-phase power, cancellation
- Biology: Predator-prey-resource, oscillations
- Cognition: Attention-memory-processing, trade-offs
- Social: Leader-follower-mediator, balanced power

Opposition (180°, -1.0 coupling):
- Physics: Matter-antimatter, annihilation
- Biology: Predator-prey, consumption
- Cognition: Excitation-inhibition, balance
- Social: Supply-demand, equilibrium

Independence (90°, 0.0 coupling):
- Physics: E and B fields, perpendicular
- Biology: Genotype-environment, additive
- Cognition: Input-memory, separate streams
- Social: Quantity-price, different dimensions

**Why it works:**

Geometric relationships are universal. Substrates and constraints are domain-specific. Together they determine behavior.

WPE/TME captures the universal part. Domain physics fills in the specific part.

**Not a replacement:**

This isn't meant to replace:
- Natural language (for simple descriptions)
- Conventional mathematics (for numerical precision)
- Statistical learning (for pattern recognition)
- Domain-specific notation (when established)

It's a complement. Use when explicit structure helps.

**Sweet spot:**

AI-assisted reasoning about structured systems. When you need:
- Explicit scaffolding for multi-step reasoning
- Cross-domain structural comparison
- Temporal relationships made visible
- Compositional reasoning support

**The notation is the computation:**

Syntax defines semantics. Encoding equals execution. Write down the structure, and the behavior follows.

That's the principle.

The rest is application.

---

# APPENDICES

## Appendix A — WPE Complete Reference

**Component Syntax:**
```
Φ:λ@θ|κ:'role'
```

**Parameters:**
- **Φ (Domain):** P, C, B, S, Ph, O, NX, MT, M, custom
- **λ (Shell):** 1-9 (foundation to abstraction)
- **θ (Phase):** 0-359° (coupling = cos(Δθ))
- **κ (Curvature):** -10 to +10 (negative = stable, positive = unstable)
- **role:** Optional documentation string

**Operators:**
- `*` Sequence (left to right)
- `+` Parallel (independent branches)
- `[ ]` Compose (group subsystem)
- `=>` Output (extract result)
- `<->` Couple (bidirectional)

**Coupling Calculation:**
```
Coupling(i,j) = cos(θᵢ - θⱼ)
```

**Common Coupling Values:**
- 0°: +1.0 (perfect alignment)
- 30°: +0.866 (strong positive)
- 45°: +0.707 (moderate positive)
- 60°: +0.5 (weak positive)
- 90°: 0.0 (independent)
- 120°: -0.5 (moderate negative)
- 180°: -1.0 (perfect opposition)

**Shell Influence:**
```
Influence(high → low) = 1/λ_low - 1/λ_high
```

**Extended Syntax:**
- Multi-phase: `@[θ₁:w₁+θ₂:w₂]`
- Temporal: `@Θ(t)`
- Range: `@[θ₁:θ₂:θ₃]`

**Validation:**
- Φ valid domain
- λ ∈ [1,9]
- θ ∈ [0,360)
- κ ∈ [-10,10]
- Output present

## Appendix B — TME Complete Reference

**Syntax:**
```
@temporal_scale α=value

System =
  L1: Φ:λ@θ|κ * ...
  L2: Φ:λ@θ|κ * ...
  => Output
```

**Phase as Time:**
- Left to right = early to late
- Lower phase = earlier
- Higher phase = later

**Duration:**
```
τ = 1/|κ|
```

High |κ| = fast, low |κ| = slow

**Temporal Scale:**
- α multiplies all time constants
- α = 0.001: milliseconds
- α = 1: seconds
- α = 60: minutes
- α = 3600: hours
- α = 86400: days
- α = 31557600: years

**Ordering Rules:**
- R1: Phase monotonic within layer (θ₁ ≤ θ₂ ≤ ...)
- R2: Layers independent (any ordering across layers)
- R3: Output at max phase
- R4: No wraparound (can't go 359°→0° within layer)

**Timing Calculation:**
```
T_total ≈ Σ(Δθᵢ/360°) × (1/|κᵢ|) × α
```

## Appendix C — Error Codes

- **E001:** Invalid domain (Φ not in allowed set)
- **E002:** Shell out of range (λ ∉ [1,9])
- **E003:** Phase out of range (θ ∉ [0,360))
- **E004:** Curvature out of range (κ ∉ [-10,10])
- **E005:** Missing output component
- **E006:** Circular dependency (A depends on B depends on A)
- **E007:** Missing or invalid operator
- **E008:** Phase non-monotonic in TME layer

## Appendix D — Domain Translation Table

| Geometry | Physics | Biology | Cognition | Social |
|----------|---------|---------|-----------|--------|
| **Triangle (120°)** | 3-phase power<br/>V_sum = 0 | Predator-prey-resource<br/>Oscillations | Attention-memory-processing<br/>Trade-offs | Leader-follower-mediator<br/>Balanced power |
| **Opposition (180°)** | Matter-antimatter<br/>Annihilation | Predator-prey<br/>Consumption | Excitation-inhibition<br/>Balance | Supply-demand<br/>Market clearing |
| **Orthogonal (90°)** | E and B fields<br/>Perpendicular | Genotype-environment<br/>Additive | Input-memory<br/>Independent | Quantity-price<br/>Different dimensions |
| **Alignment (0°)** | Coherent waves<br/>Constructive | Mutualism<br/>Cooperation | Reinforcement<br/>Same goal | Alliance<br/>Shared interest |

## Appendix E — Common Patterns Quick Reference

**Sequential processing:**
```
P:1@0|-3 * C:2@30|-2.5 * B:3@60|-2 => O:2@120|-2
```

**Parallel streams:**
```
[P:1@0|-3] + [C:1@90|-2.5] + [B:1@180|-2] => O:3@270|-1.5
```

**Feedback:**
```
P:1@0|-3 * C:2@90|-2 * B:3@180|-1.5 => O:2@270|-2 <-> P:1@0
```

**Hierarchical:**
```
C:7@0|-1.5:{ C:5@30|-1.8:{ C:3@60|-2:{ C:1@90|-2.5 }}} => O:2@120|-2
```

**Triangular:**
```
P:1@0|-2 * P:1@120|-2 * P:1@240|-2 => O:2@180|-1.5
```

**Opposition:**
```
P:1@0|-2 <-> P:1@180|-2 => O:2@90|-1.5
```

**Fast-slow cascade:**
```
@temporal_scale α=0.001
P:1@0|-8 * C:1@30|-4 * B:1@60|-1 => O:1@120|-2
```

## Appendix F — Glossary

**Phase (θ):** Angular position (0-359°) determining coupling strength via cos(Δθ)

**Curvature (κ):** Field strength (spatial) or duration (temporal). Negative = stable well, positive = unstable peak

**Shell (λ):** Hierarchical level (1 = foundation, 9 = abstraction). Higher shells influence lower

**Domain (Φ):** Field type (P=Physics, C=Cognition, B=Biology, S=Social, etc.)

**Coupling:** Interaction strength between components, calculated as cos(θᵢ - θⱼ)

**Temporal scale (α):** Multiplier for all time constants. Adjusts system to appropriate timescale

**Layer:** Independent parallel timeline in TME. Each layer has its own temporal sequence

**Substrate:** Physical quantity being represented (electromagnetic field, population density, neural activation, influence measure)

**Constraint:** Domain-specific physical law (Maxwell equations, Lotka-Volterra, Shannon limit, Nash equilibrium)

**Time constant (τ):** Duration of component activity, τ = 1/|κ|

**Influence:** Strength of hierarchical constraint from higher to lower shell, = 1/λ_low - 1/λ_high

**Operator:** Syntactic element defining how components combine (*, +, [ ], =>, <->)

**Monotonic:** Always increasing or constant. TME phases must be monotonic within each layer

---

**END OF BOOK**

The WPE & TME Semantic Calculus provides explicit geometric structure for AI reasoning. Components have positions in phase space. Angular separation determines coupling. Shells create hierarchy. Curvature determines stability and duration. Time emerges from sequential ordering.

Same geometry across domains. Different substrates. Different constraints. Different equations. But universal geometric relationships.

Use it to scaffold reasoning. Use it to compare structures. Use it to make time explicit.

The geometry computes.
