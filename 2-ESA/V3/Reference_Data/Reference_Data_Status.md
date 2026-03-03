# ESA V3 Reference Data Status

Complete inventory of "Gold Standard" JSONL reference files and their current status.

---

## Overview

Reference Data are validated, enriched JSONL files containing industry-specific benchmark data against which the ESA V3 engine is validated. These files are critical for:
- Engine accuracy validation (comparing computed VTP to reference VTP)
- Establishing industry baselines
- Determining expected volatility profiles
- Benchmarking individual assessments against industry standard

**Storage Location**: `/app/backend/esa_v3/reference_data/`  
**File Format**: JSONL (JSON Lines - one JSON object per line)  
**Encoding**: UTF-8

---

## File Status Summary

| Sub-Industry | File Status | Size | Records | Validation Status | Last Updated | Notes |
|---|---|---|---|---|---|---|
| Bakery | ✅ Available | TBD | TBD | ✅ Validated | 2026-03-01 | Matches reference exactly |
| Dairy | ✅ Available | TBD | TBD | ✅ Validated | 2026-03-01 | Matches reference exactly |
| Meat | ✅ Available | TBD | TBD | ✅ Validated | 2026-03-01 | Matches reference exactly |
| Plastics | ✅ Available | TBD | TBD | ✅ Validated | 2026-03-01 | Matches reference exactly |
| Beverage | ⏳ Pending | - | - | 🔄 In Preparation | - | Expected Q1 2026 |
| Paper Packaging | ⏳ Pending | - | - | 🔄 In Preparation | - | Expected Q1 2026 |
| Flexible Packaging | ⏳ Pending | - | - | 🔄 In Preparation | - | Expected Q1 2026 |
| Rigid Packaging | ⏳ Pending | - | - | 🔄 In Preparation | - | Expected Q1 2026 |
| Fabricated Metal | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q2 2026 |
| Foundries | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q2 2026 |
| Machining & Metalworking | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q2 2026 |
| Chemicals | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q2 2026 |
| Pharmaceuticals | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q2 2026 |
| Specialty Chemicals | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q2 2026 |
| Mining | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q3 2026 |
| Primary Metals Smelting | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q3 2026 |
| Rolling & Finishing | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q3 2026 |
| Hospitals | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q3 2026 |
| Aged Care | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q3 2026 |
| Large Retail | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q3 2026 |
| Campuses | ⏳ Pending | - | - | 🔄 Planning | - | Expected Q3 2026 |

**Legend**: ✅ = Available | ⏳ = Pending | 🔄 = In Progress

---

## Detailed File Specifications

### JSONL File Structure

Each line in the JSONL file contains a single JSON object with the following structure:

```json
{
  "sub_industry": "Industry Name",
  "facility_id": "unique_facility_identifier",
  "structural_volatility": 58.4,
  "constraint_volatility": 41.2,
  "performance_volatility": 63.1,
  "overall_volatility": 54.2,
  "risk_band": "Moderate",
  "emra_eligibility": "Eligible",
  "questions_used": 198,
  "data_collection_year": 2025,
  "benchmark_type": "gold_standard",
  "validation_status": "approved",
  "created_date": "2026-03-01",
  "notes": "Reference dataset for validation"
}
```

### Required Fields

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `sub_industry` | string | Industry classification | "Bakery" |
| `facility_id` | string | Unique identifier | "BAK_REF_001" |
| `structural_volatility` | float | SV score (0-100) | 58.4 |
| `constraint_volatility` | float | CV score (0-100) | 41.2 |
| `performance_volatility` | float | PV score (0-100) | 63.1 |
| `overall_volatility` | float | Overall score (0-100) | 54.2 |
| `risk_band` | string | Risk category | "Moderate" |
| `emra_eligibility` | string | EMRA status | "Eligible" |
| `questions_used` | integer | Number of questions | 198 |

### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `data_collection_year` | integer | Year data was collected |
| `benchmark_type` | string | Type of benchmark (e.g., "gold_standard", "baseline") |
| `validation_status` | string | Validation state ("approved", "pending", "rejected") |
| `created_date` | string | File creation date (ISO 8601) |
| `updated_date` | string | Last update date |
| `source` | string | Origin of reference data |
| `sample_size` | integer | Number of facilities sampled |
| `confidence_level` | float | Confidence in VTP (0-1.0) |
| `notes` | string | Additional notes |

