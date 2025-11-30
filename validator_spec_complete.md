# CRYS Validator Specification v1.0 (COMPLETE)

This document defines **semantic validation rules** for CRYS programs. Syntax is governed by `crys_complete.ebnf`.

Semantics are derived from:
- WPE 5.0 (syntax-as-computation)
- TME 1.0 (time-as-ordering)
- Mindlitch v2.0 (1:1 reversible encoding)

---

## 1. Validation Pipeline

### 1.1 Phase 0: Mindlitch Pre-Validation

**IF** program begins with `###MINDLITCH` section:

1. **Extract breadcrumb**: Parse Mindlitch symbolic string
2. **Decode to WPE/TME**: Apply Mindlitch v2.0 decoding algorithm:
   - Parse domain symbols (⬡✝○~=P, □○=B, etc.)
   - Parse shell markers (①=1, ②=2, etc.)
   - Decode phase phonetics (@④⑤ = @45°)
   - Decode curvature phonetics (κ−②·⑤ = κ-2.5)
   - Decode role hashes (ℜXXX → role lookup)
3. **Generate reference CRYS**: Convert decoded WPE/TME to CRYS syntax
4. **Defer comparison**: Store reference for final verification (Phase 7)

**ELSE**: Skip Mindlitch validation

### 1.2 Phase 1: Lex/Parse

1. **Tokenize** per `crys_complete.ebnf`
2. **Parse** using grammar rules
3. **On failure** → `E000:parse-error` with position and context
4. **Success** → Abstract Syntax Tree (AST)

### 1.3 Phase 2: Declaration Processing

#### 2.1 Temporal Scale
- **Scope**: Per-system unless globally declared before all systems
- **Validation**: 
  - α must be positive real number (α > 0)
  - If α ≤ 0 → `E013:temporal-scale-invalid`
  - If multiple conflicting scales in scope → `E024:temporal-scale-conflict`
- **Application**: Multiplies all time constants (τ = 1/|κ|) by α
- **Default**: α = 1.0 if not declared

#### 2.2 Domain Aliases
```
domain Bio = B;
```
- Creates alias `Bio` → `B`
- Validation:
  - Target must be valid domain (built-in or user-defined)
  - Alias cannot shadow built-in domains → `E017:domain-forbidden`
  - Circular aliases → `E025:circular-domain-alias`
- Scope: Global within program

#### 2.3 Import/Using
- **Status**: Reserved for future extension
- **Current behavior**: Parse and store, emit `W020:unknown-using` if referenced
- **No validation errors** in v1.0

### 1.4 Phase 3: Library Definition Expansion

#### 3.1 Build Dependency Graph
1. Extract all `$Domain.Cat.Name = Expression;` definitions
2. For each definition, identify all library references in RHS
3. Construct directed graph: `$A → $B` if `$A` uses `$B`

#### 3.2 Detect Cycles
- Apply DFS cycle detection
- If cycle found → `E016:library-recursion` with cycle path
- Exception: Self-reference with base case allowed (future: conditional expansion)

#### 3.3 Topological Sort
- Order definitions by dependencies (leaves first)
- Expansion order: bottom-up (dependencies before dependents)

#### 3.4 Expand Libraries
**Algorithm:**
```
function expand_library(lib_ref, depth):
    if depth > 10:
        error E022:expansion-too-deep
    
    if lib_ref not in definitions:
        error E005a:unresolved-library
    
    expr = definitions[lib_ref]
    
    for each nested_ref in expr:
        expr = substitute(nested_ref, expand_library(nested_ref, depth+1))
    
    return expr
```

**Multi-select expansion:**
```
$B{DNA+Mito+Cyto} 
→ expands to: ($B.DNA) + ($B.Mito) + ($B.Cyto)
```
- Each component wrapped in group
- Combined with `+` (parallel operator)
- Individual components recursively expanded

#### 3.5 Normalize AST
- Replace all library references with expanded definitions
- Maintain source mapping for error messages
- Result: Pure CRYS AST with no library refs

