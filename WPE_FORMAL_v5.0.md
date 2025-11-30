# WPE 5.0 - Formal Specification

**Version:** 5.0.0  
**Status:** Operational  
**Principle:** Syntax defines computation. No external formulas required.

---

## 1. Component Syntax

```
Component ::= Φ:λ@θ|κ[:role]
```

### 1.1 Domain (Φ)
- **Type:** Identifier
- **Built-ins:** `P, C, B, S, Ph, O, NX, MT, M`
- **User-defined:** Any uppercase identifier
- **Semantics:** Defines interaction domain boundaries

### 1.2 Shell (λ)
- **Type:** Integer
- **Range:** [1, 9]
- **Semantics:** Hierarchical level
  - 1-3: Foundation/processing
  - 4-6: Integration
  - 7-9: Abstraction
- **Influence:** `I(λ_high → λ_low) = 1/λ_low - 1/λ_high` (λ_high > λ_low)

### 1.3 Phase (θ)
- **Type:** Angle (degrees) or weighted set
- **Range:** [0, 359]
- **Forms:**
  - Single: `@45`
  - Range: `@[0:30:60]` → weights [1/3, 1/3, 1/3]
  - Weighted: `@[0:0.6+30:0.4]`
  - Pattern: `@identifier`
  - Function: `@Θ(t)` (reserved)

### 1.4 Curvature (κ)
- **Type:** Real number
- **Range:** [-10.0, 10.0]
- **Semantics:**
  - κ < 0: Stable well (attracts)
  - κ = 0: Critical point
  - κ > 0: Unstable peak (repels)
- **Duration:** `τ = 1/|κ|` (time units)

### 1.5 Role
- **Type:** String (optional)
- **Semantics:** Documentation only

---

## 2. Operators

### 2.1 Sequence (*)
```
A * B
```
- **Semantics:** Information flows A → B
- **Coupling:** `c(A,B) = cos(θ_B - θ_A)`
- **Ordering:** Left-to-right execution

### 2.2 Parallel (+)
```
A + B
```
- **Semantics:** Independent execution
- **Coupling:** 0 (unless overridden)
- **Combination:** Results merge at convergence

### 2.3 Grouping ([])
```
[A * B]
```
- **Semantics:** Subsystem boundary
- **Interface:** Group acts as single component externally

### 2.4 Output (=>)
```
System => O
```
- **Semantics:** Final state extraction
- **Constraint:** O must be Component type

### 2.5 Coupling (<->)
```
A <-> B
```
- **Semantics:** Bidirectional influence
- **Coupling:** Mutual, strength from phase difference

---

## 3. Coupling Computation

### 3.1 Single-Phase Coupling
```
Δθ = θ_j - θ_i
c_ij = cos(Δθ)
```

**Interpretation:**
- c = +1.0 (Δθ = 0°): Maximum synergy
- c = +0.707 (Δθ = 45°): Strong positive
- c = +0.5 (Δθ = 60°): Moderate
- c = 0.0 (Δθ = 90°): Independent
- c = -0.5 (Δθ = 120°): Opposition
- c = -1.0 (Δθ = 180°): Maximum opposition

### 3.2 Multi-Phase Coupling
For `A@[θ_A1:w_A1+...] * B@[θ_B1:w_B1+...]`:

**Method 1 (Weighted):**
```
Δθ_eff = Σᵢ Σⱼ w_Ai · w_Bj · (θ_Bj - θ_Ai)
c = cos(Δθ_eff)
```

**Method 2 (Dominant):**
```
θ_A_dom = arg max_i(w_Ai)
θ_B_dom = arg max_j(w_Bj)
c = cos(θ_B_dom - θ_A_dom)
```

### 3.3 Coupling Matrix
For system with n components:
```
C[i,j] = cos(θ_j - θ_i)  for i < j
C[i,i] = 1.0
C[j,i] = C[i,j]  (symmetric)
```

---

## 4. Duration & Timing

### 4.1 Component Duration
```
τ_base = 1 / |κ|
```

### 4.2 Temporal Scaling
```
@temporal_scale α;
τ_scaled = α · τ_base
```
- **Constraint:** α > 0
- **Default:** α = 1.0

### 4.3 Phase Coverage
For sequence A₁ * A₂ * ... * Aₙ:
```
coverage_i = (θ_{i+1} - θ_i) / 360°
```

---

## 5. Library System

### 5.1 Definition
```
$Domain.Category.Name = Expression;
```

### 5.2 Reference
```
$Domain.Category.Name
```
**Expansion:** Substitute definition at reference point

### 5.3 Multi-Select
```
$Domain{A+B+C}
```
**Expansion:** `($Domain.A) + ($Domain.B) + ($Domain.C)`

### 5.4 Expansion Rules
- Maximum depth: 10 levels
- Cycle detection: Mandatory
- Order: Topological sort (dependencies first)

---

## 6. Validation Rules

### 6.1 Range Constraints
- Shell: λ ∈ [1, 9]
- Phase: θ ∈ [0, 359]
- Curvature: κ ∈ [-10.0, 10.0]

### 6.2 Stability Policy
- **Default:** Require κ < 0
- **Override:** `--allow-unstable` flag

