# ESA V3 Validation Workflow

**How to Validate a Sub-Industry Module & Record Results**

This document describes the operational process for validating new sub-industries in ESA V3.

---

## Overview

Validation confirms that the ESA V3 engine produces correct VTP scores for a given sub-industry. It requires:

1. **Reference data** (gold standard JSONL file)
2. **Engine run** (compute VTP on same inputs)
3. **Output comparison** (verify match to reference)
4. **Results documentation** (record outcome)
5. **PR submission** (follow contribution guidelines)

**Timeline**: ~2-4 weeks per sub-industry (data prep → execution → documentation)

---

## Step-by-Step Workflow

### Phase 1: Preparation (Weeks 1-2)

#### 1.1 Data Collection

- Identify 3-5 representative facilities in target sub-industry
- Gather 12+ months of operational data
- Collect assessment responses (if available) or conduct interviews
- Document data sources and assumptions

**Deliverable**: `validation/data-collection-[industry].md` with source inventory

#### 1.2 Create Reference JSONL

Calculate reference Volatility Type Profile for the sub-industry:

```python
# Example calculation
reference_vtp = {
    "sub_industry": "Bakery",
    "facility_id": "BAK_REF_001",
    "structural_volatility": 58.4,
    "constraint_volatility": 41.2,
    "performance_volatility": 63.1,
    "overall_volatility": 54.2,
    "risk_band": "Moderate",
    "emra_eligibility": "Eligible",
    "questions_used": 198,
    "data_collection_year": 2025,
    "validation_status": "approved"
}
```

Save to: `data/reference/[industry]_reference.jsonl`

**Validation Rule**: All required fields must be populated; VTP scores 0-100.

### Phase 2: Engine Validation (Week 3)

#### 2.1 Prepare Assessment Input

Construct assessment input object using the same facility data:

```python
assessment = {
    "q001": 75,
    "q002": 45,
    "q003": 82,
    # ... all 198 questions
}
```

#### 2.2 Run Engine

```bash
# Command-line validation
python -m engine.validate --industry "Bakery" --reference-file data/reference/bakery_reference.jsonl --assessment-file validation/data/bakery_assessment.json

# Python API
from engine import engine

reference = load_jsonl("data/reference/bakery_reference.jsonl")
vtp = engine.calculate_vtp(assessment, "Bakery")
risk = engine.assign_risk_band(vtp)
emra = engine.determine_emra_eligibility(vtp, "Bakery")

# Compare
match = compare_outputs(vtp, reference)
```

#### 2.3 Verify Match

**Acceptance Criteria**:
- ✅ Structural Volatility: Match within ±0.1%
- ✅ Constraint Volatility: Match within ±0.1%
- ✅ Performance Volatility: Match within ±0.1%
- ✅ Overall Volatility: Match within ±0.1%
- ✅ Risk Band: Correct classification
- ✅ EMRA Eligibility: Correct determination

**If match fails**:
1. Check question mappings (SV/CV/PV classification correct?)
2. Verify sub-industry weighting applied correctly
3. Review assessment input data
4. Debug engine logic if needed
5. Document discrepancy and resolution

### Phase 3: Documentation (Week 3-4)

#### 3.1 Create Validation Results Document

Create `validation/results/[industry]/README.md`:

```markdown
# [Industry] Validation Results

**Status**: ✅ VALIDATED  
**Validation Date**: March 15, 2026  
**Reference VTP**: SV=58.4, CV=41.2, PV=63.1  
**Engine VTP**: SV=58.4, CV=41.2, PV=63.1  
**Match**: 100% ✅

## Test Scenarios

- [x] Scenario 1: Typical facility, mid-range volatility
- [x] Scenario 2: Low-volatility facility
- [x] Scenario 3: High-volatility facility

## Observations

- Engine performed consistent across test range
- Risk band assignments correct per rules
- EMRA eligibility determinations accurate

## Sign-Off

Validated by: Brad Parsons  
Date: March 15, 2026
```

#### 3.2 Update Module Checklist

Update `docs/module-checklist.md` with validation status:

```markdown
### [Industry]
- [x] Reference JSONL file generated
- [x] JSONL structure validated
- [x] Sample data loaded into engine
- [x] VTP scores computed
- [x] Scores compared to reference VTP
- [x] 100% match verification
- [x] Risk band assignments validated
- [x] EMRA eligibility determined correctly
- [x] Measurement readiness profile generated
- [x] Documentation updated
- [x] Test cases documented
- [x] Sign-off from validation owner

**Status**: ✅ VALIDATED (March 15, 2026)
```

#### 3.3 Archive Reference Data

- Copy reference JSONL to `data/reference/archive/[date]/` (for historical tracking)
- Document any sub-industry-specific weighting or rules in `validation/results/[industry]/weighting-notes.md`

### Phase 4: Submission (Week 4)

#### 4.1 Create Feature Branch

```bash
git checkout -b feature/validate-[industry]-v3-0-0
```

#### 4.2 Commit Changes

