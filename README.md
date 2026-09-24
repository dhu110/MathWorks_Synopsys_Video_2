# PMSM Calibration Workflow Project (Video 2 Artifacts)
*Built using MATLAB R2026a, Copyright 2026 The MathWorks, Inc.*

## Executive Summary

This project demonstrates a model-based calibration workflow for a permanent magnet synchronous motor (PMSM) current controller using Motor-CAD generated SaturationLossMap.mat as the starting point. The main artifact, [PMSMCalibrationWorkflowScript.mlx], takes raw `SaturationLossMap.mat` data, converts irregular flux, torque, and loss measurements into a calibration-ready dataset, and then uses the Model-Based Calibration API (`mbcrunner`) with included Model Fitting/CAGE templates to generate optimal `Id` and `Iq` lookup tables across speed and torque operating points.

At a high level, the workflow closes the gap between motor characterization data and controls implementation. It preprocesses finite-element motor data into a structured `MBCdata` table, derives control-relevant quantities such as voltage, current magnitude, shaft torque, and loss, fits calibration models, and produces lookup tables that can be reviewed and applied in the included closed-loop PMSM simulation model. In practical terms, this repository is a compact handoff package for turning offline motor map data into controller calibration assets and validating those assets in simulation.

## Workflow Scope

The live script performs these major steps:

1. Load raw PMSM characterization data from `SaturationLossMap.mat`.
2. Visualize flux linkage and torque behavior in the `Id`-`Iq` plane.
3. Define controller constraints such as DC bus voltage, current limit, and modulation voltage.
4. Interpolate non-cartesian Motor-CAD data onto a regular grid and derive torque, loss, and inductance quantities.
5. Rearrange the motor map into torque-speed operating points suitable for calibration.
6. Build the `MBCdata` table with units and derived signals.
7. Use the MBC project and CAGE template files to generate optimal current controller tables.
8. Plot the resulting torque envelope, `Id` table, `Iq` table, and loss breakdown versus speed.

## Included Assets

- [PMSMCalibrationWorkflowScript.mlx]: Primary workflow script and documentation.
- [SaturationLossMap.mat]: Raw PMSM flux, torque, and loss data coming out of Motor-CAD.
- [MBCTemplateFiles]: Included MBC model-fitting template and CAGE optimization project used by the script.
- [ClosedLoopPMSMSimulationModel]: Closed-loop Simulink/Simscape PMSM drive model for downstream validation of the generated calibration tables.

## Expected Outputs

Key outputs from the workflow include:

- `MBCdata`: calibration-ready operating-point table containing torque, current, speed, voltage, shaft torque, and loss information.
- Optimal controller lookup tables for `Id`, `Iq`, and torque envelope across the requested speed and torque breakpoints.
- Diagnostic plots that help review interpolation quality, operating envelope coverage, and full-load loss composition.

## Notes for Users

This example was authored for MATLAB R2026a. The live script indicates optional use of Signal Processing Toolbox for `downsample`, and the calibration portion depends on the Model-Based Calibration workflow assets included in `MBCTemplateFiles`. The closed-loop model also depends on the Simscape Electrical toolchain used by the PMSM drive model.

## License

See [license.txt].