### 6.3 Output Requirement
- Every system must have exactly one output component
- Output component must be after `=>`

### 6.4 Graph Structure
- Sequence chains form DAG (no cycles except via `<->`)
- Grouping creates hierarchical boundaries

---

## 7. Domain Semantics

### 7.1 Built-in Domains
```
P  = Physics       (physical processes)
C  = Cognition     (cognitive processes)
B  = Biology       (biological systems)
S  = Sociology     (social systems)
Ph = Philosophy    (philosophical reasoning)
O  = Output        (system output)
NX = NexusField    (unknown/undefined)
MT = Manufacturing (production processes)
M  = Memory        (state retention)
```

### 7.2 Cross-Domain Coupling
- **Same domain:** Standard coupling rules apply
- **Different domains:** Coupling computed identically (no domain-specific adjustments)

### 7.3 Domain Boundaries
- Domains define logical partitions
- Information flow permitted across domains
- No enforcement of domain isolation

---

## 8. Shell Hierarchy

### 8.1 Influence Direction
```
Higher shells → Lower shells (context flows down)
```

### 8.2 Influence Strength
```
I(λ_h, λ_l) = 1/λ_l - 1/λ_h  where λ_h > λ_l
```

### 8.3 Information Flow
```
Info flow ∝ I(λ_h, λ_l) · c(θ_h, θ_l)
```

---

## 9. Execution Model

### 9.1 Static Analysis
For any WPE expression, compute:
1. Component durations: `τ_i = 1/|κ_i|`
2. Coupling strengths: `c_ij = cos(θ_j - θ_i)`
3. Coupling matrix: `C[n×n]`
4. Shell influences: `I(λ_i, λ_j)`

### 9.2 Result Structure
```json
{
  "components": [Component],
  "durations": [τ₁, τ₂, ..., τₙ],
  "couplings": [c₁₂, c₂₃, ..., c_{n-1,n}],
  "coupling_matrix": C[n×n],
  "shell_influences": [I(λ_i, λ_j)],
  "output": Component
}
```

### 9.3 No Dynamic Simulation
WPE defines structure, not evolution. For temporal dynamics, see TME.

---

## 10. Optimization Tiers

### 10.1 Tier 1 (Explicit)
```
P:1@45|-2.5:'input'
```
- Full specification
- 15% of typical usage

### 10.2 Tier 2 (Library)
```
$P.Thermo.Heat
```
- Reference to predefined component
- 70% of typical usage

### 10.3 Tier 3 (Pattern)
```
M:5@recurring_structure|-4.0
```
- High-level abstraction
- 15% of typical usage

---

## 11. Examples

### 11.1 Linear Pipeline
```
P:1@0|-3 * C:2@30|-2.5 * B:3@60|-2 => O:2@120|-2
```
**Computation:**
- c(P,C) = cos(30°) = 0.866
- c(C,B) = cos(30°) = 0.866
- c(B,O) = cos(60°) = 0.500
- τ_P = 0.333, τ_C = 0.400, τ_B = 0.500, τ_O = 0.500

### 11.2 Parallel Branches
```
[P:1@0|-3 * C:2@30|-2] + [B:1@90|-3 * S:2@120|-2] => O:3@180|-1.5
```
**Computation:**
- Branch 1: c(P,C) = 0.866
- Branch 2: c(B,S) = 0.866
- Branch coupling: c(C,B) = cos(60°) = 0.5

### 11.3 Library Usage
```
$B.DNA = P:1@0|-3.5 * C:2@30|-2.5;
$B.Mito = C:2@60|-2.0 * P:3@90|-1.5;

System = $B{DNA+Mito} => O:3@180|-2.5;
```
**Expansion:**
```
System = ($B.DNA) + ($B.Mito) => O:3@180|-2.5;
```

---

## 12. Error Codes

- `E001`: Invalid domain
- `E002`: Shell out of range [1,9]
- `E003`: Phase out of range [0,359]
- `E004`: Curvature out of range [-10,10]
- `E005`: Unstable curvature (κ ≥ 0)
- `E005a`: Unresolved library reference
- `E006`: Cycle in sequence chain
- `E007`: Missing output component
- `E008`: Invalid operator usage
- `E009`: Empty group

---

## 13. Formulas Reference

### 13.1 Core Formulas
```
τ = 1/|κ|                           (duration)
c = cos(Δθ)                         (coupling)
I = 1/λ_low - 1/λ_high              (shell influence)
Δθ_eff = Σᵢ Σⱼ w_i·w_j·(θ_j - θ_i)  (multi-phase)
```

### 13.2 Derived Quantities
```
T_total = Σ (Δθᵢ/360°) · τᵢ         (timeline estimate)
C[i,j] = cos(θ_j - θ_i)             (coupling matrix)
```

---

## 14. Implementation Requirements

### 14.1 Parser
- Handle all component forms
- Support library definitions
- Validate syntax per grammar

### 14.2 Validator
- Enforce range constraints
- Check graph acyclicity
- Resolve library references
- Compute couplings and durations

### 14.3 Output
- Component list with computed properties
- Coupling matrix
- Duration vector
- Shell influence graph

---

**END WPE 5.0 FORMAL SPECIFICATION**
