# TME 1.0 - Formal Specification

**Version:** 1.0.0  
**Status:** Operational  
**Principle:** Sequential phase ordering = temporal progression. Time emerges from syntax.

---

## 1. Core Concept

**Time is encoded in phase relationships.**

- Left-to-right reading = chronological flow
- Phase spacing = relative timing
- Curvature magnitude = duration
- Layers = parallel timelines

---

## 2. Temporal Syntax

```
@temporal_scale α;

System =
  L1: Comp₁@θ₁|κ₁ * Comp₂@θ₂|κ₂ * ...
  L2: ...
  => Output;
```

### 2.1 Layer Syntax
```
L<n>: Expression
```
- **n:** Integer layer number (1-indexed)
- **Semantics:** Independent parallel timeline
- **Scope:** Per-system

### 2.2 Temporal Scale
```
@temporal_scale α;
```
- **α:** Positive real number
- **Effect:** Multiplies all time constants by α
- **Default:** α = 1.0

---

## 3. Phase as Time

### 3.1 Phase Ordering
**Within each layer, phases must be non-decreasing:**
```
θ₁ ≤ θ₂ ≤ θ₃ ≤ ... ≤ θₙ
```

**Violation:** Phase non-monotonic error

### 3.2 Phase Spacing = Timing

| Spacing | Δθ | Relationship |
|---------|-----|--------------|
| Tight   | 15-30° | Rapid succession |
| Moderate| 45-60° | Sequential stages |
| Separated | 90-180° | Long intervals |

### 3.3 Relative Time
```
t_relative = θ / 360°
```
- θ = 0° → t = 0.0
- θ = 90° → t = 0.25
- θ = 180° → t = 0.5
- θ = 360° → t = 1.0

---

## 4. Duration from Curvature

### 4.1 Time Constant
```
τ = 1 / |κ|
```

**Physical meaning:**
- High |κ| (e.g., -5.0) → fast/brief (τ = 0.2)
- Low |κ| (e.g., -1.0) → slow/prolonged (τ = 1.0)

### 4.2 Temporal Scaling
```
τ_scaled = α · (1/|κ|)
```

### 4.3 Examples
```
Component @|-5.0 → τ = 0.2 time units (fast)
Component @|-1.0 → τ = 1.0 time units (slow)

With α=2.0:
  τ_scaled = 2.0 × 0.2 = 0.4 (still faster than second)
```

---

## 5. Timeline Computation

### 5.1 Phase-Weighted Duration
For sequence `A₁@θ₁|κ₁ * A₂@θ₂|κ₂ * ... * Aₙ@θₙ|κₙ`:

```
T_total = Σᵢ₌₁ⁿ⁻¹ [(θᵢ₊₁ - θᵢ)/360°] · τᵢ + τₙ
```

**Components:**
- `(θᵢ₊₁ - θᵢ)/360°`: Phase coverage of component i
- `τᵢ`: Duration of component i
- `τₙ`: Final component duration

### 5.2 Example Calculation
```
P:1@0|-3 * C:2@45|-2 * B:3@90|-1.5
```

**Step by step:**
```
τ_P = 1/3 = 0.333
τ_C = 1/2 = 0.500
τ_B = 1/1.5 = 0.667

Coverage_P = (45-0)/360 = 0.125
Coverage_C = (90-45)/360 = 0.125

T_total = 0.125 × 0.333 + 0.125 × 0.500 + 0.667
        = 0.042 + 0.063 + 0.667
        = 0.772 time units
```

---

## 6. Layer Semantics

### 6.1 Independence
**Layers are parallel timelines:**
- No temporal constraints between layers
- Each layer has independent phase ordering
- System completion = max(layer_durations) or sum (depends on merge semantics)

### 6.2 Example
```
L1: A@0|-3 * B@30|-2 * C@60|-1.5
L2: D@90|-2 * E@45|-2.5
```

**Valid:**
- L1 phases: 0° → 30° → 60° (monotonic ✓)
- L2 phases: 90° → 45° (no constraint, different layer)
- E@45° before B@30° is OK (different timelines)

### 6.3 Merge Points
When layers converge (implicit or via output):
```
L1: ... => intermediate
L2: ... => intermediate
Combined => O:1@180|-2
```

---

## 7. Temporal Constraints

### 7.1 Phase Monotonicity (Per Layer)
**Rule:** `θᵢ ≤ θᵢ₊₁` for all consecutive components in same layer

