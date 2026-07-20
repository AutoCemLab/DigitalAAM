# AAM Composition Predictor Prototype

This is a first-pass interactive website based on:

Ke and Duan, *Composites Part B* 216 (2021) 108801, "Coupling machine learning with thermodynamic modelling to develop a composition-property model for alkali-activated materials."

## What It Does

- Lets a visitor enter two mineral precursors, including mass share and oxide chemistry for `CaO`, `SiO2`, `Al2O3`, `MgO`, and `Na2O`.
- Includes activator controls for dry `NaOH`, sodium silicate solution mass, activator modulus `SiO2/Na2O` molar ratio, water content in the sodium silicate solution, and additional water.
- Includes concrete-scale controls for total binder mass, fine aggregate, and coarse aggregate in kg per m3 concrete.
- Computes GPR-style inputs on a total binder mass basis for nearest saved prediction matching.
- Displays an illustrative predicted compressive strength and uncertainty.
- Draws three ternary diagrams matching Figure 4:
  - `CaO-Al2O3-SiO2`: predicted strength color map.
  - `CaO-Na2O-SiO2`: predicted strength color map.
  - `CaO-MgO-SiO2`: predicted strength color map.
- Uses the same vertex placement as Figure 4: `CaO` at the top, `SiO2` bottom-right, and `Al2O3`/`Na2O`/`MgO` bottom-left.
- Uses a revised strength color scale for the updated prediction distribution: `<35`, `35-40`, `40-42.5`, `42.5-45`, `45-47.5`, `47.5-50`, `50-55`, and `>55` MPa.
- Adds Figure 4-style Ca/Si molar-ratio guide lines: `Ca/Si=1.0` with grey dashed boundary guides for `0.8` and `1.5`, plus C-(N)-A-S-H and N-A-S-H dominance annotations positioned from the paper's reported midpoint compositions for the high-Ca and low-Ca strength regions.

## Current Data Connection

The browser app now loads `strength_predictions.js`, generated from:

`C:\Users\xk221\Documents\Codex\2026-07-10\can\Saved_DigitalAAM_Strength_output_noise_only\random_range_forward_predictions.csv`

The static site uses binder-specific exported prediction files generated from the revised random-range forward prediction datasets:

- `strength_predictions_binder_350.js` from `random_range_forward_predictions_binder_350.csv`.
- `strength_predictions.js` from `random_range_forward_predictions_binder_450.csv`, used as the default file.
- `strength_predictions_binder_550.js` from `random_range_forward_predictions_binder_550.csv`.

Each file contains 547,632 saved strength predictions with uncertainty. The exported browser data keeps the trained-model feature units in kg/m3 and stores rows in a compact array format for local static use.

Current levels in the exported file:

- Binder/oxide sum: `350`, `450`, or `550` kg/m3, depending on the selected data file.
- Water: `100` to `250` kg/m3 in 10 kg/m3 steps.
- Fine aggregate: `600`, `800`, `1000` kg/m3.
- Coarse aggregate: `800`, `1000`, `1200` kg/m3.

The web page loads the data file that matches the selected binder option, then filters the active plot and nearest prediction lookup to rows that exactly match the selected binder, fine aggregate, and coarse aggregate levels. Ternary coordinates are normalised inside each three-component projection.

For arbitrary new inputs, the current app uses nearest saved response-surface matching from this exported prediction table. It does not require a local Python backend.

Feature order represented by the trained-model output table:

```text
CaO, SiO2, MgO, Al2O3, Na2O, water, Fine aggregate, Coarse aggregate, Age
```

## Current Input Draft

The input panel is now a user-facing mix design draft, not yet the final exact GPR feature mapping. The current conversion assumes:

- Two mineral precursor shares are normalised to 100 g total mineral precursors.
- Total binder mass is `100 g total mineral precursors + dry NaOH + sodium silicate solution + additional water`.
- Precursor oxide masses are calculated as `precursor oxide wt.% * precursor mass`.
- Dry `NaOH` is converted to `Na2O` equivalent using `Na2O / (2 NaOH)` molar mass conversion.
- Sodium silicate solution is split into water and dry solids using its water wt.%.
- Sodium silicate dry solids are split into `Na2O` and `SiO2` using the activator modulus and molar masses.
- `CaO`, `Al2O3`, and `MgO` inputs are precursor oxide masses divided by total binder mass.
- `SiO2` input is precursor `SiO2` plus sodium silicate `SiO2`, divided by total binder mass.
- `Na2O` input is precursor `Na2O` plus NaOH-derived `Na2O` plus sodium-silicate-derived `Na2O`, divided by total binder mass.
- Water input is sodium silicate solution water plus additional water, divided by total binder mass.
- Concrete-scale inputs are passed through as total binder mass, fine aggregate, and coarse aggregate in kg/m3 for nearest saved prediction matching.
- Degree of reaction is entered separately for mineral precursor 1 and mineral precursor 2. Each DoR is applied only to oxide contributions from its own precursor for prediction. Precursor-derived `CaO`, `SiO2`, `Al2O3`, `MgO`, and `Na2O` are multiplied by that precursor's `DoR / 100`; activator-derived `Na2O` and `SiO2` are kept fully counted. Water, binder mass, aggregates, and age are left unchanged.

## Important Scientific Note

The current maps and displayed prediction are based on saved GPR outputs, but the app is not directly executing the trained `.pkl` model in response to every possible new user input.

Before public scientific use, replace the `predict()` function in `index.html` with either:

1. A call to a Python API serving the real trained GPR model, or
2. A JavaScript-exported version of the trained model, if the model is simple enough to run in the browser.

## Recommended Production Architecture

For a proper research website:

- Static frontend: this `index.html` or a React/Vue/Svelte version.
- Model backend: Python `FastAPI`, `Streamlit`, or `Gradio`.
- Model files:
  - trained GPR model,
  - scaler/normalizer,
  - feature-generation pipeline,
  - model metadata and units.
- Hosting:
  - free/low-cost demo: Hugging Face Spaces or Streamlit Community Cloud,
  - polished deployment: static frontend plus hosted Python API.

## How To Preview

Open `index.html` directly in a browser.

No build step is required for this prototype.
