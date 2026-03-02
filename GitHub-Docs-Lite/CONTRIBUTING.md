# Contributing to ESA V3

**Rules for Contributions, PRs, & Sub-Industry Validation**

Thank you for contributing to ESA V3! This document outlines how to maintain consistency and discipline across the project.

---

## Core Principles

1. **Data Integrity**: Validation is non-negotiable. Reference data must be gold-standard quality.
2. **Schema Compliance**: All JSONL files, PRs, and outputs follow defined schemas.
3. **Clear Boundaries**: RAMP Knowledge owns conceptual depth; this repo owns operational execution.
4. **Documentation-First**: Every submission documents its intent and validation.

---

## Contribution Types

### Type 1: Validate a Sub-Industry

**Requirements**:
- Follow [validation-workflow.md](docs/validation-workflow.md) completely
- Collect 12+ months of facility data
- Achieve 100% match (±0.1% tolerance) between engine and reference
- Document all test scenarios
- Get sign-off from validation owner

**Deliverables**:
```
data/reference/[industry]_reference.jsonl        (JSONL file)
validation/results/[industry]/README.md           (Results document)
validation/results/[industry]/[industry]_assessment.json  (Test data)
docs/module-checklist.md                         (Updated with status)
```

**PR Template**: See [PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md)

---

### Type 2: Improve Engine Logic

**Requirements**:
- File GitHub issue first with detailed explanation
- Propose change with test cases
- Ensure backward compatibility
- Update CHANGELOG.md with notes

**Testing**:
```bash
# Run against all 4 validated sub-industries
python -m pytest tests/engine_validation_suite.py

# Result must be: ALL PASS
# ✅ Bakery: PASS
# ✅ Dairy: PASS
# ✅ Meat: PASS
# ✅ Plastics: PASS
```

**PR Process**:
1. Code review required
2. All tests pass
3. CHANGELOG updated
4. Merge only after approval

---

### Type 3: Add Test Cases or Utilities

**Requirements**:
- Test cases must cover edge cases
- Clear documentation of what's being tested
- No impact on engine logic (separate from Type 2)

**Deliverables**:
```
tests/test_[feature].py
tests/fixtures/[test_data].json
```

---

### Type 4: Documentation Updates

**Requirements**:
- Operational docs only (this repo)
- For conceptual content, add to RAMP Knowledge
- Maintain consistency with existing structure

**Scope**:
- ✅ How-to guides
- ✅ Troubleshooting
- ✅ API reference
- ❌ Industry context (goes in RAMP)
- ❌ Strategic rationale (goes in RAMP)

---

## Branching Convention

**Branch Naming**:

```
feature/validate-[industry]-v3-[version]
  → feature/validate-beverage-v3-0-0

feature/[feature-name]-v3-[version]
  → feature/improve-vtp-calculation-v3-0-1

fix/[issue-name]
  → fix/risk-band-threshold-calculation

docs/[change-description]
  → docs/clarify-emra-eligibility-rules
```

**Branching From**: Always branch from `main`

---

## Pull Request Process

### 1. Prepare Your Branch

```bash
git checkout -b feature/validate-beverage-v3-0-0
# ... make changes ...
git add [files]
git commit -m "Commit message (see below)"
git push origin feature/validate-beverage-v3-0-0
```

### 2. Commit Message Format

**For Validation PRs**:

```
Validate [Industry]: VTP scores, risk band assignment, EMRA eligibility

Reference file: [industry]_reference.jsonl
VTP Scores: SV=XX.X, CV=XX.X, PV=XX.X, Overall=XX.X
Risk Band: [Low/Moderate/High/Critical]
EMRA Eligibility: [Eligible/Conditional/Not Eligible]
Match Rate: 100% ± 0.1%
Test Scenarios: [Number] facilities
Signed Off: [Name]

This PR adds validation for [Industry] with gold-standard reference
data, achieving 100% match on all VTP dimensions. See validation/
results/[industry]/README.md for detailed test results.
```

**For Engine Changes**:

```
[Fix|Improve] [What changed]: [Why it matters]

Details:
- What changed
- Why it's needed
- Test coverage
- Breaking changes (if any)

Tested against: All 4 validated sub-industries
Result: ✅ PASS
```

**For Documentation**:

```
Docs: [What changed]

Updated:
- [doc_name]: [specific section]
- [doc_name]: [specific section]

No operational impact.
```

### 3. Create PR with Template

Use [PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md). It includes:

- **Change Type**: Validation / Engine / Tests / Docs
- **Description**: What and why
- **Validation Checklist**: Specific to change type
- **Testing**: How it was tested
- **Reviewers**: Who should approve
- **Related Issues**: Link to discussions

### 4. Code Review

**Reviewers will check**:
- Schema compliance (all fields present and valid)
- Validation completeness (all test scenarios covered)
- Documentation accuracy (examples work, no dead links)
- Backward compatibility (no breaking changes without warning)

**Review SLA**:
- Simple PRs: 2-3 days
- Complex PRs: 5-7 days
- Blocking issues: immediate escalation

### 5. Merge Criteria

- ✅ All automated checks pass
- ✅ Code review approval
- ✅ Validation sign-off (if applicable)
- ✅ CHANGELOG entry
- ✅ No conflicts with main

---

## Schema Compliance

### JSONL Reference Files

**Requirements**:
- All required fields present
- Valid JSON on each line
- UTF-8 encoding
- Valid numeric ranges (0-100 for scores)
- Updated `docs/module-checklist.md` reflects status

**Validation Script**:

```bash
# Validate your reference file before submitting
python -m schemas.validate_jsonl data/reference/[industry]_reference.jsonl

# Output must be:
# ✅ [industry]_reference.jsonl: VALID
# - 1 records validated
# - All required fields present
# - All numeric values in valid range
```

### VTP Output Schema

Engine output must include all fields:

```json
{
  "structural_volatility": float,
  "constraint_volatility": float,
  "performance_volatility": float,
  "overall_volatility": float,
  "confidence_level": float
}
```

Missing any field → PR blocked.

---

## Naming Conventions

### Sub-Industry Identifiers

Use **consistent naming**:

```
Bakery                  (Display name)
bakery                  (Folder name, file prefix)
BAK                     (Short code)
```

**NO**:
- ❌ Baking Industry
- ❌ Bakeries
- ❌ bakeries (plural)
- ❌ BAKERY (all caps in folder names)

### File Names

```
[industry]_reference.jsonl      ✅
[industry]_assessment.json      ✅
[industry]_validation_result.md ✅

bakery_reference.jsonl          ✅ (lowercase, no spaces)
bakery_REFERENCE.jsonl          ❌ (inconsistent casing)
bakery reference.jsonl          ❌ (spaces)
```

### Branch Names

```
feature/validate-bakery-v3-0-0          ✅ (lowercase, hyphens)
feature/validate_bakery_v3_0_0          ❌ (underscores)
feature/VALIDATE-BAKERY-V3-0-0          ❌ (all caps)
```

---

## Validation Acceptance Criteria

**For all sub-industry submissions**:

- [ ] VTP calculation matches reference ±0.1%
- [ ] Risk band assignment correct per rules
- [ ] EMRA eligibility determination accurate
- [ ] Test scenarios documented (3 facilities minimum)
- [ ] Results in `validation/results/[industry]/`
- [ ] `docs/module-checklist.md` updated
- [ ] Sign-off from validation owner
- [ ] PR follows template
- [ ] Commit message follows format
- [ ] No conflicts with main

**Failure to meet any criteria** → PR request changes.

---

## Testing Requirements

### Unit Tests

If you modify engine logic:

```bash
pytest tests/engine_validation_suite.py -v

# Must pass for:
# - test_calculate_vtp_bakery
# - test_calculate_vtp_dairy
# - test_calculate_vtp_meat
# - test_calculate_vtp_plastics
# - test_risk_band_assignment
# - test_emra_eligibility_rules
```

### Validation Tests

For sub-industry validation:

```bash
# Provided test harness
python -m engine.validate --industry "[Industry]" \
  --reference-file data/reference/[industry]_reference.jsonl \
  --assessment-file validation/data/[industry]_assessment.json

# Output must be:
# ✅ [Industry] VALIDATION PASSED
# Match rate: 100% (±0.1% tolerance)
```

---

## Common Issues & Solutions

### "PR rejected: VTP doesn't match reference"

1. Check question mappings (all 198 included?)
2. Verify assessment input data is identical to reference
3. Check for sub-industry-specific weighting
4. Run debug script: `python -m engine.debug_vtp --industry "[Industry]"`
5. Document findings in PR

### "PR rejected: Schema invalid"

1. Validate with: `python -m schemas.validate_jsonl [file]`
2. Ensure all required fields present
3. Check numeric ranges (0-100 for VTP)
4. Verify JSON format (no trailing commas, etc.)

### "PR rejected: Documentation incomplete"

1. Create/update `validation/results/[industry]/README.md`
2. Update `docs/module-checklist.md` with status
3. Link to results in PR description
4. Include sign-off timeline

### "PR rejected: Commit message format"

Use template from above. Must include:
- Change type (Validate / Fix / Improve / Docs)
- What changed and why
- Validation/test results
- Sign-off (if applicable)

---

## Code Style & Standards

### Python

- Python 3.8+
- Follow [PEP 8](https://pep8.org/)
- Type hints required for public functions
- Docstrings for all functions

```python
def calculate_vtp(assessment_data: dict, sub_industry: str) -> dict:
    """Calculate VTP from assessment responses.
    
    Args:
        assessment_data: {q001: 75, q002: 45, ...}
        sub_industry: Industry identifier ("Bakery", etc.)
        
    Returns:
        {structural_volatility, constraint_volatility, ...}
        
    Raises:
        ValueError: If assessment incomplete or sub_industry unknown
    """
```

### JSON/JSONL

- UTF-8 encoding
- Consistent field ordering
- Numeric precision: 1 decimal for VTP scores
- ISO 8601 dates

```jsonl
{"sub_industry": "Bakery", "structural_volatility": 58.4, ...}
```

### Markdown

- Consistent heading hierarchy (H1 → H2 → H3)
- Code blocks with language specified
- Tables for structured data
- Links to RAMP Knowledge where appropriate

---

## Questions?

- **How do I validate a sub-industry?** → [validation-workflow.md](docs/validation-workflow.md)
- **What's the schema?** → [data-dictionary.md](docs/data-dictionary.md)
- **How does the engine work?** → [module-overview.md](docs/module-overview.md)
- **Strategic context?** → [RAMP Knowledge](https://github.com/RelyAbility/RAMP-Knowledge/tree/main/ESA/V3)

---

## Questions or Issues?

1. Check the docs above
2. Search [existing issues](https://github.com/RelyAbility/ESA-V3-02-03-2026-Latest/issues)
3. Open a new issue with details
4. Link to relevant RAMP Knowledge articles

---

## License & Ownership

By submitting a PR, you agree:
- Code/data contributions are proprietary (© RelyAbility)
- All validation work is owned by the project
- Changes may be modified or merged as needed

---

*Last Updated: March 2, 2026*  
*Version: 3.0.0*
