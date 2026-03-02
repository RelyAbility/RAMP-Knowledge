# ESA V3 Data Dictionary

**Complete Field Definitions & Schema Reference**

---

## Assessment Input Format

### Question Response Object

Individual assessment responses. Each question scored 0-100.

```json
{
  "q001": 75,
  "q002": 45,
  "q003": 82,
  "q004": 60,
  "...": "..."
}
```

| Field | Type | Range | Required | Notes |
|-------|------|-------|----------|-------|
| `q[001-198]` | integer | 0-100 | Yes | Question response score |

**Validation Rules**:
- All 198 questions required (no nulls)
- Each score must be 0-100 integer
- No negative values or >100 values

**Schema File**: `schemas/assessment_schema.json`

---

## VTP Output Format

### Volatility Type Profile

Machine-readable VTP calculation result.

```json
{
  "structural_volatility": 58.4,
  "constraint_volatility": 41.2,
  "performance_volatility": 63.1,
  "overall_volatility": 54.2,
  "confidence_level": 0.95
}
```

| Field | Type | Range | Meaning |
|-------|------|-------|---------|
| `structural_volatility` | float | 0-100 | SV score (business model/supply chain volatility) |
| `constraint_volatility` | float | 0-100 | CV score (resource constraint volatility) |
| `performance_volatility` | float | 0-100 | PV score (operational performance volatility) |
| `overall_volatility` | float | 0-100 | Average of SV, CV, PV |
| `confidence_level` | float | 0-1.0 | Confidence in scores (based on question coverage) |

**Calculation**:
```
overall_volatility = (SV + CV + PV) / 3
confidence_level = (questions_answered / 198) * base_confidence
```

---

## Risk Band Format

### Risk Band Assignment

Translation of VTP to actionable risk category.

```json
{
  "risk_band": "Moderate",
  "overall_score": 54.2,
  "intervention_intensity": "Standard",
  "recommendation": "Implement standard energy management program"
}
```

| Field | Type | Values | Notes |
|-------|------|--------|-------|
| `risk_band` | string | "Low" \| "Moderate" \| "High" \| "Critical" | Risk category |
| `overall_score` | float | 0-100 | VTP overall score |
| `intervention_intensity` | string | "Light" \| "Standard" \| "Intensive" \| "Crisis" | Recommended program intensity |
| `recommendation` | string | Text | Brief recommendation text |

**Risk Band Thresholds**:
- **Low**: 0-30 (stable, predictable)
- **Moderate**: 31-60 (manageable volatility)
- **High**: 61-80 (significant volatility, intervention-ready)
- **Critical**: 81-100 (extreme volatility, stabilization required)

---

## EMRA Eligibility Format

### Energy Management & Resilience Assessment Eligibility

Determination of readiness for EMRA program enrollment.

```json
{
  "eligibility": "Eligible",
  "rationale": "Overall volatility within acceptable range with adequate measurement infrastructure",
  "prerequisites": [],
  "estimated_readiness_date": null,
  "next_assessment_date": "2026-06-01",
  "confidence": 0.92
}
```

| Field | Type | Values | Notes |
|-------|------|--------|-------|
| `eligibility` | string | "Eligible" \| "Conditional" \| "Not Eligible" | Enrollment status |
| `rationale` | string | Text (100-500 chars) | Explanation of determination |
| `prerequisites` | array | [list of strings] | Required actions (if Conditional) |
| `estimated_readiness_date` | string | ISO date or null | When prerequisites complete (if Conditional) |
| `next_assessment_date` | string | ISO date | When to re-assess |
| `confidence` | float | 0-1.0 | Confidence in determination |

**Eligibility Logic** (simplified):
- **Eligible**: VTP ≤ 75 AND has measurement infrastructure
- **Conditional**: VTP 61-80 AND missing prerequisites
- **Not Eligible**: VTP ≥ 81 OR critical blockers

See [RAMP Engine Documentation](../../ESA/V3/Documentation/Engine_Documentation.md) for complete rules.

---

## Measurement Readiness Profile Format

### Infrastructure & Capability Assessment

Readiness for accurate energy measurement and monitoring.

```json
{
  "current_state": "Partial - main meter only",
  "maturity_level": 2,
  "identified_gaps": [
    "No sub-metering for major end-uses",
    "Manual data collection only",
    "No historical trend capability"
  ],
  "priority_upgrades": [
    {
      "upgrade": "Install HVAC sub-metering",
      "priority": "High",
      "estimated_cost_range": "$5K-$15K"
    },
    {
      "upgrade": "Deploy automated data logging system",
      "priority": "High",
      "estimated_cost_range": "$10K-$30K"
    }
  ],
  "estimated_timeline_months": 4
}
```

| Field | Type | Values | Notes |
|-------|------|--------|-------|
| `current_state` | string | Text (50-200 chars) | Qualitative description |
| `maturity_level` | integer | 1-5 | 1=minimal, 5=fully instrumented |
| `identified_gaps` | array | [list of strings] | Missing capabilities |
| `priority_upgrades` | array | [upgrade objects] | Recommended improvements |
| `estimated_timeline_months` | integer | 1-36 | Time to full readiness |

**Maturity Levels**:
- **1**: No metering beyond main utility meter
- **2**: Main meter + minimal sub-metering
- **3**: Partial sub-metering, some automation
- **4**: Comprehensive sub-metering, automated logging
- **5**: Real-time monitoring, advanced analytics ready

---

## Reference Data (JSONL) Format

### Gold Standard Validation Files

One JSON object per line. Used for engine validation.

