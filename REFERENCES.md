# References and model context

This prototype is informed by the ALPE literature but does **not** yet reproduce the full published/commercial ALPE oxygen-transport model.

1. Kjaergaard S, Rees SE, Grønlund J, et al. **Hypoxaemia after cardiac surgery: clinical application of a model of pulmonary gas exchange.** Related ALPE work established model-based separation of shunt and V/Q mismatch using controlled oxygen changes.

2. Kjaergaard S, Rees SE, Malczynski J, et al. **Non-invasive estimation of shunt and ventilation-perfusion mismatch.** *Intensive Care Medicine.* 2003. PMID: 12698242.  
   https://pubmed.ncbi.nlm.nih.gov/12698242/

3. Smith BW, Rees SE, Karbing DS, Kjaergaard S, Andreassen S. **Quantitative assessment of pulmonary shunt and ventilation-perfusion mismatch without a blood sample.** *IEEE EMBS.* 2007. PMID: 18002942.  
   https://pubmed.ncbi.nlm.nih.gov/18002942/

4. Thomsen LP, Karbing DS, Smith BW, et al. **Clinical refinement of the automatic lung parameter estimator (ALPE).** *Journal of Clinical Monitoring and Computing.* 2013;27:341-350. PMID: 23430364.  
   https://pubmed.ncbi.nlm.nih.gov/23430364/

5. Rees SE and colleagues. The published ALPE framework fits a physiological oxygen-transport model to multiple oxygenation steady states, with end-expired O2 / saturation data used to identify shunt and a low-V/Q parameter expressed as ΔA-cPO2 / oxygen normalization pressure (ONP).

## Why the current model is reduced

The current browser model uses the following transparent approximation:

`Pc'O2 = PETO2 - ONP`

followed by oxygen-content mixing with shunted venous blood. This preserves the key conceptual behavior needed to explore experimental design:

- ONP predominantly shifts the oxygen-response relationship;
- shunt produces a different response pattern, especially toward higher oxygen;
- measurement noise and systematic pulse-ox bias impair identifiability;
- the information content of a measurement depends strongly on where it falls on the oxygen dissociation curve.

Before using design recommendations prospectively in a physiologic study, the reduced function should be benchmarked against a full ALPE implementation and empirical RespirAct data.
