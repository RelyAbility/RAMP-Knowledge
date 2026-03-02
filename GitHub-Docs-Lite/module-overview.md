# ESA V3 Module Overview

**Operational Reference for ESA V3 Engine**

This document describes the scope, boundaries, and operational mechanics of ESA V3. For strategic context, industry deep-dives, and knowledge base content, see [RAMP Knowledge](https://github.com/RelyAbility/RAMP-Knowledge/tree/main/ESA/V3).

---

## Module Scope

**Purpose**: Compute Volatility Type Profiles (VTP) across 20 industrial sub-categories; determine risk bands and EMRA eligibility.

**Input**: Assessment responses (0-100 scores for 198 industry-specific questions)  
**Output**: VTP scores, risk band assignment, EMRA eligibility, measurement readiness profile

**Status**: Production ready for 4 sub-industries; validation in progress for remaining 16

---

## Sub-Industries (20 Total)

### Food & Beverage (4)
| Sub-Industry | Status | VTP | Risk Band |
|---|---|---|---|
| Bakery | ✅ Validated | 58.4 | Moderate |
| Dairy | ✅ Validated | 52.0 | Moderate |
| Meat | ✅ Validated | — | — |
| Beverage | 🔄 Q1 2026 | — | — |

### Plastics & Packaging (4)
| Sub-Industry | Status | VTP | Risk Band |
|---|---|---|---|
| Plastics | ✅ Validated | — | — |
| Paper Packaging (NAICS 3222) | 🔄 Q1 2026 | — | — |
| Flexible Packaging | 🔄 Q1 2026 | — | — |
| Rigid Packaging | 🔄 Q1 2026 | — | — |

### Metal Products (3)
| Sub-Industry | Status | VTP | Risk Band |
|---|---|---|---|
| Fabricated Metal | 🔄 Q2 2026 | — | — |
| Foundries | 🔄 Q2 2026 | — | — |
| Machining & Metalworking | 🔄 Q2 2026 | — | — |

### Chemicals & Pharmaceuticals (3)
| Sub-Industry | Status | VTP | Risk Band |
|---|---|---|---|
| Chemicals | 🔄 Q2 2026 | — | — |
| Pharmaceuticals | 🔄 Q2 2026 | — | — |
| Specialty Chemicals | 🔄 Q2 2026 | — | — |

### Mining & Primary Metals (3)
| Sub-Industry | Status | VTP | Risk Band |
|---|---|---|---|
| Mining | 🔄 Q3 2026 | — | — |
| Primary Metals Smelting | 🔄 Q3 2026 | — | — |
| Rolling & Finishing | 🔄 Q3 2026 | — | — |

### Facilities Management (4)
| Sub-Industry | Status | VTP | Risk Band |
|---|---|---|---|
| Hospitals | 🔄 Q3 2026 | — | — |
| Aged Care | 🔄 Q3 2026 | — | — |
| Large Retail | 🔄 Q3 2026 | — | — |
| Campuses | 🔄 Q3 2026 | — | — |

**Legend**: ✅ = Validated | 🔄 = In Progress | — = Not yet completed

---

## Engine Components

### Core Calculation Engine (`engine/engine.py`)

**Functions**:

#### `calculate_vtp(assessment_data: dict, sub_industry: str) → dict`

Computes Volatility Type Profile from assessment responses.

**Parameters**:
- `assessment_data`: Question scores {q001: 75, q002: 45, ...}
- `sub_industry`: Industry code ("Bakery", "Dairy", etc.)

**Returns**:
```python
{
    "structural_volatility": float,      # 0-100
    "constraint_volatility": float,      # 0-100
    "performance_volatility": float,     # 0-100
    "overall_volatility": float,         # 0-100
    "confidence_level": float            # 0-1.0
}
```

#### `assign_risk_band(vtp_scores: dict) → dict`

Assigns risk band based on overall volatility score.

**Returns**:
```python
{
    "risk_band": str,                   # "Low", "Moderate", "High", "Critical"
    "overall_score": float,             # 0-100
    "drivers": list,                    # [SV, CV, PV]
    "intervention_intensity": str,      # "Light", "Standard", "Intensive"
    "recommendation": str
}
```

#### `determine_emra_eligibility(vtp_scores: dict, sub_industry: str, additional_factors: dict = {}) → dict`

Determines EMRA eligibility based on VTP and sub-industry rules.

**Returns**:
```python
{
    "eligibility": str,                 # "Eligible", "Conditional", "Not Eligible"
    "rationale": str,
    "prerequisites": list,              # if Conditional
    "estimated_readiness_date": str,
    "next_assessment_date": str
}
```

#### `generate_measurement_readiness(sub_industry: str, current_infrastructure: dict) → dict`

Creates measurement readiness profile based on current state.

**Returns**:
```python
{
    "current_state": str,
    "maturity_level": int,              # 1-5
    "identified_gaps": list,
    "priority_upgrades": list,
    "estimated_timeline_months": int
}
```

---

### EMRA Eligibility Rules (`engine/emra_rules.py`)

Determines eligibility state based on VTP and sub-industry factors.

**Eligible**: Overall volatility ≤ 75 AND has measurement infrastructure  
**Conditional**: Overall volatility 61-80 AND missing infrastructure  
**Not Eligible**: Overall volatility ≥ 81 OR regulatory blockers

See [RAMP Engine Documentation](../../ESA/V3/Documentation/Engine_Documentation.md) for complete rules.

---

### Measurement Readiness (`engine/measurement_readiness.py`)

Identifies gaps in metering infrastructure and measurement capability.

Returns maturity assessment (1-5) and priority upgrades for EMRA readiness.

---

## Question Taxonomy

**Total Questions**: 198 unique questions (consistent across all sub-industries)

**Distribution**:
- Structural Volatility (SV): ~66 questions
- Constraint Volatility (CV): ~66 questions
- Performance Volatility (PV): ~66 questions

**Question Format**: 0-100 scale responses

**Sub-Industry Customization**: Question wording and weighting adjusted per industry; core framework consistent.

For full question mappings and taxonomies, see [RAMP Knowledge](https://github.com/RelyAbility/RAMP-Knowledge/tree/main/ESA/V3).

---

## Validation Mechanics

### Reference Data

Each validated sub-industry has a "gold standard" JSONL reference file:

```jsonl
{"sub_industry": "Bakery", "structural_volatility": 58.4, "constraint_volatility": 41.2, "performance_volatility": 63.1, "overall_volatility": 54.2, ...}
```

**Location**: `data/reference/[industry]_reference.jsonl`

### Validation Process

1. **Load reference data** from JSONL file
2. **Run engine** with same assessment inputs
3. **Compare outputs** (VTP must match exactly or within tolerance ±0.1%)
4. **Record results** in `validation/results/[industry].md`
5. **Update checklist** in `docs/module-checklist.md`

See [validation-workflow.md](validation-workflow.md) for complete steps.

---

## Data Dictionary

See [data-dictionary.md](data-dictionary.md) for complete schema definitions, required fields, and validation rules.

---

## Architecture

```
Assessment Input
    ↓
Question Mapping (SV/CV/PV classification)
    ↓
Score Aggregation (average per dimension)
    ↓
Overall VTP Calculation
    ↓
Risk Band Assignment (thresholds)
    ↓
EMRA Eligibility Rules
    ↓
Measurement Readiness Assessment
    ↓
Output Package (VTP, risk band, EMRA status, readiness)
```

---

## Performance Characteristics

| Metric | Target | Status |
|--------|--------|--------|
| Calculation time per assessment | <100ms | ✅ Meets target |
| VTP match to reference | 100% (±0.1% tolerance) | ✅ 4/4 validated |
| EMRA eligibility accuracy | >95% | 🔄 In validation |
| Engine uptime | 99.9% | ✅ Stable |

---

## Versioning

**Current Version**: 3.0.0  
**Release Date**: March 2, 2026  
**Status**: Production Ready (Partial - 4/20 sub-industries validated)

See [CHANGELOG.md](../CHANGELOG.md) for version history.

---

## Next Steps

- Validate remaining 16 sub-industries (Q1-Q3 2026)
- Build UI layer for assessment delivery
- Integrate with EMRA enrollment workflows
- Develop continuous monitoring for VTP trending

---

## Additional Resources

- **Complete Engine Specs**: [RAMP Engine Documentation](../../ESA/V3/Documentation/Engine_Documentation.md)
- **Sub-Industry Deep-Dives**: [RAMP Sub-Industry Modules](../../ESA/V3/)
- **Validation Status**: [module-checklist.md](module-checklist.md)
- **Data Formats**: [data-dictionary.md](data-dictionary.md)

---

*Part of the RAMP ecosystem platform.*