**Violation example:**
```
L1: P@90 * C@45  // ERROR: 90° → 45° backward
```

### 7.2 No Wraparound
**Rule:** Cannot go 359° → 0° within same chain

**Detection:** If `Δθ < -180°` → wraparound detected

**Workaround:**
```
L1: A@350 => O1@360;
L2: O1@0 * B@10;
```

### 7.3 Output Phase Bound
**Rule:** Output phase ≥ all upstream phases

**Example:**
```
P@0 * C@90 => O@45  // ERROR: Output at 45° before C at 90°
P@0 * C@90 => O@180 // VALID: Output at 180° after all
```

---

## 8. Execution Semantics

### 8.1 Component Activation
Component at phase θ activates at time:
```
t_activate = θ / 360°
```

### 8.2 Component Completion
Component completes at:
```
t_complete = t_activate + τ
```

### 8.3 Sequential Flow
For `A * B`:
```
B activates after A completes (coupling-dependent)
```

Actual activation:
```
t_B_activate = t_A_complete · coupling_factor
```
where `coupling_factor = (1 + cos(Δθ))/2` (normalized)

---

## 9. Multi-Phase Temporal Presence

### 9.1 Weighted Temporal Distribution
```
C:2@[30:0.6+60:0.4]|-2
```

**Interpretation:**
- Component is 60% present at time t=30°/360°
- Component is 40% present at time t=60°/360°
- Duration τ = 1/2 = 0.5 applies to component, not per phase

### 9.2 Effective Time
```
t_effective = Σᵢ wᵢ · (θᵢ/360°)
```

---

## 10. Temporal Patterns

### 10.1 Linear Progression
```
P@0|-3 * C@30|-2 * B@60|-1.5 * O@90|-1
```
- Regular spacing (30° intervals)
- Sequential processing
- Predictable timing

### 10.2 Parallel Start, Sequential Merge
```
L1: A@0|-4 * B@0|-4
L2: C@90|-2
=> O@180|-1.5
```
- A and B start simultaneously (different layers)
- Both complete at ~0.25 time
- C starts after at t=0.25
- Merge at output

### 10.3 Fast-Then-Slow Cascade
```
P@0|-5:'rapid' * C@30|-1:'sustained'
```
- Rapid phase: τ=0.2 completes quickly
- Sustained phase: τ=1.0 takes longer
- Total time dominated by slow component

### 10.4 Feedback Cycle
```
L1: A@0|-3 * B@90|-2 * C@180|-1.5 => O@270|-1
L2: O@0|-2 * A  // Cycle back
```
- Linear progression in L1
- Output feeds back in L2
- Creates temporal loop

---

## 11. Library Temporal Patterns

### 11.1 Definition
```
$T.Fast = {P@0|-5 * C@30|-4 => O@60|-3};
$T.Slow = {P@0|-1 * C@60|-0.8 => O@120|-0.5};
```

### 11.2 Usage
```
L1: $T.Fast * $T.Slow => O@180;
```

**Timeline:**
- Fast pattern: ~0.6 time units
- Slow pattern: ~1.8 time units
- Total: ~2.4 time units

---

## 12. Temporal Validation

### 12.1 Check Phase Monotonicity
```python
def check_monotonicity(layer):
    phases = [c.phase for c in layer.components]
    for i in range(len(phases)-1):
        if phases[i+1] < phases[i]:
            error("Phase non-monotonic", i, i+1)
```

### 12.2 Check Wraparound
```python
def check_wraparound(layer):
    phases = [c.phase for c in layer.components]
    for i in range(len(phases)-1):
        Δθ = phases[i+1] - phases[i]
        if Δθ < -180:
            error("Phase wraparound", i, i+1)
```

### 12.3 Check Output Bound
```python
def check_output(system):
    max_phase = max(c.phase for c in system.components)
    output_phase = system.output.phase
    if output_phase < max_phase:
        error("Output too early", output_phase, max_phase)
```

---

## 13. Timeline Examples

### 13.1 Simple Sequence
```
P:1@0|-3 * C:2@90|-2 => O:1@180|-2

Timeline:
  t=0.00: P activates (τ=0.333)
  t=0.33: P completes, C activates (τ=0.500)
  t=0.83: C completes, O activates (τ=0.500)
  t=1.33: O completes
  
Total: 1.33 time units
```