### 1.5 Phase 4: Domain Resolution

#### 4.1 Built-in Domains
Reserved keywords (case-sensitive):
```
P  = Physics
C  = Cognition
B  = Biology
S  = Sociology
Ph = Philosophy
O  = Output
NX = NexusField (unknown)
MT = Manufacturing
M  = Memory/Metafield
```

#### 4.2 User-Defined Domains
- Any uppercase identifier not in built-in set
- Examples: `Bio`, `Tech`, `Econ`, `Custom`
- No registration required (dynamically recognized)
- Validation: First character must be uppercase → `E001:invalid-domain`

#### 4.3 Domain Aliases
- Resolved per Phase 2.2
- Substituted during parsing
- Must not create ambiguity

### 1.6 Phase 5: Component Range Checks

For each `Component = Domain:Shell@Phase|Curvature:Role`:

#### 5.1 Shell Validation
- **Range**: `shell ∈ [1, 9]` (inclusive)
- **Type**: Integer
- **Error**: `E002:shell-out-of-range`
- **Semantic meaning**:
  - 1-3: Foundation/processing
  - 4-6: Integration
  - 7-9: Abstraction/meta-level

#### 5.2 Phase Validation

**Single Angle:**
- **Range**: `angle ∈ [0, 359]` (inclusive, integer degrees)
- **Error**: `E003:phase-out-of-range`

**Phase Range** `@[θ₁:θ₂:...:θₙ]`:
- Each angle validated individually
- **Normalization**: Generate equal weights `w = 1/n` for each
- **Internal representation**: Convert to weighted form
- Example: `@[0:30:60]` → `@[0:0.333+30:0.333+60:0.334]`

**Phase Weighted** `@[θ₁:w₁+θ₂:w₂+...+θₙ:wₙ]`:
- Each angle validated: `θᵢ ∈ [0, 359]`
- Each weight must be: `wᵢ > 0` (positive real)
- **Normalization**: If `Σwᵢ ≠ 1.0`:
  - Normalize: `wᵢ' = wᵢ / Σwⱼ`
  - Emit `W001:weights-renormalized`
- **Error codes**:
  - Empty list: `E014:multi-phase-empty`
  - Negative/NaN/Inf weight: `E015:phase-weight-invalid`

**Phase Pattern** `@identifier`:
- Identifier must reference defined pattern (future feature)
- Current: Treated as opaque identifier (pass-through)
- No validation in v1.0

**Phase Function** `@Θ(expr)`:
- **Status**: Reserved, not supported in v1.0
- **Error**: `E020:dynamic-phase-unsupported`
- Future: Will validate expression syntax and parameters

#### 5.3 Curvature Validation
- **Range**: `κ ∈ [-10.0, 10.0]` (inclusive, real number)
- **Type**: Signed decimal
- **Range error**: `E004:curvature-out-of-range`
- **Stability policy** (default):
  - Require: `κ < 0` (stable well)
  - If `κ ≥ 0`: `E005:unstable-curvature`
  - Override: `--allow-unstable` flag suppresses E005
- **Physical meaning**:
  - `κ < -3.0`: Deep well (persistent state)
  - `-3.0 ≤ κ ≤ -1.0`: Standard well
  - `-1.0 < κ < 0`: Shallow well (transient)
  - `κ = 0`: Critical point (unstable)
  - `κ > 0`: Peak (repulsive)

#### 5.4 Role Validation
- **Format**: Single-quoted string with escapes
- **Escapes**: `\'`, `\\`, `\n`, `\t`, `\r`
- **Max length**: 256 characters (advisory, emit `W025:role-too-long`)
- **Unclosed string**: `E018:role-string-unclosed`
- **Semantic meaning**: Documentation only, no computational effect
- Optional: If omitted, role is empty

#### 5.5 Component Completeness
- All four required fields must be present: Domain, Shell, Phase, Curvature
- Missing any field → `E019:component-missing-field`

