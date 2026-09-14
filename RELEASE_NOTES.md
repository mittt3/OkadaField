# OkadaField release notes

## 1.0.0

First stable release of OkadaField.

- Promoted the v0.4.4 beta feature set to the first stable release.
- Retained 1:1 plot-distance scaling and centered, symmetric displacement-vector placement.
- Retained browser heartbeat monitoring and automatic shutdown after about 90 seconds without browser communication.
- Retained displacement, strain, stress-change, Coulomb-stress-change, tilt, GSI map overlay, PNG/CSV export, and JSON settings support.
- Simplified README for end users; detailed beta development history remains in these release notes.
- Updated application and citation metadata to version 1.0.0.
- No changes to the DC3D numerical kernel or the strain/stress/Coulomb calculations relative to v0.4.4 beta.

## 0.4.4 beta

- Centered the plot data region horizontally while preserving 1:1 distance scaling.
- Changed displacement-vector subsampling to be symmetric about the grid center.
- For the default 101 x 101 grid, the vector lattice includes the exact center point and is left/right and up/down symmetric.
- No changes to the DC3D numerical kernel, strain/stress/Coulomb calculations, or browser auto-shutdown behavior.

## 0.4.3 beta

- Added browser heartbeat monitoring and automatic application shutdown.
- If browser communication stops for about 90 seconds, the localhost server and `OkadaField.exe` shut down automatically.
- Updated README contact to `mit [at] shizuoka.ac.jp`.
- Added `CITATION.cff` for GitHub/Zenodo citation metadata.

## 0.4.2 beta

- Changed plot geometry so horizontal and vertical distance scales are equal (1:1).
- Geographic-map mode fits the plot area to the Web Mercator projected aspect ratio.

## 0.4.1 beta

- Added shared / per-field / user-defined stress color-scale controls.
- dTau, dSigmaN, and dCFF can be compared using one common symmetric MPa range.

## 0.4.0 beta

- Added elastic stress-change calculation from the DC3D strain tensor.
- Added receiver-fault Coulomb stress calculation.
- Added rigidity `mu` (GPa), receiver strike / dip / rake, and effective friction coefficient `mu'`.

## 0.3.2 beta

- Renamed the application to OkadaField.
- Simplified the source overlay to a dashed horizontal fault projection and source-reference `+` only.
- Enlarged basic plot labels for readability.