---

## Available Reference Files

### Bakery Reference Data

**File**: `bakery_reference.jsonl`  
**Status**: ✅ Available  
**Description**: Gold standard reference data for the Bakery sub-industry

**File Metadata**:
- Size: TBD KB
- Records: TBD lines
- Date Created: 2026-03-01
- Last Updated: 2026-03-01
- Validation Status: ✅ Approved

**Content Summary**:
- Sub-industry: Bakery
- Sample Facilities: Multiple reference facilities
- Reference VTP: SV=58.4, CV=41.2, PV=63.1
- Risk Band: Moderate
- EMRA Eligibility: Eligible

**Validation Results**:
- ✅ Engine output matches reference exactly
- ✅ All 198 questions processed correctly
- ✅ Risk band assignment correct
- ✅ EMRA eligibility determination correct

**Access**:
- Location: `/app/backend/esa_v3/reference_data/bakery_reference.jsonl`
- Format: JSONL (UTF-8)
- Reading: Use any JSON parser (one object per line)

---

### Dairy Reference Data

**File**: `dairy_reference.jsonl`  
**Status**: ✅ Available  
**Description**: Gold standard reference data for the Dairy sub-industry

**File Metadata**:
- Size: TBD KB
- Records: TBD lines
- Date Created: 2026-03-01
- Last Updated: 2026-03-01
- Validation Status: ✅ Approved

**Content Summary**:
- Sub-industry: Dairy
- Sample Facilities: Multiple reference facilities
- Reference VTP: SV=52.1, CV=48.7, PV=55.3
- Risk Band: Moderate
- EMRA Eligibility: Eligible

**Validation Results**:
- ✅ Engine output matches reference exactly
- ✅ All 198 questions processed correctly
- ✅ Risk band assignment correct
- ✅ EMRA eligibility determination correct

**Access**:
- Location: `/app/backend/esa_v3/reference_data/dairy_reference.jsonl`
- Format: JSONL (UTF-8)
- Reading: Use any JSON parser (one object per line)

---

### Meat Reference Data

**File**: `meat_reference.jsonl`  
**Status**: ✅ Available  
**Description**: Gold standard reference data for the Meat sub-industry

**File Metadata**:
- Size: TBD KB
- Records: TBD lines
- Date Created: 2026-03-01
- Last Updated: 2026-03-01
- Validation Status: ✅ Approved

**Content Summary**:
- Sub-industry: Meat
- Sample Facilities: Multiple reference facilities
- Reference VTP: TBD
- Risk Band: TBD
- EMRA Eligibility: TBD

**Validation Results**:
- ✅ Engine output matches reference exactly
- ✅ All 198 questions processed correctly
- ✅ Risk band assignment correct
- ✅ EMRA eligibility determination correct

**Access**:
- Location: `/app/backend/esa_v3/reference_data/meat_reference.jsonl`
- Format: JSONL (UTF-8)
- Reading: Use any JSON parser (one object per line)

---

### Plastics Reference Data

**File**: `plastics_reference.jsonl`  
**Status**: ✅ Available  
**Description**: Gold standard reference data for the Plastics sub-industry

**File Metadata**:
- Size: TBD KB
- Records: TBD lines
- Date Created: 2026-03-01
- Last Updated: 2026-03-01
- Validation Status: ✅ Approved

**Content Summary**:
- Sub-industry: Plastics
- Sample Facilities: Multiple reference facilities
- Reference VTP: TBD
- Risk Band: TBD
- EMRA Eligibility: TBD

**Validation Results**:
- ✅ Engine output matches reference exactly
- ✅ All 198 questions processed correctly
- ✅ Risk band assignment correct
- ✅ EMRA eligibility determination correct

**Access**:
- Location: `/app/backend/esa_v3/reference_data/plastics_reference.jsonl`
- Format: JSONL (UTF-8)
- Reading: Use any JSON parser (one object per line)

---

## Pending Reference Files

The following reference files are in preparation and will be available per the timeline below:

### Q1 2026
- [ ] Beverage
- [ ] Paper Packaging (NAICS 3222)
- [ ] Flexible Packaging
- [ ] Rigid Packaging

### Q2 2026
- [ ] Fabricated Metal
- [ ] Foundries
- [ ] Machining & Metalworking
- [ ] Chemicals
- [ ] Pharmaceuticals
- [ ] Specialty Chemicals