### 1.7 Phase 6: Structural Validation

#### 6.1 System Structure
**Required elements:**
- At least one expression (component or group)
- Exactly one output component after `=>`
- Output component must be a `Component`, not a group/library ref

**Errors:**
- No output → `E007:missing-output`
- Multiple outputs in same system → `E026:multiple-outputs`
- Output is not a component → `E027:output-not-component`

#### 6.2 Layer Structure

**Layer numbering:**
- Layers numbered `L1, L2, L3, ...` (sequential, 1-indexed)
- **Requirement**: Consecutive numbering (no gaps)
- **Example INVALID**: `L1: ... L3: ...` (missing L2) → `E021:layer-gap`
- **Example VALID**: `L1: ... L2: ... L3: ...`

**Layer scope:**
- Layers are local to each system
- Different systems can have different layer counts
- Layer `Ln` in System A is independent from layer `Ln` in System B

**Layer semantics:**
- Each layer represents an independent parallel timeline
- Components within same layer follow sequential ordering (TME rules)
- Components across layers can have any phase relationships

**No layer declaration:**
- If no explicit layers, entire expression is implicitly `L1:`
- Single-layer systems don't require `L1:` prefix

#### 6.3 Operator Validation

**Precedence** (low to high):
1. `=>` (output)
2. `+` (parallel)
3. `*` (sequence)
4. `<->` (coupling)

**Validation:**
- Operators must connect valid terms
- No dangling operators: `A * * B` → `E008:operator-misuse`
- No operator at start/end: `* A`, `A *` → `E008:operator-misuse`

**Empty groups:**
- `()` or `[]` with no content → `E009:empty-group`

#### 6.4 Graph Structure (Cycle Detection)

**Within a layer:**
- Sequence operator `*` creates directed edges: `A * B` → edge `A→B`
- Must form a DAG (Directed Acyclic Graph)
- Cycles prohibited (except via explicit `<->`)

**Cycle detection algorithm:**
```
function detect_cycles(layer):
    for each component C in layer:
        visited = {}
        if has_cycle(C, visited):
            error E006:cycle-not-allowed with path
            
function has_cycle(node, visited):
    if node in visited:
        return true
    visited[node] = true
    for each successor S via '*':
        if has_cycle(S, visited):
            return true
    return false
```

**Explicit cycles:**
- `<->` operator creates bidirectional coupling (allowed)
- Does not violate acyclicity requirement
- Represents mutual influence, not sequential flow

**Across layers:**
- No cycle constraints between layers
- Layers are independent parallel timelines

### 1.8 Phase 7: Temporal/Ordering Checks (TME Rules)

#### 7.1 Phase Monotonicity (Per Layer)

**Rule:** Within each layer, along any `*` chain, phases must be non-decreasing.

**Algorithm:**
```
function check_monotonicity(layer):
    for each sequence chain A₁ * A₂ * ... * Aₙ:
        for i = 1 to n-1:
            θᵢ = effective_phase(Aᵢ)
            θᵢ₊₁ = effective_phase(Aᵢ₊₁)
            if θᵢ₊₁ < θᵢ:
                error E010:phase-nonmonotonic at (Aᵢ, Aᵢ₊₁)
```

**Effective phase for multi-phase components:**
```
function effective_phase(component):
    if single angle θ:
        return θ
    if weighted multi-phase @[θ₁:w₁+...+θₙ:wₙ]:
        return Σᵢ wᵢ·θᵢ  // weighted average
```

**Violation example:**
```
L1: P:1@90|-3 * C:2@45|-2  // INVALID: 90° → 45° is backward
```

**Valid example:**
```
L1: P:1@0|-3 * C:2@45|-2 * B:3@90|-1.5  // VALID: 0° → 45° → 90°
```

#### 7.2 Phase Wraparound Prohibition

**Rule:** Phases cannot "wrap around" from high angles to low within a chain.

