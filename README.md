# ALPE Design Simulator

A browser-based simulator for exploring ALPE-inspired pulmonary oxygen exchange and optimizing PETO₂ sampling protocols.

## What it does

- Simulates a PETO₂ → SpO₂ response from:
  - oxygen normalization pressure (ONP; low-V/Q surrogate),
  - shunt fraction,
  - pulse-ox random error,
  - subject-level pulse-ox bias,
  - PETO₂ measurement error,
  - residual model / steady-state error.
- Refits ONP and shunt by nonlinear least squares (coarse-to-fine grid search).
- Searches PETO₂ protocols across:
  - number of measurements,
  - low and high PETO₂ boundaries,
  - schedule family,
  - a grid of plausible ONP and shunt values.
- Uses Fisher-information approximations to rank designs.
- Propagates PETO₂ error using the local dSpO₂/dPETO₂ slope.
- Treats systematic SpO₂ bias as a nuisance parameter with a prior.
- Monte Carlo-validates the winning design by simulating and refitting noisy datasets.
- Exports simulated data and protocol rankings as CSV.

## Important model status

**v0.1 is a reduced ALPE teaching / experimental-design model, not a validated clone of commercial ALPE.**

It approximates low-V/Q mismatch as an effective oxygen pressure loss:

`Pc'O2 = PETO2 - ONP`

then converts end-capillary PO₂ to oxygen content, mixes in shunted venous blood, and converts the resulting arterial oxygen content back to saturation.

The application is deliberately structured so that the physiology function in `app.js` can later be replaced by a full two-compartment ALPE oxygen-transport implementation without changing the UI or the optimization framework.

## Run locally

No build step is required.

You can simply open `index.html`, although serving it through a local HTTP server is preferable:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Deploy with GitHub Pages

1. Create a GitHub repository and put these files in the repository root.
2. In GitHub, open **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select the branch containing the app (usually `main`) and `/ (root)`.
5. Save. GitHub Pages will publish the static site.

No server, database, package manager, or secret is required.

## Files

- `index.html` — application markup and explanatory methods page
- `styles.css` — responsive light/dark styling
- `app.js` — physiology model, nonlinear fitting, Fisher design optimization, Monte Carlo simulation, SVG plotting, CSV export

## Model roadmap

1. Benchmark the reduced ONP model against published ALPE curves.
2. Replace `predictSpO2Reduced()` with the full two-compartment oxygen mass-balance model.
3. Add measured/assumed Hb, VO₂, cardiac output, RQ, temperature, pH, and dyshemoglobins if required by the full implementation.
4. Validate simulator-generated parameter recovery against known synthetic truth.
5. Import real RespirAct steady-state PETO₂/PETCO₂ and pulse-ox data.
6. Add autocorrelated SpO₂ noise and step-specific steady-state uncertainty.
7. Compare fixed PETO₂ designs with adaptive SpO₂-targeted designs.

## Research-use caution

This software is currently intended for physiology education, method development, and study-design simulation. It is not a medical device and should not be used for clinical decision-making.

## References

See [`REFERENCES.md`](REFERENCES.md) for the ALPE papers motivating the model and the distinction between the current reduced implementation and a full ALPE oxygen-transport model.