```jsonl
{"sub_industry": "Bakery", "facility_id": "BAK_REF_001", "structural_volatility": 58.4, "constraint_volatility": 41.2, "performance_volatility": 63.1, "overall_volatility": 54.2, "risk_band": "Moderate", "emra_eligibility": "Eligible", "questions_used": 198, "data_collection_year": 2025, "sample_size": 47, "validation_status": "approved", "created_date": "2026-03-01"}
{"sub_industry": "Bakery", "facility_id": "BAK_REF_002", ...}
```

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `sub_industry` | string | Yes | Industry classification |
| `facility_id` | string | Yes | Unique facility identifier |
| `structural_volatility` | float | Yes | SV score (0-100) |
| `constraint_volatility` | float | Yes | CV score (0-100) |
| `performance_volatility` | float | Yes | PV score (0-100) |
| `overall_volatility` | float | Yes | Average VTP (0-100) |
| `risk_band` | string | Yes | Risk classification |
| `emra_eligibility` | string | Yes | Eligibility determination |
| `questions_used` | integer | Yes | Should be 198 |
| `data_collection_year` | integer | Yes | Year data collected |
| `sample_size` | integer | No | Number of facilities sampled |
| `validation_status` | string | Yes | "approved", "pending", "rejected" |
| `created_date` | string | Yes | ISO 8601 date |
| `updated_date` | string | No | ISO 8601 date |

**Validation Rules**:
- All required fields must be populated
- Numeric values must be in valid ranges (0-100 for scores)
- `risk_band` must match calculated VTP
- One JSON object per line (no multi-line objects)
- UTF-8 encoding required
- File extension: `.jsonl`

**Location**: `data/reference/[industry]_reference.jsonl`

---

## Question Schema

### Individual Question Definitions

Not explicitly stored (embedded in engine), but conceptually:

```json
{
  "q_id": "q001",
  "industry": "Bakery",
  "dimension": "SV",
  "topic": "Seasonality",
  "question_text": "How significant is seasonal demand variation in your production?",
  "response_scale": "0-100 (0=no variation, 100=extreme variation)",
  "weighting": 1.0
}
```

**Properties**:
- **q_id**: Question identifier (q001-q198)
- **industry**: Specific to industry or universal
- **dimension**: SV, CV, or PV
- **question_text**: Assessment question
- **response_scale**: 0-100 (interpretation varies by question)
- **weighting**: Applied during aggregation (default 1.0)

All 198 questions documented in [RAMP Knowledge](https://github.com/RelyAbility/RAMP-Knowledge/tree/main/ESA/V3).

---

## Validation Result Format

### Test Outcome Documentation

Record of validation test execution and results.

```markdown
# [Industry] Validation Results

**Status**: ✅ VALIDATED  
**Validation Date**: 2026-03-15  
**Engine Version**: 3.0.0

## Inputs

- Reference JSONL: `data/reference/bakery_reference.jsonl`
- Assessment Data: `validation/data/bakery_assessment.json`
- Test Scenarios: 3 facilities

## Outputs

| Metric | Expected | Actual | Match |
|--------|----------|--------|-------|
| SV | 58.4 | 58.4 | ✅ |
| CV | 41.2 | 41.2 | ✅ |
| PV | 63.1 | 63.1 | ✅ |
| Overall | 54.2 | 54.2 | ✅ |
| Risk Band | Moderate | Moderate | ✅ |
| EMRA | Eligible | Eligible | ✅ |

**Match Rate**: 100% ✅

## Sign-Off

Validated by: Brad Parsons  
Date: 2026-03-15
```

---

## Type Definitions (Python)

### Pydantic Models

For type checking and validation:

```python
from pydantic import BaseModel
from typing import Optional, List, Dict

class AssessmentInput(BaseModel):
    q001: int  # ... q198
    
    class Config:
        ge = 0
        le = 100

class VTPOutput(BaseModel):
    structural_volatility: float
    constraint_volatility: float
    performance_volatility: float
    overall_volatility: float
    confidence_level: float

class RiskBandOutput(BaseModel):
    risk_band: str  # Literal["Low", "Moderate", "High", "Critical"]
    overall_score: float
    intervention_intensity: str
    recommendation: str

class EMRAEligibility(BaseModel):
    eligibility: str  # Literal["Eligible", "Conditional", "Not Eligible"]
    rationale: str
    prerequisites: List[str]
    estimated_readiness_date: Optional[str]
    next_assessment_date: str
    confidence: float
```

---

## JSON Schema Files

Formal schema definitions in `schemas/`:

- `schemas/vtp_schema.json` - VTP output validation
- `schemas/risk_band_schema.json` - Risk band output validation
- `schemas/emra_eligibility_schema.json` - EMRA determination validation
- `schemas/reference_data_schema.json` - JSONL reference file validation

---

## Encoding & Format Standards

- **Character Encoding**: UTF-8 (all files)
- **Line Endings**: LF (Unix-style, not CRLF)
- **JSON Formatting**: Compact (no pretty-printing in production)
- **Date Format**: ISO 8601 (YYYY-MM-DD)
- **Numeric Precision**: 1 decimal place for VTP scores

---

## Backward Compatibility

**Current Version**: 3.0.0  
**Breaking Changes**: None yet

When schema changes are made:
1. Document in CHANGELOG.md
2. Provide migration guide
3. Maintain backward compatibility for 2+ releases
4. Announce deprecation in advance

---

## Additional Resources

- [Module Overview](module-overview.md) - Engine functions and components
- [Validation Workflow](validation-workflow.md) - How to validate using these structures
- [RAMP Schema Definitions](../../ESA/V3/Documentation/Engine_Documentation.md) - Detailed schema reasoning

---

*Last Updated: March 2, 2026*  
*Version: 3.0.0*