**Detection:**
- If `θᵢ > 270` and `θᵢ₊₁ < 90` → likely wraparound
- Check: `Δθ = θᵢ₊₁ - θᵢ`
- If `Δθ < -180` → wraparound detected

**Error:** `E011:phase-wraparound`

**Example INVALID:**
```
L1: A@350|-2 * B@10|-2  // 350° → 10° wraps around
```

**Workaround:** Use new layer for cyclic behavior:
```
L1: A@350|-2 => O1@360|-2;
L2: O1@0|-2 * B@10|-2;
```

#### 7.3 Output Phase Bound

**Rule:** Output component phase must be ≥ all upstream phases in its system.

**Algorithm:**
```
function check_output_phase(system):
    θ_max = max(effective_phase(C) for C in all_components(system))
    θ_output = effective_phase(output_component)
    if θ_output < θ_max:
        error E012:output-too-early
```

**Rationale:** Output represents system completion, must occur after all processing.

**Example INVALID:**
```
P:1@0|-3 * C:2@90|-2 => O:1@45|-2  // Output at 45° before C at 90°
```

**Example VALID:**
```
P:1@0|-3 * C:2@90|-2 => O:1@180|-2  // Output at 180° after all
```

#### 7.4 Temporal Scale Application

**Scope resolution:**
1. Check for `@temporal_scale α;` before system declaration
2. If found: applies to that system and subsequent systems
3. If multiple declarations: later overrides earlier (emit `W026:temporal-scale-override`)
4. Default: α = 1.0

**Computation:**
- For each component with curvature κ:
  - Base duration: `τ_base = 1/|κ|`
  - Scaled duration: `τ = α · τ_base`
- Time constants used in timeline estimation (Phase 8)

#### 7.5 Cross-Layer Independence

**Rule:** Different layers have independent phase constraints.

**Example VALID:**
```
L1: A@0|-3 * B@30|-2;
L2: C@90|-2 * D@45|-1.5;  // D@45 is OK; different layer from B@30
```

**No monotonicity required across layers.**

### 1.9 Phase 8: Coupling & Duration Computation (WPE/TME)

#### 8.1 Coupling Calculation

**For adjacent sequence components** `A * B`:

**Single-phase coupling:**
```
Δθ = θ_B - θ_A
coupling = cos(Δθ)
```

**Multi-phase coupling:**

For `A@[θ_A1:w_A1+...+θ_Am:w_Am] * B@[θ_B1:w_B1+...+θ_Bn:w_Bn]`:

```
Δθ_effective = ΣᵢΣⱼ w_Ai · w_Bj · |θ_Bi - θ_Aj|
coupling = cos(Δθ_effective)
```

**Alternative (dominant phase method):**
```
θ_A_dominant = arg max_i(w_Ai)  // phase with highest weight
θ_B_dominant = arg max_j(w_Bj)
coupling = cos(θ_B_dominant - θ_A_dominant)
```

**Default method:** Weighted average (first formula)
**Override:** `--coupling-method=dominant` flag

**Interpretation:**
- `coupling > 0.707` (Δθ < 45°): Strong positive coupling
- `coupling ≈ 0.5` (Δθ ≈ 60°): Moderate coupling
- `coupling ≈ 0` (Δθ ≈ 90°): Independent
- `coupling < 0` (Δθ > 90°): Opposition

**Advisory warnings:**
- `W003:coupling-orthogonal` if `|coupling| < 0.1` (informational)
- `W027:coupling-opposed` if `coupling < -0.5` (strong opposition)

#### 8.2 Duration Calculation

**Per component:**
```
τ = 1 / |κ|  (in base time units)
```

**With temporal scaling:**
```
τ_scaled = α · (1 / |κ|)
```

**Multi-phase components:**
- Duration is property of component, not each phase
- Same τ applies regardless of phase distribution

**Example:**
```
@temporal_scale 2.0;
Component C:2@45|-2.5;
τ_base = 1/2.5 = 0.4
τ_scaled = 2.0 · 0.4 = 0.8 time units
```

