# OkadaField release notes

## 0.4.2 beta

- Changed plot geometry so horizontal and vertical distance scales are equal (1:1).
- East–North maps now use the same pixel scale per km in both directions.
- East–Depth and North–Depth sections also use the same pixel scale per km horizontally and vertically.
- Geographic-map mode fits the plot area to the Web Mercator projected aspect ratio so vector directions are not distorted by the plotting rectangle.
- Vector arrows therefore preserve their displayed direction relative to the coordinate axes.
- No changes to the DC3D numerical kernel, strain/stress calculation, or Coulomb-stress calculation.
- Simplified the GSI map-background wording in README.

## 0.4.1 beta

- Added stress color-scale controls.
- Default scale is shared within each comparison group:
  - six stress-tensor components share one symmetric MPa range;
  - dTau, dSigmaN, and dCFF share one symmetric MPa range.
- Added per-field automatic scaling and user-defined symmetric +/- MPa scaling.
- Color-scale mode and manual range are saved in JSON settings.
- No changes to the DC3D numerical kernel, elastic stress calculation, or Coulomb stress calculation.
- Added a regression test confirming dTau and dCFF are numerically distinct for the default reverse-fault example.

## 0.4.0 beta

- Added elastic stress-change calculation from the DC3D strain tensor.
- Added rigidity `mu` (GPa) as a user input.
- The first Lamé constant `lambda` is calculated automatically from Poisson's ratio and rigidity.
- Added stress components: dSigmaEE, dSigmaNN, dSigmaUU, dSigmaEN, dSigmaEU, dSigmaNU (MPa).
- Added receiver-fault Coulomb stress calculation:
  - dTau: positive in the specified receiver rake direction
  - dSigmaN: positive for tension / unclamping
  - dCFF = dTau + mu' dSigmaN
- Added receiver strike / dip / rake and user-defined effective friction coefficient `mu'`.
- Added `receiver = source` mode. Receiver strike/dip follow the source and rake is derived from atan2(DISL2, DISL1).
- Stress and Coulomb quantities are included in CSV export and JSON settings.
- Kept the simple fault projection used in v0.3.2: dashed horizontal projection and source-reference `+` only.
- Kept the enlarged plot axis/tick labels.
- Public package remains Windows x64 / Japanese UI only. Source code is not included in the distribution.

### Validation added in 0.4.0

- `nu = 0.25`, `mu = 30 GPa` gives `lambda = 30 GPa`.
- A synthetic receiver-plane stress tensor resolves to the expected normal stress, shear stress, and Coulomb stress signs and magnitudes.
- At the free surface, dSigmaUU, dSigmaEU, and dSigmaNU are zero to numerical precision away from singular points.

## 0.3.2 beta

- Renamed the application to OkadaField.
- Simplified the source overlay to a dashed horizontal fault projection and source-reference `+` only.
- Enlarged basic plot labels for readability.
- Public package contains only the GUI executable, documentation, and examples.
