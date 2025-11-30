# WPE 5.0 + TME 1.0 - Quick Reference Card

**Single-page reference for both specifications**

---

## Component Syntax

```
Φ:λ@θ|κ[:role]
```

| Field | Type | Range | Meaning |
|-------|------|-------|---------|
| Φ | Domain | P,C,B,S,Ph,O,NX,MT,M | Interaction domain |
| λ | Shell | [1,9] | Hierarchical level |
| θ | Phase | [0,359]° | Position/timing |
| κ | Curvature | [-10,10] | Stability/duration |
| role | String | Optional | Documentation |

---

## Operators

| Op | Syntax | WPE Semantics | TME Semantics |
|----|--------|---------------|---------------|
| * | A * B | Sequential flow | A then B in time |
| + | A + B | Parallel branches | Concurrent execution |
| => | S => O | Output extraction | Final state |
| <-> | A <-> B | Bidirectional | Mutual influence |
| [] | [A*B] | Subsystem boundary | Grouped timeline |

---

## Core Formulas

### WPE (Structure)
```
c = cos(Δθ)                    // Coupling strength
Δθ = θⱼ - θᵢ                   // Phase difference
I = 1/λ_low - 1/λ_high        // Shell influence
```

### TME (Time)
```
τ = 1/|κ|                      // Duration
t_rel = θ/360°                 // Relative time
T = Σ[(Δθᵢ/360°)·τᵢ] + τₙ      // Total timeline
τ_scaled = α·τ                 // Temporal scaling
```

---

## Coupling Interpretation

| Δθ | cos(Δθ) | Relationship |
|----|---------|--------------|
| 0° | +1.0 | Maximum synergy |
| 45° | +0.707 | Strong positive |
| 60° | +0.5 | Moderate |
| 90° | 0.0 | Independent |
| 120° | -0.5 | Opposition |
| 180° | -1.0 | Maximum opposition |

---

## Phase Spacing → Timing

| Δθ | TME Interpretation |
|----|-------------------|
| 15-30° | Rapid succession |
| 45-60° | Sequential stages |
| 90-180° | Long intervals |

---

## Curvature → Duration

| κ | τ = 1/\|κ\| | Type |
|---|------------|------|
| -5.0 | 0.2 | Fast/brief |
| -3.0 | 0.333 | Standard |
| -2.0 | 0.5 | Moderate |
| -1.0 | 1.0 | Slow/prolonged |
| -0.5 | 2.0 | Very slow |

**Stability:**
- κ < -3.0: Deep well (persistent)
- -3.0 ≤ κ ≤ -1.0: Standard well
- -1.0 < κ < 0: Shallow (transient)
- κ = 0: Critical (unstable)
- κ > 0: Peak (repulsive)

---

## Shell Hierarchy

| Level | Type | Example Processes |
|-------|------|------------------|
| 1-3 | Foundation | Input, basic processing |
| 4-6 | Integration | Synthesis, combination |
| 7-9 | Abstraction | Meta-analysis, output |

**Influence:** Higher shells constrain lower shells
```
I(λ_high, λ_low) = 1/λ_low - 1/λ_high
```

---

## Layer Syntax (TME)

```
L1: A@0|-3 * B@30|-2
L2: C@45|-2 * D@90|-1.5
=> O@180|-2
```

**Rules:**
- Each layer = independent timeline
- Phases monotonic within layer
- No constraints between layers
- System completion = merge point

---

## Temporal Constraints

### Per Layer:
1. **Phase monotonicity:** θ₁ ≤ θ₂ ≤ ... ≤ θₙ
2. **No wraparound:** Cannot go 359° → 0°
3. **Output bound:** Output phase ≥ all upstream

### Global:
- Temporal scale: `@temporal_scale α` (α > 0)
- Default: α = 1.0

---

## Multi-Phase Syntax

### Range (equal weights):
```
@[0:30:60]  →  @[0:0.333+30:0.333+60:0.334]
```

### Weighted:
```
@[0:0.7+45:0.3]
```

### Effective phase:
```
θ_eff = Σᵢ wᵢ·θᵢ
```

---

## Library System

### Definition:
```
$Domain.Category.Name = Expression;
```

### Reference:
```
$Domain.Category.Name
```

### Multi-select:
```
$B{DNA+Mito+Cyto}  →  ($B.DNA) + ($B.Mito) + ($B.Cyto)
```

---

## Example: Complete System

```
@temporal_scale 1.0;

$B.DNA = P:1@0|-3.5 * C:2@30|-2.5;

System =
  L1: $B.DNA * B:2@45|-2.8:'processing';
  L2: C:3@[90:0.7+120:0.3]|-1.8:'regulation';
  [B:3@150|-1.5 * C:4@165|-1.2]
  => O:3@180|-2.5:'output';
```

**WPE Analysis:**
- Couplings: cos(30°)=0.866, cos(15°)=0.966, cos(15°)=0.966
- Shell influences: Various I(λ_high, λ_low)

**TME Analysis:**
- L1 duration: ~0.5 time units
- L2 duration: ~0.6 time units  
- Total: ~1.2 time units (parallel merge)

---

## Validation Quick Check

- [ ] All shells in [1,9]
- [ ] All phases in [0,359]
- [ ] All κ in [-10,10]
- [ ] κ < 0 (stability policy)
- [ ] Phases monotonic per layer
- [ ] No phase wraparound
- [ ] Output phase ≥ upstream phases
- [ ] System has output component
- [ ] No cycles (except via <->)

---

## Error Codes (Common)

| Code | Issue |
|------|-------|
| E002 | Shell out of range |
| E003 | Phase out of range |
| E004 | Curvature out of range |
| E005 | Unstable curvature (κ ≥ 0) |
| E007 | Missing output |
| E010 | Phase non-monotonic |
| E011 | Phase wraparound |
| E012 | Output too early |

---

## Computation Pipeline

```
Input: CRYS program
  ↓
Parse → AST
  ↓
Expand libraries
  ↓
Validate ranges
  ↓
Compute WPE:              Compute TME:
- Couplings (cos Δθ)      - Durations (1/|κ|)
- Shell influences        - Timeline (Σ coverage·τ)
- Coupling matrix         - Phase ordering
  ↓
Output: JSON result
```

---

## Minimal Valid Program

```
P:1@0|-3 => O:1@0|-2;
```

**Analysis:**
- Components: 2
- Coupling: None (single sequence)
- Durations: τ_P=0.333, τ_O=0.500
- Timeline: 0.833 time units

---

**Version:** WPE 5.0 + TME 1.0  
**Date:** 2025-11-06  
**Format:** Formal Reference Card