#### 8.3 Timeline Estimation

**Phase-weighted timeline** (per TME):
```
T_total ≈ Σᵢ (Δθᵢ / 360°) · τᵢ
```

where:
- `Δθᵢ` = phase coverage of component `i` (spacing to next component)
- `τᵢ` = duration of component `i`

**Algorithm:**
```
function estimate_timeline(layer):
    components = sort_by_phase(layer.components)
    total = 0
    for i = 0 to n-2:
        Δθ = components[i+1].phase - components[i].phase
        coverage = Δθ / 360.0
        total += coverage * components[i].duration
    # Last component to end (360° or output phase)
    total += components[n-1].duration
    return total
```

**Per-layer timeline:**
- Each layer has independent timeline
- System completion = max(layer timelines) if parallel
- System completion = sum(layer timelines) if sequential

**Output metadata:**
- Timeline estimate is advisory (not validated)
- Included in validation report for analysis

### 1.10 Phase 9: Shell Hierarchy Validation (Advisory)

#### 9.1 Shell Influence Rule

**Principle:** Higher shells constrain/influence lower shells.

**Check:** For component pairs with same domain in sequence:
```
if A.domain == B.domain and A.shell > B.shell:
    if A is not before B in any path:
        warn W010:shell-influence-unchecked
```

**Example:**
```
P:3@0|-2 * P:1@30|-3  // WARN: Shell 3 should influence shell 1, but shell 1 comes after
```

**Advisory only:** Does not fail validation (warning, not error).

**Rationale:** WPE framework suggests higher shells provide context for lower shells, but not strictly enforced.

### 1.11 Phase 10: Mindlitch Consistency Check

**IF** Mindlitch breadcrumb was present (Phase 0):

1. **Generate Mindlitch from CRYS:** Encode validated CRYS program → Mindlitch symbolic
2. **Compare with breadcrumb:** String comparison (ignoring whitespace)
3. **On mismatch:** `E030:mindlitch-mismatch` with diff

**Encoding algorithm:**
```
function encode_to_mindlitch(component):
    domain_symbol = domain_to_symbol(component.domain)
    shell_symbol = number_to_circled(component.shell)
    phase_encoding = encode_phase_phonetic(component.phase)
    kappa_encoding = encode_kappa_phonetic(component.curvature)
    role_hash = compute_role_hash(component.role)
    
    return domain_symbol + shell_symbol + "@" + phase_encoding + 
           "κ" + kappa_encoding + "ℜ" + role_hash
```

**Phase encoding:**
```
function encode_phase_phonetic(phase):
    if single angle θ:
        return circled_digits(θ)  // 45 → ④⑤
    if multi-phase:
        # Use dominant or weighted average
        θ_eff = effective_phase(phase)
        return circled_digits(θ_eff)
```

**Kappa encoding:**
```
function encode_kappa_phonetic(κ):
    sign = "−" if κ < 0 else ""
    value = abs(κ)
    # Convert to circled digits with · for decimal
    # Example: -2.5 → −②·⑤
    return sign + circled_digits(value)
```

**Validation strictness:**
- Exact match required (after whitespace normalization)
- Mindlitch serves as cryptographic integrity check
- Ensures AI reasoning matches claimed WPE/TME structure

---

## 2. Error & Warning Code Reference

### 2.1 Parse Errors (E000-E009)

| Code | Name | Description |
|------|------|-------------|
| E000 | parse-error | Syntax does not match grammar |
| E001 | invalid-domain | Domain identifier malformed |
| E002 | shell-out-of-range | Shell not in [1,9] |
| E003 | phase-out-of-range | Angle not in [0,359] |
| E004 | curvature-out-of-range | κ not in [-10,10] |
| E005 | unstable-curvature | κ ≥ 0 (policy violation) |
| E005a | unresolved-library | Library reference not defined |
| E006 | cycle-not-allowed | Cycle in sequence chain |
| E007 | missing-output | System lacks output component |
| E008 | operator-misuse | Invalid operator placement |
| E009 | empty-group | Grouping contains nothing |

