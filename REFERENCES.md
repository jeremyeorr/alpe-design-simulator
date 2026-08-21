# References and model context

## Model lineage

The simulator follows the conceptual structure used in ALPE-related pulmonary gas-exchange work: a shunt compartment plus two ventilated compartments. One ventilated compartment receives 90% of nonshunted perfusion and the other receives 10%; `fA2` distributes alveolar ventilation between them. When `fA2 = 0.90`, ventilation and perfusion are matched across the two ventilated compartments. Lower values produce increasing V/Q inequality.

Published ALPE studies estimate shunt and V/Q mismatch from measurements collected after step changes in inspired oxygen. V/Q mismatch has been expressed as `fA2` or as the oxygen-pressure drop from mixed alveolar gas to nonshunted end-capillary blood (`DeltaPO2`, closely related to the simulator's derived ONP measure).

## Current implementation

The browser implementation uses:

- fixed 10% / 90% nonshunted perfusion;
- variable ventilation split `1 - fA2` / `fA2`;
- oxygen mass balance in each ventilated compartment;
- a simplified Hill oxyhemoglobin dissociation relationship;
- oxygen-content mixing of the two end-capillary streams;
- mixed-venous oxygen determined from assumed VO2 and cardiac output by Fick closure;
- oxygen-content mixing with true-shunt blood; and
- ONP derived as `PETO2 - Pc'O2`, where `Pc'O2` is the oxygen pressure corresponding to mixed nonshunted end-capillary oxygen content.

The protocol-design layer adds Fisher-information optimization, adaptive subject-specific enforcement of an SpO2 floor, paired bootstrap non-inferiority analysis for minimum adequate N, and optional paired nonlinear Monte Carlo validation. These design procedures are features of this simulator and should not be attributed to the cited ALPE implementations.

## Important limitations

This application is an **ALPE-style research prototype**, not a validated clone of the complete published or commercial ALPE software.

In particular:

- blood-gas chemistry is simplified;
- acid-base status, temperature, PCO2, dyshemoglobins, and detailed dissociation-curve shifts are not modeled explicitly;
- compartment perfusion is fixed at 10% / 90%;
- systemic variables are assumptions rather than subject measurements; and
- the optimizer has not yet been validated against empirical RespirAct acquisitions.

The software should therefore be used for physiology education, method development, sensitivity analysis, and protocol-design simulation. It is not a medical device and should not guide clinical care.

## Primary and supporting literature

1. Kjaergaard S, Rees SE, Malczynski J, Nielsen JA, Thorgaard P, Toft E, Andreassen S. **Non-invasive estimation of shunt and ventilation-perfusion mismatch.** *Intensive Care Medicine.* 2003;29(5):727-734. [PMID 12698242](https://pubmed.ncbi.nlm.nih.gov/12698242/). [DOI 10.1007/s00134-003-1708-0](https://doi.org/10.1007/s00134-003-1708-0).

2. Kjaergaard S, Rees SE, Gronlund J, Nielsen EM, Lambert P, Thorgaard P, Toft E, Andreassen S. **Hypoxaemia after cardiac surgery: clinical application of a model of pulmonary gas exchange.** *European Journal of Anaesthesiology.* 2004;21(4):296-301. [PMID 15109193](https://pubmed.ncbi.nlm.nih.gov/15109193/). [DOI 10.1017/S0265021504004089](https://doi.org/10.1017/S0265021504004089).

3. Smith BW, Rees SE, Karbing DS, Kjaergaard S, Andreassen S. **Quantitative assessment of pulmonary shunt and ventilation-perfusion mismatch without a blood sample.** *Annual International Conference of the IEEE Engineering in Medicine and Biology Society.* 2007:4255-4258. [PMID 18002942](https://pubmed.ncbi.nlm.nih.gov/18002942/). [DOI 10.1109/IEMBS.2007.4353276](https://doi.org/10.1109/IEMBS.2007.4353276).

4. Moesgaard J, Kristensen JH, Malczynski J, et al. **Can new pulmonary gas exchange parameters contribute to evaluation of pulmonary congestion in left-sided heart failure?** *Canadian Journal of Cardiology.* 2009;25(3):149-155. [PMID 19279982](https://pubmed.ncbi.nlm.nih.gov/19279982/). [DOI 10.1016/S0828-282X(09)70042-X](https://doi.org/10.1016/S0828-282X(09)70042-X).

5. Thomsen LP, Karbing DS, Smith BW, et al. **Clinical refinement of the automatic lung parameter estimator (ALPE).** *Journal of Clinical Monitoring and Computing.* 2013;27(3):341-350. [PMID 23430364](https://pubmed.ncbi.nlm.nih.gov/23430364/). [DOI 10.1007/s10877-013-9442-9](https://doi.org/10.1007/s10877-013-9442-9).

6. Riedlinger A, Kretschmer J, Moller K. **On the practical identifiability of a two-parameter model of pulmonary gas exchange.** *BioMedical Engineering OnLine.* 2015;14:82. [PMID 26337953](https://pubmed.ncbi.nlm.nih.gov/26337953/). [DOI 10.1186/s12938-015-0077-6](https://doi.org/10.1186/s12938-015-0077-6).

## Interpretation

The cited literature supports the use of oxygen-step response data to distinguish shunt from V/Q mismatch and supports the fixed 10/90, `fA2`-parameterized two-compartment structure. It does **not** by itself validate the browser implementation's simplified chemistry, adaptive SpO2-floor rule, or recommended protocol sizes. Those elements require separate simulation and empirical validation.
