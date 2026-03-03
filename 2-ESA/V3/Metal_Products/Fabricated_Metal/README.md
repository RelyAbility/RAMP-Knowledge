# Fabricated Metal Sub-Industry Module

## Overview

The Fabricated Metal module documents the ESA V3 Volatility & Resilience Diagnostic engine application to the fabricated metal manufacturing industry.

**Status**: 🔄 **PENDING VALIDATION**  
**Target Validation**: Q2 2026  
**Reference Data**: Planned  
**Engine Status**: Ready for validation

---

## Industry Context

### Industry Definition
Fabricated metal manufacturing facilities producing metal components, assemblies, and finished metal products through cutting, forming, welding, and assembly processes from metal stock and parts.

### Key Characteristics
- **Production Model**: Job shop, batch, or assembly-line based
- **Operating Pattern**: Shift-based operations (typically single or double shift)
- **Typical Size**: Small to medium facilities (20-200+ employees)
- **Seasonality**: Low to moderate (customer demand driven)
- **Supply Chain**: Metal stock (steel, aluminum, stainless), fasteners, electrodes, specialty materials
- **Regulatory**: OSHA workplace safety, environmental (welding fumes), customer quality (aerospace, automotive, medical)
- **Equipment**: CNC machines, welders, sheet metal equipment, assembly fixtures, testing equipment

### Energy Profile
- **Dominant End-Use**: Electrical (CNC machines, welders), compressed air, hydraulics
- **Secondary Uses**: Material handling, facility lighting, HVAC for environment control
- **Operating Pattern**: Equipment-schedule driven
- **Peak Periods**: High-production order periods
- **Seasonal Variation**: Dependent on customer industry cycles (automotive, construction)

---

## Pre-Validation Status

**Current Phase**: Planning  
**Dependencies**:
- [ ] Reference JSONL file generation planned for Q2 2026
- [ ] Industry expert interviews to be conducted
- [ ] Question customization for fabricated metal

**Timeline**:
- Data Collection: April - May 2026
- Reference File Preparation: May 2026
- Validation Testing: June 2026
- Completion Target: June 30, 2026

---

## Provisional Volatility Framework (Preliminary)

### Expected Structural Volatility Drivers
- Customer order variability and job shop complexity
- Material type and specification diversity
- Production process complexity (job-shop vs. assembly)
- Technology level (CNC capabilities, automation)
- Market/customer concentration

### Expected Constraint Volatility Drivers
- Metal stock sourcing and price volatility
- Equipment capacity and versatility constraints
- Skilled worker availability (machining, welding expertise)
- Facility space for work-in-process and tooling
- Environmental/waste management infrastructure

### Expected Performance Volatility Drivers
- Equipment utilization and scheduling complexity
- Setup and changeover time impact on energy
- Assembly rework and defect rates
- Demand forecasting accuracy
- Equipment maintenance and downtime patterns

---

## Next Steps

### Q2 2026 Timeline
- [ ] Finalize facility selection for reference data
- [ ] Begin data collection (April-May)
- [ ] Prepare JSONL reference file (May)
- [ ] Conduct engine validation (June)

---

## Related Documentation

- [ESA V3 Master README](../../README.md) - Overview and architecture
- [Validation Tracking](../../Validation_Tracking.md) - Progress across all sub-industries

---

*Module Status: PENDING VALIDATION*  
*Last Updated: March 2, 2026*  
*Target Completion: June 30, 2026*