### 2.2 Temporal Errors (E010-E020)

| Code | Name | Description |
|------|------|-------------|
| E010 | phase-nonmonotonic | Sequence phase decreases |
| E011 | phase-wraparound | Phase 359°→0° in chain |
| E012 | output-too-early | Output before upstream component |
| E013 | temporal-scale-invalid | α ≤ 0 or non-numeric |
| E014 | multi-phase-empty | Phase set [] is empty |
| E015 | phase-weight-invalid | Negative/NaN/Inf weight |
| E016 | library-recursion | Circular library definition |
| E017 | domain-forbidden | Policy forbids domain |
| E018 | role-string-unclosed | Missing closing quote |
| E019 | component-missing-field | Incomplete component syntax |
| E020 | dynamic-phase-unsupported | Θ(t) not implemented |

### 2.3 Structural Errors (E021-E030)

| Code | Name | Description |
|------|------|-------------|
| E021 | layer-gap | Non-consecutive layer numbers |
| E022 | expansion-too-deep | Library recursion > 10 levels |
| E023 | domain-reserved | Attempt to redefine built-in |
| E024 | temporal-scale-conflict | Multiple conflicting α values |
| E025 | circular-domain-alias | Domain alias cycle |
| E026 | multiple-outputs | System has > 1 output |
| E027 | output-not-component | Output is group/ref, not component |
| E028 | undefined-layer | Layer reference without definition |
| E029 | layer-order-violation | Layer used before declaration |
| E030 | mindlitch-mismatch | Breadcrumb ≠ encoded program |

### 2.4 Warnings (W001-W030)

| Code | Name | Description |
|------|------|-------------|
| W001 | weights-renormalized | Phase weights adjusted to sum=1 |
| W002 | unused-import | Declared import not used |
| W003 | coupling-orthogonal | 90° coupling (independent) |
| W010 | shell-influence-unchecked | Higher shell should constrain lower |
| W020 | unknown-using | Using clause references unknown |
| W025 | role-too-long | Role string > 256 chars |
| W026 | temporal-scale-override | Later α overrides earlier |
| W027 | coupling-opposed | Strong opposition (< -0.5) |
| W028 | shallow-well | κ ∈ (-1.0, 0) transient |
| W029 | deep-well | κ < -5.0 extremely stable |
| W030 | library-unused | Defined library never referenced |

---

## 3. Validation Output Format

### 3.1 Success Output (JSON)

```json
{
  "status": "VALID",
  "mindlitch_verified": true,
  "systems": [
    {
      "system_id": 1,
      "layers": [
        {
          "layer_num": 1,
          "components": [
            {
              "type": "Component",
              "domain": "P",
              "shell": 1,
              "phase": 0,
              "curvature": -3.0,
              "role": "input",
              "duration": 0.333
            }
          ],
          "timeline_estimate": 0.5
        }
      ],
      "couplings": [
        {
          "from": "P:1@0|-3",
          "to": "C:2@45|-2.5",
          "phase_diff": 45,
          "coupling_strength": 0.707
        }
      ],
      "total_timeline": 0.8,
      "output": {
        "domain": "O",
        "shell": 2,
        "phase": 180,
        "curvature": -2.5
      }
    }
  ],
  "warnings": [
    {
      "code": "W001",
      "message": "Phase weights renormalized",
      "location": "Line 5, Component C:2@[0:0.6+30:0.5]"
    }
  ],
  "metadata": {
    "temporal_scale": 1.0,
    "library_count": 2,
    "component_count": 5
  }
}
```

### 3.2 Failure Output (JSON)

