# ALPE Design Simulator

A browser-based simulator and protocol-design tool for an ALPE-style two-compartment model of pulmonary oxygen exchange.

The current reviewed interface is **v18**. The application is a single self-contained `index.html` file and runs entirely in the browser.

## What the simulator models

The lung model contains:

- a true-shunt compartment;
- two ventilated compartments receiving fixed 10% and 90% shares of nonshunted perfusion;
- `fA2`, the fraction of total alveolar ventilation sent to the 90%-perfused compartment;
- compartment-specific V/Q ratios, alveolar oxygen pressures, and end-capillary oxygen content;
- ventilation-weighted expired-gas mixing to obtain PETO2;
- perfusion-weighted capillary blood mixing;
- mixed-venous oxygen determined from the assumed VO2 and cardiac output using Fick closure;
- arterial oxygenation after shunt mixing; and
- ONP derived as the pressure difference between mixed expired gas and nonshunted end-capillary blood.

This replaces the repository's earlier reduced model, in which ONP was entered directly as an effective pressure loss.

## Simulator page

The simulator page:

- generates multi-point PETO2-SpO2 observations from known `fA2` and shunt values;
- adds configurable SpO2 random error, systematic pulse-ox bias, PETO2 measurement error, and residual model or steady-state error;
- fits `fA2` and shunt jointly, then derives ONP from the fitted physiology;
- plots the true, observed, and fitted oxygen-response curves;
- reports parameter recovery and pointwise residuals;
- exports the simulated dataset as CSV; and
- displays a dynamic physiology diagram using the reviewed v18 visual convention:
  - neutral compartment boxes;
  - blue gas and ventilation paths, including expired-gas mixing;
  - purple blood and perfusion paths, including capillary mixing and shunt;
  - dashed links for calculated quantities; and
  - flow-proportional line widths with arrow tips terminating at box boundaries.

## Protocol optimizer

The optimizer searches PETO2 schedules across a grid of physiology states and measurement-error assumptions.

Key behavior:

- the low and high PETO2 windows are literal anchor constraints;
- at least one nominal target must occur in each anchor window;
- the remaining points are optimized freely;
- Fisher-information calculations rank schedules by recovery of derived ONP and shunt;
- systematic SpO2 bias is represented as a nuisance parameter;
- PETO2 error is propagated through the local response-curve slope; and
- candidate schedules can be checked with nonlinear Monte Carlo refitting.

### Safety modes

**Adaptive SpO2 floor** is the default. Nominal PETO2 targets are retained for protocol design, but a target is raised for an individual simulated physiology when needed to keep predicted SpO2 at the selected floor. The optimizer reports how often each target is truncated and the distribution of effective PETO2 values.

**Fixed universal schedule** requires every nominal target to satisfy the SpO2 floor for every simulated physiology. If the requested low anchor window is incompatible with that constraint, the app reports that no feasible fixed schedule exists.

Fisher optimization, minimum-adequate-N analysis, and Monte Carlo validation all use the subject-specific effective PETO2 values in adaptive mode.

## Minimum adequate N

For each candidate number of PETO2 levels, the app optimizes a schedule and compares its precision with the largest tested design.

The analysis:

- bootstraps paired losses in derived-ONP and shunt precision;
- separates statistically detectable loss from a prespecified meaningful loss;
- recommends the smallest N whose upper 95% confidence bounds remain within both tolerances; and
- optionally performs paired nonlinear Monte Carlo validation of the recommended N against the reference design, including boundary-fit behavior.

This is a non-inferiority decision framework, not simply a test of whether adding another measurement produces a nonzero improvement.

## Run

No build step, package manager, server, database, or secret is required. Open `index.html` in a modern browser.

The published GitHub Pages site is:

<https://jeremyeorr.github.io/alpe-design-simulator/>

## Files

- `index.html` - complete application, model, optimizer, plots, and styling
- `README.md` - project and workflow overview
- `REFERENCES.md` - literature basis, implementation assumptions, and model limitations

## Model status and research-use caution

This is an ALPE-style research and experimental-design simulator, not a validated reproduction of the full commercial ALPE implementation. It uses a simplified oxyhemoglobin dissociation relationship and blood-oxygen content calculation, fixed 10/90 nonshunted perfusion, and user-supplied systemic assumptions.

Protocol recommendations should be validated against the intended measurement system, complete blood-gas chemistry, and empirical RespirAct data before prospective physiologic use. The software is not a medical device and must not be used for clinical decision-making.

See [`REFERENCES.md`](REFERENCES.md) for the model lineage and supporting literature.