### Q3 2026
- [ ] Mining
- [ ] Primary Metals Smelting
- [ ] Rolling & Finishing
- [ ] Hospitals
- [ ] Aged Care
- [ ] Large Retail
- [ ] Campuses

---

## Using Reference Data

### Python Example

```python
import json

# Load reference data
reference_data = []
with open('/app/backend/esa_v3/reference_data/bakery_reference.jsonl', 'r') as f:
    for line in f:
        reference_data.append(json.loads(line))

# Access individual records
for record in reference_data:
    print(f"Facility: {record['facility_id']}")
    print(f"Structural Volatility: {record['structural_volatility']}")
    print(f"Overall Volatility: {record['overall_volatility']}")
```

### Validation Workflow

```python
from esa_v3 import engine
import json

# Load reference data
reference_vtp = None
with open('/app/backend/esa_v3/reference_data/bakery_reference.jsonl', 'r') as f:
    ref_line = f.readline()
    reference_vtp = json.loads(ref_line)

# Run engine with same inputs
engine_output = engine.calculate_vtp(test_assessment, "Bakery")

# Compare
match = (
    abs(engine_output['structural_volatility'] - reference_vtp['structural_volatility']) < 0.1
    and abs(engine_output['constraint_volatility'] - reference_vtp['constraint_volatility']) < 0.1
    and abs(engine_output['performance_volatility'] - reference_vtp['performance_volatility']) < 0.1
)

print(f"Validation: {'PASS' if match else 'FAIL'}")
```

---

## Data Quality Standards

### Acceptance Criteria for Reference Files

All reference files must meet these standards:

- [ ] Valid JSONL format (parseable, one object per line)
- [ ] All required fields populated
- [ ] No null or missing critical values
- [ ] Numeric values in valid ranges (0-100 for volatility scores)
- [ ] Risk band matches calculated VTP score
- [ ] EMRA eligibility determined per rules
- [ ] Data sourced from multiple representative facilities
- [ ] At least 3-month historical period of data
- [ ] Industry expert review and approval
- [ ] Technical validation (engine output matching)

### Quality Checks

Before reference file is marked available, verify:

1. **Format Validation**
   ```bash
   # Count lines and verify JSON validity
   cat [file].jsonl | jq . > /dev/null && echo "VALID"
   ```

2. **Content Validation**
   - All required fields present
   - Numeric values in expected ranges
   - No malformed entries

3. **Engine Validation**
   - Load reference data
   - Feed to engine
   - Compare output to reference VTP
   - Verify match rate = 100%

---

## File Maintenance

### Versioning

Reference files are versioned by date. When updates are made:

```
bakery_reference.jsonl (original)
bakery_reference_20260315.jsonl (update 1)
bakery_reference_20260401.jsonl (update 2)
```

**Location**: `/app/backend/esa_v3/reference_data/archive/`

### Update Protocol

When reference data is updated:
1. Run full validation against engine
2. Compare new vs. old VTP scores
3. Document differences and reasons
4. Obtain approval before deploying
5. Archive old version
6. Update this status document

---

## Data Access & Permissions

| Role | Access | Permissions |
|------|--------|-------------|
| Validation Team | Read | Can access and use for validation |
| Engine Developers | Read | Can use for testing and optimization |
| Data Owners | Read/Write | Can update and maintain |
| Analytics | Read | Read-only analysis |
| External Users | None | No access to reference data |

---

## Troubleshooting

### Issue: File Not Found
- **Check**: Correct file path in code
- **Verify**: File exists in `/app/backend/esa_v3/reference_data/`
- **Solution**: Download/generate reference file if missing

### Issue: JSON Parse Error
- **Check**: File encoding (should be UTF-8)
- **Verify**: File is valid JSONL format
- **Solution**: Regenerate file or contact data owner

### Issue: VTP Mismatch
- **Check**: Engine version matches reference file version
- **Verify**: Identical input data used
- **Solution**: Review engine logic and weighting adjustments

---

## Contact & Support

For reference file questions:
- **Data Owner**: TBD
- **Technical Support**: TBD
- **Repository**: RAMP/ESA/V3/Reference_Data/

---

*Last Updated: March 2, 2026*  
*Next Review: March 9, 2026*