```json
{
  "status": "INVALID",
  "errors": [
    {
      "code": "E010",
      "severity": "error",
      "message": "Phase non-monotonic in sequence",
      "location": "Line 8, L1: P:1@90|-3 * C:2@45|-2",
      "detail": "Phase decreases from 90° to 45°"
    },
    {
      "code": "E030",
      "severity": "error",
      "message": "Mindlitch breadcrumb does not match program",
      "location": "Line 1-3, ###MINDLITCH section",
      "expected": "⬡✝○~①○@⓪κ−③·⓪ℜinput",
      "actual": "⬡✝○~①○@④⑤κ−③·⓪ℜinput"
    }
  ],
  "partial_analysis": {
    "parsed_components": 3,
    "failed_at": "Line 8"
  }
}
```

---

## 4. Implementation Requirements

### 4.1 Parser Must Support

- Full EBNF grammar from `crys_complete.ebnf`
- Unicode support for Mindlitch symbols
- Source position tracking for error messages
- Multi-line breadcrumb sections
- Comment stripping before tokenization

### 4.2 Validator Must Implement

- All error codes (E000-E030)
- All warning codes (W001-W030)
- Coupling calculation (both methods)
- Duration calculation with temporal scaling
- Timeline estimation
- Mindlitch encoding/decoding
- Library expansion with cycle detection
- Layer validation with phase monotonicity
- Phase normalization (range → weighted)

### 4.3 Validator Must NOT

- Execute code or simulate behavior
- Require external dependencies for validation
- Modify source program (except normalization in AST)
- Make network requests
- Perform floating-point comparison without epsilon

### 4.4 Conformance Levels

**Level 0 (Minimal):**
- Parse grammar correctly
- Check basic ranges (E002-E005)
- Detect missing output (E007)

**Level 1 (Standard):**
- All Level 0 checks
- Temporal ordering (E010-E012)
- Library expansion (E005a, E016, E022)
- Coupling/duration calculation

**Level 2 (Complete):**
- All Level 1 checks
- Mindlitch validation (E030)
- All advisory warnings
- JSON output format
- Timeline estimation

**Level 3 (Extended):**
- All Level 2 checks
- Optimization suggestions
- Pattern recognition
- Auto-fix capabilities

---

## 5. Mindlitch Integration Details

### 5.1 Symbol Mapping Tables

#### Domain Symbols
```
P  → ⬡✝○~
C  → ⬡✝~:
B  → □○
S  → ◊/☽○
Ph → △+∩·○~
O  → ○□
NX → □⬡○
MT → ⬡··○
M  → ⬡○~
```

#### Shell Symbols
```
1 → ①    6 → ⑥
2 → ②    7 → ⑦
3 → ③    8 → ⑧
4 → ④    9 → ⑨
5 → ⑤
```

#### Digit Symbols
```
0 → ⓪
1 → ①
2 → ②
3 → ③
4 → ④
5 → ⑤
6 → ⑥
7 → ⑦
8 → ⑧
9 → ⑨
. → ·
- → −
```

#### Operator Symbols
```
*   → ×
+   → (use ×)
=>  → ⇒
<-> → (bidirectional arrow or repeat)
```

### 5.2 Role Hash Algorithm

**CRC32-based phonetic hash:**
```python
import zlib

def hash_role(role_string):
    # Compute CRC32
    crc = zlib.crc32(role_string.encode('utf-8')) & 0xffffffff
    
    # Convert to base-36
    chars = "0123456789abcdefghijklmnopqrstuvwxyz"
    result = ""
    while crc > 0:
        result = chars[crc % 36] + result
        crc //= 36
    
    # Convert to circled/symbolic form
    return encode_hash_symbols(result)
```

**Collision handling:**
- If two roles hash to same value, append disambiguator: `_1`, `_2`
- Validator maintains role→hash bidirectional map

### 5.3 Breadcrumb Verification

**Whitespace normalization:**
```python
def normalize_mindlitch(text):
    # Remove all whitespace
    return ''.join(text.split())
```

**Comparison:**
```python
def verify_breadcrumb(declared, computed):
    norm_declared = normalize_mindlitch(declared)
    norm_computed = normalize_mindlitch(computed)
    
    if norm_declared != norm_computed:
        diff = compute_diff(norm_declared, norm_computed)
        raise MindlitchMismatchError(diff)
```

