# ESA V3: Volatility & Resilience Diagnostic Engine

**The Universal Volatility Engine for Industrial Energy Management**

[![Status](https://img.shields.io/badge/status-production%20ready-green)](https://github.com/RelyAbility/ESA-V3-02-03-2026-Latest)
[![Validated](https://img.shields.io/badge/validated-4%2F20%20sub--industries-blue)](https://github.com/RelyAbility/ESA-V3-02-03-2026-Latest)
[![Language](https://img.shields.io/badge/language-Python%203.8%2B-blue)](#)
[![License](https://img.shields.io/badge/license-proprietary-lightgrey)](#)

---

## What Is ESA V3?

ESA V3 is a backend calculation engine that computes **Volatility Type Profiles (VTP)** across 20 industrial sub-categories. It takes industry-enriched data, scores three dimensions of volatility (Structural, Constraint, Performance), assigns risk bands, and determines EMRA (Energy Management & Resilience Assessment) eligibility.

**The core output**: An organization knows its energy volatility state and readiness for management intervention.

---

## Architecture Philosophy

**RAMP Knowledge Repository** = conceptual + ecosystem + strategic decisions  
**ESA-V3 GitHub Repo** = operational + executable + versioned code  

This boundary is intentional. RAMP grows with the entire ecosystem; this repo stays focused on ESA V3 mechanics and validation. Cross-linked, not duplicated.

---

## What's in This Repo

```
ESA-V3-02-03-2026-Latest/
├── README.md                    (this file)
├── CONTRIBUTING.md              (rules for PRs, naming, validation)
├── CHANGELOG.md                 (releases, breaking changes)
├── .github/
│   └── PULL_REQUEST_TEMPLATE.md (consistency for PRs)
├── docs/
│   ├── module-overview.md       (scope, boundaries, sub-industries)
│   ├── validation-workflow.md   (how to validate, record results)
│   ├── data-dictionary.md       (JSONL schema, field requirements)
│   └── module-checklist.md      (which modules complete, progress)
├── schemas/
│   └── vtp_schema.json          (validation schema, field defs)
├── data/
│   ├── reference/               (gold standard JSONL files)
│   └── templates/               (sample inputs for testing)
├── engine/
│   ├── engine.py                (VTP calculation, risk assignment)
│   ├── emra_rules.py            (EMRA eligibility logic)
│   └── measurement_readiness.py (readiness profile generation)
└── validation/
    ├── templates/               (validation result templates)
    └── results/                 (validation outcomes, one per sub-industry)
```

---

## Quick Start

### 1. Installation

```bash
git clone https://github.com/RelyAbility/ESA-V3-02-03-2026-Latest.git
cd ESA-V3-02-03-2026-Latest
pip install -r requirements.txt
```

### 2. Run a Validation Test

```bash
python -m engine.validate --industry "Bakery" --reference-file data/reference/bakery_reference.jsonl
```

**Expected Output**:
```
Validating: Bakery
Engine VTP: SV=58.4, CV=41.2, PV=63.1
Reference VTP: SV=58.4, CV=41.2, PV=63.1
Risk Band: Moderate
EMRA Eligibility: Eligible
Status: ✅ PASS
```

### 3. Calculate VTP for Your Data

```python
from engine import engine

assessment = {
    "q001": 75,
    "q002": 45,
    "q003": 82,
    # ... more question responses
}

vtp = engine.calculate_vtp(assessment, sub_industry="Bakery")
risk_band = engine.assign_risk_band(vtp)
emra_status = engine.determine_emra_eligibility(vtp, "Bakery")

print(f"VTP: {vtp}")
print(f"Risk Band: {risk_band['risk_band']}")
print(f"EMRA Eligibility: {emra_status['eligibility']}")
```

---

## Data Formats

### Input: Assessment Responses

Question-score pairs (0-100 scale):
```json
{
  "q001": 75,
  "q002": 45,
  "q003": 82
}
```

### Output: JSONL Reference Files

One JSON object per line:
```jsonl
{"sub_industry": "Bakery", "structural_volatility": 58.4, "constraint_volatility": 41.2, "performance_volatility": 63.1, "overall_volatility": 54.2, "risk_band": "Moderate", "emra_eligibility": "Eligible"}
{"sub_industry": "Dairy", "structural_volatility": 52.1, "constraint_volatility": 48.7, "performance_volatility": 55.3, ...}
```

See [data-dictionary.md](docs/data-dictionary.md) for complete schema.

---

## Validation Workflow

### For Contributors: Adding a New Sub-Industry

1. **Prepare reference data** - Collect industry data, calculate reference VTP
2. **Create JSONL file** - Place in `data/reference/[industry]_reference.jsonl`
3. **Run validation** - Execute validation script, record results
4. **Document results** - Update `docs/module-checklist.md` and `validation/results/[industry].md`
5. **Submit PR** - Follow [CONTRIBUTING.md](CONTRIBUTING.md)

See [validation-workflow.md](docs/validation-workflow.md) for detailed steps.

### For Operations: Current Status

**Validated (4/20)**: Bakery, Dairy, Meat, Plastics ✅  
**In Progress (4/20)**: Beverage, Paper Packaging, Flexible Packaging, Rigid Packaging  
**Planned (12/20)**: Metal Products, Chemicals, Mining, Facilities Management categories

Full tracking: [module-checklist.md](docs/module-checklist.md) | [RAMP Knowledge](https://github.com/RelyAbility/RAMP-Knowledge)

---

## Key Concepts

### Volatility Type Profile (VTP)

Three dimensions of volatility, each scored 0-100:

| Dimension | Meaning | Example Drivers |
|-----------|---------|----------------|
| **SV** | Structural Volatility | Business model, supply chain, market structure |
| **CV** | Constraint Volatility | Resource limits, regulatory constraints |
| **PV** | Performance Volatility | Energy consumption variation, yield variation |

**Overall VTP** = (SV + CV + PV) / 3

### Risk Bands

| Band | Score | Meaning |
|------|-------|---------|
| Low | 0-30 | Stable, predictable operations |
| Moderate | 31-60 | Manageable volatility |
| High | 61-80 | Significant volatility, ready for intervention |
| Critical | 81-100 | Extreme volatility, stabilization required |

### EMRA Eligibility

- **Eligible**: Ready for energy management programs immediately
- **Conditional**: Eligible with prerequisites (e.g., additional metering)
- **Not Eligible**: Not suitable at this time

---

## API Reference

See [module-overview.md](docs/module-overview.md) for complete engine function signatures.

### Core Functions

```python
engine.calculate_vtp(assessment_data, sub_industry)
→ {structural_volatility, constraint_volatility, performance_volatility, overall_volatility, confidence_level}

engine.assign_risk_band(vtp_scores)
→ {risk_band, overall_score, intervention_intensity, recommendation}

engine.determine_emra_eligibility(vtp_scores, sub_industry, additional_factors)
→ {eligibility, rationale, prerequisites, estimated_readiness_date}

engine.generate_measurement_readiness(sub_industry, current_infrastructure)
→ {current_state, maturity_level, gaps, priority_upgrades, timeline_months}
```

---

## Links to Knowledge Base

**📚 Full Knowledge Base** → [RAMP Knowledge Repository](https://github.com/RelyAbility/RAMP-Knowledge)

- [ESA V3 Complete Module](../ESA/V3/README.md) - Full strategic context
- [All Sub-Industry Modules](../ESA/V3/) - Deep dives into each industry category
- [Engine Documentation](../ESA/V3/Documentation/Engine_Documentation.md) - Complete technical specs
- [Validation Tracking](../ESA/V3/Validation_Tracking.md) - Live progress dashboard
- [Reference Data Status](../ESA/V3/Reference_Data/Reference_Data_Status.md) - JSONL file inventory

---

## Contributing

We welcome contributions! Please:

1. Read [CONTRIBUTING.md](CONTRIBUTING.md) - branching, naming, schema rules
2. Check [module-checklist.md](docs/module-checklist.md) - what's in progress
3. Follow the [PR template](.github/PULL_REQUEST_TEMPLATE.md)

Common contributions:
- Validate a new sub-industry module
- Improve engine efficiency
- Add test cases
- Propose new VTP refinements (link to RAMP for discussion)

---

## Support & Issues

- **Questions about engine logic?** → [Engine Documentation](docs/module-overview.md)
- **How do I validate a sub-industry?** → [Validation Workflow](docs/validation-workflow.md)
- **What sub-industries are done?** → [Module Checklist](docs/module-checklist.md)
- **Strategic direction?** → [RAMP Knowledge](https://github.com/RelyAbility/RAMP-Knowledge)
- **Bug reports / feature requests?** → [Open an Issue](https://github.com/RelyAbility/ESA-V3-02-03-2026-Latest/issues)

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for all releases, breaking changes, and validation milestones.

**Current Version**: 3.0.0 (Production Ready)  
**Last Updated**: March 2, 2026  
**Status**: 4/20 sub-industries validated, engine stable

---

## License

Proprietary. © RelyAbility. All rights reserved.

---

## Contact

- **Technical Questions**: Open an issue
- **RAMP Integration**: See [RAMP repo](https://github.com/RelyAbility/RAMP-Knowledge)
- **Product & Strategy**: Reference RAMP Knowledge repository

---

*ESA V3 is part of the RAMP ecosystem platform.*