```bash
git add data/reference/[industry]_reference.jsonl
git add validation/results/[industry]/
git add docs/module-checklist.md
git commit -m "Validate [Industry]: VTP $scores, Risk=$band, EMRA=$status

Reference file: [industry]_reference.jsonl
Test scenarios: 3 facilities tested
Match rate: 100%
Signed off: Brad Parsons"
```

#### 4.3 Submit PR

Follow [CONTRIBUTING.md](../CONTRIBUTING.md) template. Include:
- Link to validation results document
- Summary of test scenarios
- Sign-off confirmation
- Link to RAMP Knowledge if strategic notes exist

---

## Key Templates

### Validation Results Template

`validation/results/[industry]/README.md`:

```markdown
# [Industry] Validation Results

**Status**: ✅ VALIDATED  
**Validation Date**: YYYY-MM-DD  
**Validator**: Name

## Reference Data

| Metric | Value |
|--------|-------|
| Structural Volatility | X.X |
| Constraint Volatility | X.X |
| Performance Volatility | X.X |
| Overall Volatility | X.X |
| Risk Band | [Low/Moderate/High/Critical] |
| EMRA Eligibility | [Eligible/Conditional/Not Eligible] |

## Engine Output

[Same as reference]

## Match Verification

- [ ] SV Match (±0.1%)
- [ ] CV Match (±0.1%)
- [ ] PV Match (±0.1%)
- [ ] Overall Match (±0.1%)
- [ ] Risk Band Correct
- [ ] EMRA Eligibility Correct

## Test Scenarios

1. **Scenario 1**: [Facility type], [Volatility profile]
   - Result: [Pass/Fail]

2. **Scenario 2**: ...

## Notes

[Observations, deviations, special handling]

## Sign-Off

Validated by: [Name]  
Reviewed by: [Optional]  
Date: YYYY-MM-DD
```

### Discrepancy Log Template

If validation fails:

`validation/results/[industry]/DISCREPANCIES.md`:

```markdown
# Discrepancies Found

## Issue 1: [VTP doesn't match]

**Expected**: SV=58.4, CV=41.2  
**Got**: SV=58.3, CV=41.1  
**Difference**: -0.1% (within tolerance) ✅

## Issue 2: [Sub-industry weighting]

**Finding**: Custom weighting required for [constraint type]  
**Resolution**: Applied [weighting formula]  
**Verification**: Re-run confirms match ✅

## Resolution Summary

All discrepancies resolved. Validation PASS.
```

---

## Troubleshooting

### "Engine output doesn't match reference"

1. **Check question classification**: Are all 198 questions mapped to correct SV/CV/PV?
2. **Verify assessment input**: Is the input data identical to reference calculation?
3. **Sub-industry weighting**: Has custom weighting been applied?
4. **Engine version**: Are you using the latest engine code?

**Debug steps**:
```python
# Print question breakdown
output = engine.debug_calculate_vtp(assessment, "Bakery")
# {q001: 75 → SV, q002: 45 → CV, ...}

# Check dimension scores
print(f"SV questions: {output['sv_questions']}")
print(f"CV questions: {output['cv_questions']}")
print(f"PV questions: {output['pv_questions']}")
```

### "EMRA eligibility determine is wrong"

1. Check [CONTRIBUTING.md](../CONTRIBUTING.md) for EMRA rules
2. Verify additional_factors parameter passed (measurement infrastructure, financial status, etc.)
3. Review eligibility rules in `engine/emra_rules.py`

### "Module checklist is incomplete"

Review each checkbox requirement:
- Is reference JSONL valid JSON Lines format?
- Have test scenarios been documented?
- Did sign-off validation owner confirm?

---

## Validation Cadence

- **Q1 2026**: Validate Beverage, Paper Packaging, Flexible Packaging, Rigid Packaging + 2 Metal Products
- **Q2 2026**: Continue Metal Products, Chemicals, Pharmaceuticals
- **Q3 2026**: Complete Mining & Primary Metals, Facilities Management
- **Q4 2026+**: Continuous validation, improvements, monitoring

---

## Escalation Path

**Issue**: Reference data can't be collected  
**Action**: Document blockers, escalate to RAMP [open issue](https://github.com/RelyAbility/ESA-V3-02-03-2026-Latest/issues)

**Issue**: Engine logic seems wrong  
**Action**: Create detailed bug report, include test case

**Issue**: Sub-industry rules unclear  
**Action**: Reference [RAMP Knowledge](https://github.com/RelyAbility/RAMP-Knowledge/tree/main/ESA/V3) for context, link to discussion

---

## Additional Resources

- [Module Checklist](module-checklist.md) - Tracking across all sub-industries
- [Data Dictionary](data-dictionary.md) - Field definitions
- [RAMP Validation Tracking](../../ESA/V3/Validation_Tracking.md) - Strategic progress
- [Contributing Guidelines](../CONTRIBUTING.md) - PR rules and conventions

---

*Last Updated: March 2, 2026*