**Diff format:**
```
Expected: ⬡✝○~①○@⓪κ−③·⓪
Actual:   ⬡✝○~①○@④⑤κ−③·⓪
          ___________^^^_____
          Position 11-12: phase mismatch (0° vs 45°)
```

---

## 6. Advanced Validation Features

### 6.1 Coupling Analysis Matrix

For complex systems, generate coupling matrix:

```
    | P:1 | C:2 | B:3 | O:2 |
----|-----|-----|-----|-----|
P:1 | 1.0 | 0.71| 0.0 | -0.5|
C:2 |     | 1.0 | 0.50| 0.0 |
B:3 |     |     | 1.0 | 0.71|
O:2 |     |     |     | 1.0 |
```

- Diagonal: 1.0 (self-coupling)
- Upper triangle: computed couplings
- Lower triangle: symmetric (or blank)

### 6.2 Shell Hierarchy Graph

Visualize shell relationships:
```
L9: [Ph:9] ← meta-abstraction
    ↓
L7: [C:7, O:7] ← abstract integration
    ↓
L4: [P:4, B:4] ← integration
    ↓
L2: [P:2, C:2] ← processing
    ↓
L1: [P:1, B:1] ← foundation
```

### 6.3 Timeline Gantt Chart

Generate ASCII timeline:
```
L1: [P:1]==(0.3s)====>[C:2]====(0.5s)====>[B:3]
L2:     [S:2]======(0.8s)======>[O:2]
     ├────┼────┼────┼────┼────┼────┤
     0   0.2  0.4  0.6  0.8  1.0  1.2s
```

### 6.4 Optimization Suggestions

**Auto-detect common issues:**
1. **Weak coupling**: If all couplings < 0.3, suggest phase adjustment
2. **Unnecessary layers**: If layers have no phase overlap, suggest merging
3. **Phase clustering**: If multiple components at same angle, suggest spreading
4. **Shell inversions**: If lower shells appear after higher, suggest reordering

---

## 7. Test Suite Requirements

### 7.1 Minimal Valid Program

```
P:1@0|-3 => O:1@0|-2;
```

### 7.2 All Features Program

```
###MINDLITCH
⬡✝○~①○@⓪κ−③·⓪ℜinput × ⬡✝~:②○@④⑤κ−②·⑤ℜprocess ⇒ ○□①○@①⑧⓪κ−②·⓪ℜout
---

@temporal_scale 1.5;

domain Custom = P;

$P.Example = P:1@0|-3.5 * C:2@30|-2.5;

System =
  L1: $P.Example * Custom:3@[60:0.7+90:0.3]|-2.0:'multi-phase';
  L2: B:2@45|-2.8:'biology' <-> S:3@50|-2.2:'sociology';
  
  [C:4@120|-1.5 * Ph:5@150|-1.0]
  => O:3@180|-2.0:'final_output';
```

### 7.3 Error Test Cases

Each error code must have:
- Minimal reproducing case
- Expected error message
- Suggested fix

Example:
```
# E010 Test Case
L1: P:1@90|-3 * C:2@45|-2;  # SHOULD FAIL: backward phase
# Fix: L1: P:1@45|-3 * C:2@90|-2;
```

---

## 8. Conformance Statement

A validator is **CONFORMANT** to this specification if:

1. ✓ Parses all valid programs per `crys_complete.ebnf`
2. ✓ Rejects all invalid programs with correct error codes
3. ✓ Implements all Level 2 (Complete) checks
4. ✓ Produces correct coupling/duration calculations (±0.01 tolerance)
5. ✓ Validates Mindlitch breadcrumbs (if present)
6. ✓ Outputs JSON in specified format
7. ✓ Passes reference test suite (minimum 95% pass rate)

**Version:** 1.0.0  
**Date:** 2025-11-06  
**Status:** COMPLETE SPECIFICATION

---

END SPECIFICATION
