# Campuses Sub-Industry Module

## Overview

The Campuses module documents the ESA V3 Volatility & Resilience Diagnostic engine application to institutional campus facilities, including educational institutions and corporate campuses.

**Status**: 🔄 **PENDING VALIDATION**  
**Target Validation**: Q3 2026  
**Reference Data**: Planned  
**Engine Status**: Ready for validation

---

## Industry Context

### Industry Definition
Institutional campus facilities including universities, colleges, corporate headquarters, and research parks with centralized utility systems serving multiple buildings and diverse energy end-uses.

### Key Characteristics
- **Facility Type**: Multi-building campuses (typically 50-500+ acres, multiple buildings)
- **Operating Pattern**: Business/academic hours (buildings occupied 6am-10pm typically, some 24/7)
- **Typical Size**: Large facilities with multiple building zones, 500-3000+ staff
- **Seasonality**: High (heating/cooling demand swings, semester/fiscal year patterns)
- **Systems**: Central heating/cooling plant, electrical distribution, water/wastewater
- **Regulatory**: Local building codes, accessibility (ADA), safety standards
- **Equipment**: Central boiler/chiller plant, district heating/cooling, emergency power

### Energy Profile
- **Dominant End-Use**: Space conditioning (HVAC for occupied buildings)
- **Secondary Uses**: Hot water, lighting, plug loads (offices, labs, computers)
- **Operating Pattern**: Occupancy-driven (variable hours by building/zone)
- **Peak Periods**: Business/academic hours, season-dependent HVAC peaks
- **Seasonal Variation**: Very high (winter/summer HVAC extremes, semester variations)

---

## Pre-Validation Status

**Current Phase**: Planning  
**Dependencies**:
- [ ] Reference JSONL file generation planned for Q3 2026
- [ ] Campus facility interviews and central plant data access
- [ ] Question customization for institutional campus context

**Timeline**:
- Data Collection: July - August 2026
- Reference File Preparation: August 2026
- Validation Testing: September 2026
- Completion Target: September 30, 2026

---

## Provisional Volatility Framework (Preliminary)

### Expected Structural Volatility Drivers
- Building occupancy variability (by academic/fiscal calendar)
- Campus infrastructure complexity (central plant, distribution networks)
- Building diversity (offices, labs, classroom types, residential)
- Technology infrastructure (IT loads, research equipment)
- Regulatory and institutional requirements

### Expected Constraint Volatility Drivers
- Central chiller/boiler capacity constraints
- District heating/cooling distribution network
- Building envelope variation (old vs. new, insulation quality)
- Utility infrastructure and submetering adequacy
- Maintenance staff availability for centralized systems

### Expected Performance Volatility Drivers
- Occupancy patterns (semester schedules, vacation periods)
- Weather-driven HVAC demand variability
- Building energy intensity variation (labs vs. offices)
- Central plant efficiency and load management
- Systems optimization and demand response opportunities

---

## Notes

Institutional campuses present unique challenges:
- Complex central utility systems with multiple buildings
- Significant seasonal variability driven by academic/fiscal calendars
- High potential for demand response and peak shaving programs
- Institutional mission alignment and sustainability commitments

---

## Next Steps

### Q3 2026 Timeline
- [ ] Finalize campus facility selection for reference data
- [ ] Begin data collection (July-August)
- [ ] Prepare JSONL reference file (August)
- [ ] Conduct engine validation (September)

---

## Related Documentation

- [ESA V3 Master README](../../README.md) - Overview and architecture
- [Validation Tracking](../../Validation_Tracking.md) - Progress across all sub-industries

---

*Module Status: PENDING VALIDATION*  
*Last Updated: March 2, 2026*  
*Target Completion: September 30, 2026*
