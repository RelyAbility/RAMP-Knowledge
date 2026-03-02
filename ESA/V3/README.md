# ESA V3: Volatility & Resilience Diagnostic Engine

## Overview

ESA V3 is the **Universal Volatility Engine** that computes comprehensive Volatility Type Profiles (VTP) across 20 sub-industries, assigning risk bands and determining EMRA eligibility. This is the current validated production version.

**Status**: ✅ Current - Validated  
**Operational**: Yes - Backend calculation engine running  
**UI Status**: Not yet built (UI development is next phase)

---

## What ESA V3 Does

### Core Functionality

1. **Takes Industry-Specific Input**
   - Consumes enriched JSONL files with industry-specific "Gold Standard" data
   - Applies industry-specific questions to conduct comprehensive assessment

2. **Computes Volatility Type Profile (VTP)**
   - **SV**: Structural Volatility - inherent volatility from business model/structure
   - **CV**: Constraint Volatility - volatility imposed by resource constraints
   - **PV**: Performance Volatility - volatility from operational/market performance
   - Scoring range: 0-100 for each dimension

3. **Assigns Risk Bands**
   - **Low**: Stable, predictable state
   - **Moderate**: Some variability but managed
   - **High**: Significant volatility requiring attention
   - **Critical**: Extreme volatility, high intervention needs

4. **Determines EMRA Eligibility**
   - **Eligible**: Ready for energy management interventions
   - **Conditional**: Eligible with prerequisites
   - **Not Eligible**: Not suitable for EMRA programs

5. **Generates Measurement Readiness Profile**
   - Identifies data availability and quality metrics
   - Highlights constraints in measurement infrastructure

---

## Version History

| Version | Description | Status |
|---------|-------------|--------|
| **V1** | Original ESA - industry-specific maturity journey questions | Live (Original GitHub) |
| **V2** | P/C/E/B/V question typing for horizontal asset classes (Motors, Compressed Air) | Built, superseded |
| **V3** | Universal Volatility Engine - VTP scoring, risk bands, EMRA eligibility | ✅ Current - Validated |

---

## Architecture & Components

### Backend Engine

**Location**: `/app/backend/esa_v3/engine.py`

The engine is a Python-based calculation module that:
- Loads enriched JSONL reference data
- Processes industry-specific question datasets
- Computes VTP scores using validated algorithms
- Outputs risk bands and EMRA eligibility determinations
- Generates measurement readiness profiles

**Key Entry Points**:
- `engine.calculate_vtp()` - VTP computation
- `engine.assign_risk_band()` - Risk band determination
- `engine.determine_emra_eligibility()` - EMRA eligibility logic
- `engine.generate_measurement_readiness()` - Readiness profile generation

### Data Pipeline

```
Gold Standard JSONL Reference Files
        ↓
Industry-Specific Enriched Data
        ↓
ESA V3 Engine (engine.py)
        ↓
VTP Scores (SV, CV, PV)
        ↓
Risk Band Assignment
        ↓
EMRA Eligibility Determination
        ↓
Measurement Readiness Profile
        ↓
Output Results (JSON)
```

---

## Validation Status

### Fully Validated Sub-Industries (4/20)
- ✅ Bakery - 100% valid, reference VTP matches
- ✅ Dairy - 100% valid, reference VTP matches
- ✅ Meat - 100% valid, reference VTP matches
- ✅ Plastics - 100% valid, reference VTP matches

**Validation Metrics**:
- Total Unique Questions: 198
- Questions Validated: 198 (across 4 sub-industries)
- Engine Match Rate: 100% (matches reference VTPs exactly)
- Test Data Sets: 3 of 4 with complete reference files

### Pending Validation (16/20)
See [Validation Tracking](Validation_Tracking.md) for current status across all sub-industries.

---

## Key Documents

- **[Engine Documentation](Documentation/Engine_Documentation.md)** - Technical specifications, VTP scoring methodology, EMRA eligibility rules
- **[Validation Tracking](Validation_Tracking.md)** - Progress dashboard across all 20 sub-industries
- **[Reference Data Status](Reference_Data/Reference_Data_Status.md)** - JSONL files location, status, and availability
- **Sub-Industry READMEs** - Organized by industry category with specific implementation details

---

## Sub-Industry Organization

ESA V3 covers 20 sub-industries organized into 6 industry categories:

### Food & Beverage (4 sub-industries)
- [Bakery](Food_and_Beverage/Bakery/README.md) ✅ Validated
- [Dairy](Food_and_Beverage/Dairy/README.md) ✅ Validated
- [Meat](Food_and_Beverage/Meat/README.md) ✅ Validated
- [Beverage](Food_and_Beverage/Beverage/README.md) 🔄 Pending

### Plastics & Packaging (4 sub-industries)
- [Plastics](Plastics_and_Packaging/Plastics/README.md) ✅ Validated
- [Paper Packaging](Plastics_and_Packaging/Paper_Packaging/README.md) 🔄 Pending
- [Flexible Packaging](Plastics_and_Packaging/Flexible_Packaging/README.md) 🔄 Pending
- [Rigid Packaging](Plastics_and_Packaging/Rigid_Packaging/README.md) 🔄 Pending

### Metal Products (3 sub-industries)
- [Fabricated Metal](Metal_Products/Fabricated_Metal/README.md) 🔄 Pending
- [Foundries](Metal_Products/Foundries/README.md) 🔄 Pending
- [Machining & Metalworking](Metal_Products/Machining_and_Metalworking/README.md) 🔄 Pending

### Chemicals & Pharmaceuticals (3 sub-industries)
- [Chemicals](Chemicals_and_Pharmaceuticals/Chemicals/README.md) 🔄 Pending
- [Pharmaceuticals](Chemicals_and_Pharmaceuticals/Pharmaceuticals/README.md) 🔄 Pending
- [Specialty Chemicals](Chemicals_and_Pharmaceuticals/Specialty_Chemicals/README.md) 🔄 Pending

### Mining & Primary Metals (3 sub-industries)
- [Mining](Mining_and_Primary_Metals/Mining/README.md) 🔄 Pending
- [Primary Metals Smelting](Mining_and_Primary_Metals/Primary_Metals_Smelting/README.md) 🔄 Pending
- [Rolling & Finishing](Mining_and_Primary_Metals/Rolling_and_Finishing/README.md) 🔄 Pending

### Facilities Management (4 sub-industries)
- [Hospitals](Facilities_Management/Hospitals/README.md) 🔄 Pending
- [Aged Care](Facilities_Management/Aged_Care/README.md) 🔄 Pending
- [Large Retail](Facilities_Management/Large_Retail/README.md) 🔄 Pending
- [Campuses](Facilities_Management/Campuses/README.md) 🔄 Pending

**Legend**: ✅ = Validated | 🔄 = Pending Validation

---

## Key Metrics

| Metric | Value |
|--------|-------|
| Total Sub-Industries | 20 |
| Validated Sub-Industries | 4 |
| Industry Categories | 6 |
| Total Unique Questions | 198 |
| VTP Dimensions | 3 (SV, CV, PV) |
| Risk Bands | 4 (Low, Moderate, High, Critical) |
| EMRA Eligibility States | 3 (Eligible, Conditional, Not Eligible) |

---

## Next Steps & Roadmap

### Immediate (In Progress)
- [ ] Validate remaining 16 sub-industries
- [ ] Verify JSONL reference files for all sub-industries
- [ ] Validate engine against each reference dataset
- [ ] Document any sub-industry-specific rule modifications

### Near-term (Next Phase)
- [ ] Build V3 UI layer
- [ ] Create assessment workflow interface
- [ ] Implement results visualization
- [ ] Integrate with EMRA module

### Future Enhancements
- [ ] Real-time scoring dashboard
- [ ] Comparative analysis across sub-industries
- [ ] Trend analysis and forecasting
- [ ] Integration with other RAMP modules

---

## Technical References

- **Backend Code**: `/app/backend/esa_v3/engine.py`
- **Data Format**: JSONL (JSON Lines - one JSON object per line)
- **Python Version**: 3.8+
- **Dependencies**: See engine documentation

---

## Questions & Support

For questions about:
- **Engine Logic**: See [Engine Documentation](Documentation/Engine_Documentation.md)
- **Specific Sub-Industry**: Check the sub-industry README
- **Validation Status**: See [Validation Tracking](Validation_Tracking.md)
- **Reference Data**: Check [Reference Data Status](Reference_Data/Reference_Data_Status.md)

---

*Last Updated: March 2, 2026*  
*Repository: RAMP/ESA/V3*