### 13.2 Parallel Execution
```
L1: A@0|-3 * B@30|-2
L2: C@0|-4 * D@40|-2

Timeline:
  t=0.00: A and C activate (parallel)
  t=0.25: C completes (faster)
  t=0.33: A completes, B activates
  t=0.33: D activates (layer 2 timing)
  t=0.83: B completes
  t=0.83: D completes
  
L1 duration: 0.83
L2 duration: 0.83
Total (parallel): max(0.83, 0.83) = 0.83
```

### 13.3 With Temporal Scaling
```
@temporal_scale 2.0;

P:1@0|-3 * C:2@90|-2 => O:1@180|-2

Scaled durations:
  τ_P = 2.0 × (1/3) = 0.667
  τ_C = 2.0 × (1/2) = 1.000
  τ_O = 2.0 × (1/2) = 1.000
  
Total: 2.67 time units (2× slower)
```

---

## 14. Common Errors

### 14.1 Backward Ordering
```
❌ P@90 * C@45 * B@0
✓  P@0 * C@45 * B@90
```

### 14.2 Zero Spacing
```
❌ A@30 * B@30 * C@30  // All simultaneous
✓  A@30 * B@45 * C@60  // Clear progression
```

### 14.3 Ignoring Curvature
```
❌ Fast@0|-1 * Slow@90|-1  // Same speed
✓  Fast@0|-5 * Slow@90|-1  // Fast then slow
```

### 14.4 Output Too Early
```
❌ A@0 * B@90 => O@30  // Output before B
✓  A@0 * B@90 => O@120 // Output after all
```

---

## 15. Integration with WPE

### 15.1 Combined System
```
@temporal_scale 1.0;

System =
  L1: P:1@0|-3.0:'input' * C:2@45|-2.5:'process'
  => O:2@180|-2.0:'output';
```

**WPE computes:**
- Coupling: cos(45°) = 0.707
- Shell influence: I(2,1) = 1/1 - 1/2 = 0.5

**TME computes:**
- Durations: τ_P=0.333, τ_C=0.400, τ_O=0.500
- Timeline: 0.125×0.333 + 0.375×0.400 + 0.500 = 0.692 time units

### 15.2 Full Analysis Output
```json
{
  "wpe": {
    "coupling": 0.707,
    "shell_influence": 0.5
  },
  "tme": {
    "durations": [0.333, 0.400, 0.500],
    "timeline": 0.692,
    "phases": [0, 45, 180]
  }
}
```

---

## 16. Formulas Reference

### 16.1 Core Temporal Formulas
```
τ = 1 / |κ|                              (duration)
t_rel = θ / 360°                         (relative time)
τ_scaled = α · τ                         (scaled duration)
T_total = Σ [(Δθᵢ/360°) · τᵢ] + τₙ       (timeline)
```

### 16.2 Phase Coverage
```
coverage = (θ_next - θ_current) / 360°
```

### 16.3 Multi-Phase Effective Time
```
t_eff = Σᵢ wᵢ · (θᵢ/360°)
```

---

## 17. Error Codes

- `E010`: Phase non-monotonic (decreases in sequence)
- `E011`: Phase wraparound (359° → 0° in chain)
- `E012`: Output too early (before upstream)
- `E013`: Temporal scale invalid (α ≤ 0)

---

## 18. Implementation Requirements

### 18.1 Parser
- Recognize layer syntax `L<n>:`
- Parse `@temporal_scale α;`
- Validate phase monotonicity per layer
- Allow phase independence across layers

### 18.2 Validator
- Enforce monotonicity within layers
- Check wraparound conditions
- Verify output phase bounds
- Compute timelines

### 18.3 Executor
- Calculate component durations
- Compute phase coverage
- Generate timeline estimates
- Apply temporal scaling

---

## 19. Quick Reference

| Element | Syntax | Temporal Operation | Example |
|---------|--------|-------------------|---------|
| Component | Φ:λ@θ\|κ | Phase=when, κ=duration | P@0\|-3 |
| Duration | τ=1/\|κ\| | Time at component | κ=-2→τ=0.5 |
| Timing | Δθ=θⱼ-θᵢ | Phase diff=separation | 45° apart |
| Sequence | A*B | A then B | P@0*C@45 |
| Ordering | θ₁≤θ₂≤... | Must increase | 0°≤45°≤90° |
| Layer | L1: | Parallel timeline | L1:...\|L2:... |
| Scale | @temporal_scale α | Speed multiplier | α=2→2× slower |

---

**END TME 1.0 FORMAL SPECIFICATION**
