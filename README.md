# The Donroe Doctrine
### Predicting U.S. Military Intervention in the Americas from Open-Source Narrative Data

**BLUF:** Open-source news narrative data (GDELT) can be compressed into a single
"escalation vector" that distinguishes political rhetoric from operational
preparation for military action. Benchmarked against the 2026 Venezuela strike
(escalation score 15.74 at H-minus-24), the current Mexico narrative measured
only 19.82% similar — **we assess with moderate confidence that rhetoric toward
Mexico lacked the narrative fusion that historically precedes intervention.**

## Key Judgments
- 14 GDELT narrative features (tone, polarity, thematic counts) reduce to 6
  principal components; PC1 alone carries ~78% of the signal (KMO 0.83).
- The Venezuela intervention produced a distinct, quantifiable signature in the
  24 hours before the strike — usable as an early-warning threshold.
- Mexico's narrative spiked on presidential mentions and negative tone but
  lacked sustained military + crisis theme fusion: saber-rattling, not prep.
- Operational use: run daily; a similarity jump from ~20% to >60% within 48
  hours would constitute a high-confidence early-warning signal.

## Method
GDELT Global Knowledge Graph 2.0 via BigQuery -> 14-feature daily extraction
(SQL in Appendix A) -> StandardScaler -> KMO factorability test -> PCA fit on
the Venezuela baseline -> project Mexico data into the same vector space ->
Euclidean distance to the H-hour signature, converted to similarity via RBF.

## Limitations (read this section first if you're skeptical — I would be)
- **n=1 baseline.** The model is fit on a single intervention event. It is a
  proof of concept for the method, not a validated predictor. Additional
  historical baselines (Panama 1989, Libya 2011) are the obvious next step.
- **Variance coverage is unimpressive by construction.** "6 components capture
  100% of variance" is near-automatic with 14 features and a 30-90 day window;
  the meaningful result is PC1's dominance, not the coverage figure.
- **Theme dictionaries are blunt.** GDELT V2Themes are noisy; binary flags
  discard intensity within a theme.
- Estimative language follows ICD 203 conventions: this analysis supports
  "moderate confidence" at best given the single-baseline design.

## Findings
![Rhetoric vs Reality comparison chart](comparison.png)
*The comparison bar chart: Venezuela H-hour signature vs Mexico current, per
principal component, with the similarity score annotated.*

## Repository
- `Capstone_Project-PCA-Final.ipynb` — full analysis pipeline
- `Project_Report.md` — complete written report (WGU D502 capstone, Task 3)
- `Mexico-30_days.csv`, `Venezuela-Wide_net-90_days.csv` — extracted datasets
- Appendices: BigQuery extraction SQL, Python analysis code, data dictionary

*Capstone project, B.S. Data Analytics, Western Governors University, 2026.*
