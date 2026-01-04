# When Reasoning Traces Backfire

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18050394.svg)](https://doi.org/10.5281/zenodo.18050394)

**Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning**

## Overview

This repository contains code, data, and materials for three related papers:

### Paper A0: Backfire Boundary
> HIDEKI. "When Reasoning Traces Backfire: Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning." *Preprint* (2025).  
> DOI: [10.5281/zenodo.18050394](https://doi.org/10.5281/zenodo.18050394)

### Paper A1: The Capability–Compliance Paradox
> HIDEKI. "The Capability–Compliance Paradox: Vulnerability to External Guidance Revealed by Language Models." *Preprint* (2025).  
> DOI: [10.5281/zenodo.18105329](https://doi.org/10.5281/zenodo.18105329)

### Paper A2: Cue-Conditional Length Effects (Main)
> HIDEKI. "Length Effects in Chain-of-Thought Are Conditional on Explicit Answer-Cue Availability." *Preprint* (2026).  
> DOI: [10.5281/zenodo.18144101](https://doi.org/10.5281/zenodo.18144101)

### Paper A2-preliminary: Cue-Dominant Extraction (Archived)
> HIDEKI. "Cue-Dominant Extraction Explains Length Effects in Corrupted Reasoning Traces." *Preprint* (2025).  
> DOI: [10.5281/zenodo.18132430](https://doi.org/10.5281/zenodo.18132430)

We introduce a **provided-CoT paradigm** that separates reasoning generation from reasoning following, enabling controlled study of how language models respond to reasoning traces of varying quality.

---

## Key Findings

### A0: Backfire Boundary
1. **Backfire Boundary**: A critical point where provided CoT becomes counterproductive
2. **Model-Dependent Threshold**: The boundary depends on baseline capability
3. **Redundancy over Integration**: Longer traces provide resilience through redundancy

### A1: The Capability–Compliance Paradox
1. **Compliance-Induced Failure (CIF)**: High-capability models abandon correct reasoning for erroneous external input (40% CIF rate, 38:0 asymmetry)
2. **Capability-Compliance Paradox**: Higher-capability models show greater vulnerability
3. **Verification Eliminates CIF**: VERIFY instructions fully restore performance (57.6% → 96.0%)
4. **Contamination Type Matters**: WRONG traces cause catastrophic failures (GPT-4o: 7.1%)

### A2: Cue-Conditional Length Effects (Main)
1. **Cue-Conditional Length Law**: Length effects exist only when answer cues are absent; with cues present, accuracy saturates regardless of trace length (∆L ≈ 0 pp)
2. **Dual-Path Processing Model**: Cue-preferential extraction (fast path) with fallback computation (slow path) when cues are unavailable or unreliable
3. **Corruption Sensitivity**: Models differ not in extraction ability but in threshold for abandoning extraction—Claude tolerates more corruption than GPT-4o
4. **Design Implication**: Cue integrity and corruption sensitivity calibration—not trace length—should be primary design targets for trace-consuming systems

### A2-preliminary: Cue-Dominant Extraction (Archived)
1. **Order Invariance**: Shuffling step order does not reduce accuracy (+7.5%, P=0.001), contradicting sequential integration
2. **Cue Dominance**: Final-answer cue status explains ~7× more variance than step position (31.7 vs 4.5 pp)
3. **Two-Stage Processing**: Models extract from cues first, fall back to redundancy when cues are corrupted

---

## Repository Structure

```
cot-backfire/
├── README.md
├── LICENSE
├── data/
│   ├── claude/                           # A0: Claude Sonnet baseline
│   │   ├── results_full_v3.json
│   │   ├── direct_results_v3.json
│   │   ├── acrit_results_v3.json
│   │   ├── kappa_results_v3_1.json
│   │   ├── clean_traces_I10_v3.json
│   │   └── problems_v3.json
│   ├── gpt4o/                            # A0: GPT-4o baseline
│   │   ├── direct_results_gpt.json
│   │   ├── cot_results_gpt.json
│   │   └── summary_gpt.json
│   ├── e2_contamination_type_20251228/   # A1 Study 3
│   ├── e3_verify_ignore_20251228/        # A1 Study 1-2
│   ├── e4_hard_subset_20251228/          # Extended analysis
│   ├── a3_claude_sonnet4_20251228/       # A1 Study 4
│   ├── a3_claude_haiku35_20251228/       # A1 Study 4
│   ├── a3_claude_haiku_20251228/         # A1 Study 4
│   ├── a3_gpt_gpt35_20251228/            # A1 Study 4
│   ├── a3_gpt_gpt4omini_20251228/        # A1 Study 4
│   ├── a3_scaling_pilot_20251227/        # A1 Study 4 pilot
│   ├── a3_statistical_analysis/          # A1 Study 4 analysis
│   │   ├── a3_complete_analysis.json
│   │   ├── table1_summary.csv
│   │   └── figures/
│   │
│   │   # === A2-preliminary experiments (E1-E5) ===
│   ├── E1_shuffle_20260102/              # A2-prelim E1: Shuffle (c=0.6)
│   │   └── results/
│   ├── E1b_shuffle_c04_20260102/         # A2-prelim E1b: Shuffle (c=0.4)
│   │   └── results/
│   ├── E2_position_20260102/             # A2-prelim E2: Position
│   │   └── results/
│   ├── E3_wrong_position_20260102/       # A2-prelim E3: WRONG Position
│   │   └── results/
│   ├── E4_late_protected_20260102/       # A2-prelim E4: Late-Protected (c=0.3)
│   │   └── results/
│   ├── E4prime_late_protected_c04_20260102/  # A2-prelim E4': Late-Protected (c=0.4)
│   │   └── results/
│   ├── E5_single_step_20260102/          # A2-prelim E5: Single-Step
│   │   └── results/
│   │
│   │   # === A2 main experiments (E6-E8) ===
│   ├── trace_generation_L5_L20_20260103/ # A2: Trace generation (L=5,10,15,20)
│   │   ├── L5_generation_log.json
│   │   ├── L20_generation_log.json
│   │   └── generation_summary.json
│   ├── E6_conflicting_cue_v2_20260103/   # A2 E6: Conflicting-cue (Claude)
│   │   └── results/
│   │       ├── E6_conflicting_cue_results.json
│   │       ├── E6_conflicting_cue_traces.json
│   │       └── E6_summary.json
│   ├── A2_GPT4o_E6_v2_20260103/          # A2 E6: Conflicting-cue (GPT-4o)
│   │   └── results/
│   │       ├── A2_GPT4o_E6_results.json
│   │       ├── A2_GPT4o_E6_summary.json
│   │       └── A2_GPT4o_E6_traces.json
│   ├── E7_trace_only_v2_20260103/        # A2 E7: Trace-only (Claude)
│   │   └── results/
│   │       ├── E7_trace_only_results.json
│   │       ├── E7_trace_only_traces.json
│   │       └── E7_summary.json
│   ├── A2_GPT4o_E7_trace_only_20260103/  # A2 E7: Trace-only (GPT-4o)
│   │   └── results/
│   │       ├── A2_GPT4o_E7_results.json
│   │       ├── A2_GPT4o_E7_summary.json
│   │       └── A2_GPT4o_E7_traces.json
│   ├── E8_length_cue_v2_20260103/        # A2 E8: Length × Cue × Corruption (Claude)
│   │   └── results/
│   │       ├── E8_length_cue_results.json
│   │       ├── E8_length_cue_figure.png
│   │       ├── E8_traces_sample.json
│   │       └── E8_summary.json
│   └── E8prime_GPT4o_v2_20260103/        # A2 E8': Cross-model (GPT-4o)
│       └── results/
│           ├── E8prime_GPT4o_results.json
│           └── E8prime_GPT4o_summary.json
│
├── notebooks/
│   │   # === A0 experiments ===
│   ├── cot_experiment_full_v3.ipynb      # A0 Exp 1
│   ├── cot_experiment_kappa_v3.1.ipynb   # A0 Exp 2
│   ├── cot_experiment_direct_v3.ipynb    # A0 Exp 3
│   ├── cot_experiment_acrit_v3.ipynb     # A0 Exp 4
│   ├── experiment_gpt4o.ipynb            # A0 Exp 5
│   │
│   │   # === A1 experiments ===
│   ├── e2_contamination_type.ipynb       # A1 Study 3
│   ├── e3_verify_ignore.ipynb            # A1 Study 1-2
│   ├── e4_hard_subset.ipynb              # Extended
│   ├── a3_scaling_law_claude_full.ipynb  # A1 Study 4
│   ├── a3_scaling_law_gpt_full.ipynb     # A1 Study 4
│   ├── a3_scaling_law_gpt35_pilot.ipynb  # A1 Study 4 pilot
│   ├── a3_statistical_analysis.ipynb     # A1 Study 4
│   │
│   │   # === A2-preliminary experiments (E1-E5) ===
│   ├── E1_shuffle_experiment.ipynb       # A2-prelim E1
│   ├── E1b_shuffle_c04_experiment.ipynb  # A2-prelim E1b
│   ├── E2_position_experiment.ipynb      # A2-prelim E2
│   ├── E3_wrong_position_experiment.ipynb    # A2-prelim E3
│   ├── E4_late_protected_experiment.ipynb    # A2-prelim E4
│   ├── E4prime_late_protected_c04_experiment.ipynb  # A2-prelim E4'
│   ├── E5_single_step_experiment.ipynb   # A2-prelim E5
│   │
│   │   # === A2 main experiments (E6-E8) ===
│   ├── L5_L20_trace_generation.ipynb     # A2: Trace generation
│   ├── E6_conflicting_cue_v2_FIXED.ipynb # A2 E6: Conflicting-cue (Claude)
│   ├── A2_GPT4o_E6_v2_FIXED.ipynb        # A2 E6: Conflicting-cue (GPT-4o)
│   ├── E7_trace_only_v2_FIXED.ipynb      # A2 E7: Trace-only (Claude)
│   ├── A2_GPT4o_E7_trace_only.ipynb      # A2 E7: Trace-only (GPT-4o)
│   ├── E8_length_cue_v2_FIXED.ipynb      # A2 E8: Length × Cue × Corruption (Claude)
│   ├── E8prime_GPT4o_v2_FIXED.ipynb      # A2 E8': Cross-model (GPT-4o)
│   │
│   └── analysis.ipynb                    # Reproduction
│
├── figures/
│   ├── A2/                               # A2 main paper figures
│   │   ├── fig1_e8_main_result.pdf
│   │   ├── fig1_e8_main_result.png
│   │   ├── fig2_e7_trace_only.pdf
│   │   ├── fig2_e7_trace_only.png
│   │   ├── fig3_e6_conflicting_cue.pdf
│   │   ├── fig3_e6_conflicting_cue.png
│   │   ├── fig4_corruption_sensitivity.pdf
│   │   ├── fig4_corruption_sensitivity.png
│   │   ├── fig5_dual_path_model.pdf
│   │   └── fig5_dual_path_model.png
│   ├── A2_Preliminary/                   # A2-preliminary paper figures (archived)
│   │   ├── fig1_experimental_design.pdf
│   │   ├── fig2_main_results.pdf
│   │   ├── fig3_order_vs_position.pdf
│   │   └── fig4_mechanistic_model.pdf
│   └── ...
└── docs/
```

---

## A2 Experiments (Main)

### Trace Generation (L=5, 10, 15, 20)
- **Notebook**: `L5_L20_trace_generation.ipynb`
- **Data**: `trace_generation_L5_L20_20260103/`
- **Design**: Generate reasoning traces at four length levels for factorial experiments
- **N**: 188 problems × 4 lengths

### E6: Conflicting-Cue Interventions
- **Notebooks**: `E6_conflicting_cue_v2_FIXED.ipynb` (Claude), `A2_GPT4o_E6_v2_FIXED.ipynb` (GPT-4o)
- **Data**: `E6_conflicting_cue_v2_20260103/`, `A2_GPT4o_E6_v2_20260103/`
- **Design**: Test model response when cue and reasoning conflict
  - Condition A: Wrong cue + clean reasoning
  - Condition B: Correct cue + corrupted reasoning (c=0.8)
  - Control: Clean trace with correct cue
- **N**: 199 problems × 3 conditions × 2 models

### E7: Trace-Only Extraction Test
- **Notebooks**: `E7_trace_only_v2_FIXED.ipynb` (Claude), `A2_GPT4o_E7_trace_only.ipynb` (GPT-4o)
- **Data**: `E7_trace_only_v2_20260103/`, `A2_GPT4o_E7_trace_only_20260103/`
- **Design**: Test extraction by withholding problem statement
  - Condition A: Trace + Cue (no problem)
  - Condition B: Trace only (no problem, no cue)
  - Condition C: Full context (problem + trace + cue)
- **N**: 199 problems × 3 conditions × 2 models

### E8: Length × Cue × Corruption Factorial (Claude)
- **Notebook**: `E8_length_cue_v2_FIXED.ipynb`
- **Data**: `E8_length_cue_v2_20260103/`
- **Design**: Full factorial manipulation
  - Length: L ∈ {5, 10, 15, 20}
  - Cue: Present vs Absent
  - Corruption: c ∈ {0.4, 0.8}
- **N**: 188 problems × 4 lengths × 2 cue × 2 corruption = ~3,008 inferences

### E8': Cross-Model Replication (GPT-4o)
- **Notebook**: `E8prime_GPT4o_v2_FIXED.ipynb`
- **Data**: `E8prime_GPT4o_v2_20260103/`
- **Design**: Replicate E8 factorial with GPT-4o
- **N**: 188 problems × 4 lengths × 2 cue × 2 corruption = ~3,008 inferences

---

## A2 Results Summary (Main)

### E6: Conflicting-Cue Results

| Condition | Claude | GPT-4o | Interpretation |
|-----------|--------|--------|----------------|
| A: Wrong cue + clean | 95.5% | 90.5% | Rejects inconsistent cue |
| B: Correct cue + corrupted | 99.5% | 75.9% | Claude trusts cue; GPT-4o triggers fallback |
| Cue-following rate (A) | 1.5% | 6.0% | Low = active rejection |

**Finding**: Models are not blind extractors—they assess cue reliability.

### E7: Trace-Only Results

| Condition | Claude | GPT-4o |
|-----------|--------|--------|
| A: Trace + Cue (no problem) | 98.5% | 99.5% |
| B: Trace only (no cue) | 87.4% | 83.4% |
| C: Full context | 98.5% | 99.5% |
| **Cue effect** | +11.1 pp | +16.1 pp |
| **Problem effect** | ~0 pp | ~0 pp |

**Finding**: Problem statement provides no additional benefit—cue extraction dominates.

### E8/E8': Length × Cue Interaction

| Model | Cue Status | c | ∆L (L=20 − L=5) | Pattern |
|-------|------------|---|-----------------|---------|
| Claude | Present | 0.4 | −0.5 pp | Flat (ceiling) |
| Claude | Present | 0.8 | 0.0 pp | Flat (ceiling) |
| Claude | Absent | 0.4 | +14.4 pp | Strong length effect |
| Claude | Absent | 0.8 | +16.0 pp | Strong length effect |
| GPT-4o | Present | 0.8 | +13.8 pp | Residual length effect |
| GPT-4o | Absent | 0.8 | +15.2 pp | Strong length effect |

**Finding**: Length effects are cue-conditional. GPT-4o shows residual length effect even with cue present due to higher corruption sensitivity.

### Corruption Sensitivity Comparison

| Model | E6-B Accuracy | Interpretation |
|-------|---------------|----------------|
| Claude | 99.5% | Low sensitivity (trusts cue despite corruption) |
| GPT-4o | 75.9% | High sensitivity (triggers fallback) |

**Finding**: Same dual-path architecture, different sensitivity thresholds.

---

## A2-preliminary Experiments (Archived)

These experiments represent the initial exploration that led to the main A2 findings.

### E1/E1b: Shuffle Effect
- **Design**: Test whether step order matters
- **Finding**: Shuffling does not reduce accuracy (+7.5% at c=0.4)

### E2/E3: Position Effect
- **Design**: Early vs late corruption
- **Finding**: Late corruption is catastrophic (−36.2 pp), but confounded with cue destruction

### E4/E4': Cue Protection
- **Design**: Corrupt late steps while protecting final-answer cue
- **Finding**: Cue protection recovers ~87% of accuracy; position effect is only ~13%

### E5: Single-Step Isolation
- **Design**: Corrupt only Step 1 or only Step 10
- **Finding**: 22:0 asymmetry—Step 10 (cue) is uniquely important

---

## A1 Experiments

### Study 1-2: Instruction Gating (E3)
- **Notebook**: `e3_verify_ignore.ipynb`
- **Data**: `e3_verify_ignore_20251228/`
- **Conditions**: DIRECT, USE, VERIFY, IGNORE
- **Models**: Claude 4 Sonnet, GPT-4o, Claude 3.5 Haiku
- **N**: 99 problems × 4 conditions × 3 models

### Study 3: Contamination Type (E2)
- **Notebook**: `e2_contamination_type.ipynb`
- **Data**: `e2_contamination_type_20251228/`
- **Types**: IRR, LOC, WRONG
- **Models**: Claude 4 Sonnet, GPT-4o, Claude 3.5 Haiku
- **N**: 198 problems × 3 types × 3 λ × 3 models

### Study 4: Cross-Model Scaling (A3)
- **Notebooks**: `a3_scaling_law_claude_full.ipynb`, `a3_scaling_law_gpt_full.ipynb`, `a3_statistical_analysis.ipynb`
- **Data**: `a3_*_20251228/`, `a3_statistical_analysis/`
- **Models**: 6 models (Claude 3/3.5/4, GPT-3.5/4o-mini/4o)
- **N**: 199 problems × 6 λ × 6 models

---

## A0 Experiments

### Experiment 1: I × λ Grid (Claude Sonnet)
- **Notebook**: `cot_experiment_full_v3.ipynb`
- **Inferences**: 3,582

### Experiment 2: Curvature (Mistral-7B)
- **Notebook**: `cot_experiment_kappa_v3.1.ipynb`
- **Measurements**: 597

### Experiment 3: Direct Baseline (Claude Sonnet)
- **Notebook**: `cot_experiment_direct_v3.ipynb`
- **Inferences**: 199

### Experiment 4: Fine-grained λ (Claude Sonnet)
- **Notebook**: `cot_experiment_acrit_v3.ipynb`
- **Inferences**: 995

### Experiment 5: Cross-model (GPT-4o)
- **Notebook**: `experiment_gpt4o.ipynb`
- **Inferences**: 1,393

---

## Reproducibility

All experiments use deterministic settings:
- **Global Seed**: 20251224
- **Temperature**: 0

### Quick Start

```bash
git clone https://github.com/HIDEKI-SQ/cot-backfire.git
cd cot-backfire
# Open notebooks in Google Colab
```

### Requirements

- Python 3.8+
- API keys: Anthropic, OpenAI

### Dependencies

```
datasets
anthropic
openai
numpy
pandas
scipy
matplotlib
tqdm
```

---

## Citations

### A0: Backfire Boundary
```bibtex
@article{hideki2025backfire,
  title={When Reasoning Traces Backfire: Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning},
  author={HIDEKI},
  journal={Preprint},
  year={2025},
  doi={10.5281/zenodo.18050394}
}
```

### A1: The Capability–Compliance Paradox
```bibtex
@article{hideki2025paradox,
  title={The Capability--Compliance Paradox: Vulnerability to External Guidance Revealed by Language Models},
  author={HIDEKI},
  journal={Preprint},
  year={2025},
  doi={10.5281/zenodo.18105329}
}
```

### A2: Cue-Conditional Length Effects (Main)
```bibtex
@article{hideki2026cue,
  title={Length Effects in Chain-of-Thought Are Conditional on Explicit Answer-Cue Availability},
  author={HIDEKI},
  journal={Preprint},
  year={2026},
  doi={10.5281/zenodo.18144101}
}
```

### A2-preliminary: Cue-Dominant Extraction (Archived)
```bibtex
@article{hideki2025extraction,
  title={Cue-Dominant Extraction Explains Length Effects in Corrupted Reasoning Traces},
  author={HIDEKI},
  journal={Preprint},
  year={2025},
  doi={10.5281/zenodo.18132430}
}
```

---

## License

MIT License - see [LICENSE](LICENSE)

## Author

**HIDEKI**  
Independent Researcher, Japan  
ORCID: [0009-0002-0019-6608](https://orcid.org/0009-0002-0019-6608)

## Acknowledgements

AI writing assistants were used for manuscript preparation.

---

## Version History

- **v1.0.0** (2025-12-24): Initial release (A0)
- **v1.0.A1** (2025-12-30): Added A1 experiments (E2, E3, A3)
- **v1.1.A1** (2025-12-31): Updated A1 title to "The Capability–Compliance Paradox"
- **v1.0.A2** (2026-01-02): Added A2-preliminary experiments (E1-E5)
- **v1.1.A2** (2026-01-04): Added A2 main experiments (E6-E8) and updated paper to "Length Effects Are Cue-Conditional"
