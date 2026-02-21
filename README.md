# When Reasoning Traces Backfire (A1)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXXX)

**Verification Affordance Governs Language Model Vulnerability to Corrupted Reasoning**

This repository contains code, data, and materials supporting Paper A1.

## Scope of This Release

This release corresponds specifically to **Paper A1**.

Other research threads (A0, A2, etc.) exist in this repository but are not required to reproduce A1 results and are considered separate research directions with independent releases.

---

## Paper A1: Core Contribution

> HIDEKI. "Verification affordance, not trace quality, governs language model vulnerability to corrupted reasoning." *Submitted to Nature Machine Intelligence* (2026).

### Research Question

What determines whether a language model adopts or rejects corrupted external reasoning—and does the resulting failure reflect graded degradation or categorical switching?

### Central Claim

Override vulnerability is a structural property of the task–model interface. Verification affordance—the degree to which a task permits independent answer verification—is the sole governing factor; trace quality, source authority, and model confidence have no predictive association.

### Key Findings

1. **Two Regimes**: Contamination-induced failure (CIF) ranges from 0% on verifiable tasks to 61% on verification-costly tasks.
2. **Binary Switching**: 100% of CIF cases on verification-costly tasks reflect direct adoption of the trace's wrong answer—not graded degradation. There is no intermediate state.
3. **Continuous Verification Cost**: Within verifiable domains, CIF increases monotonically with task complexity (*p* = .013–.031) and decreasing error detectability (*r* = 0.95).
4. **Capability Modulates Frequency, Not Mechanism**: Frontier models show lower CIF rates (9.8%) than mid-tier (31.0%) or base models (29.3%), but adoption is 100% when override occurs.
5. **Three Null Predictors**: Source authority, model confidence, and sequential exposure show no detectable effect on CIF.
6. **Recovery**: Explicit challenge interventions recover up to 100% of CIF cases.

---

## Experimental Paradigm

### Counterfactual Paired Evaluation

We compare model answers under two conditions on identical problems:
- **DIRECT**: Model answers without any external trace.
- **TRACE**: Model receives a corrupted reasoning trace (λ = 0.8) before answering.

**Contamination-Induced Failure (CIF)** = correct under DIRECT, incorrect under TRACE.

### Experimental Scale (A1)

| Experiment | Design | Models | Observations |
|------------|--------|--------|--------------|
| exp_B | Domain comparison (4 benchmarks) | 2 frontier | 800 |
| E2 | Complexity stratification (GSM8K) | 2 frontier | 300 |
| E4 | Error detectability (GSM8K) | 2 frontier | ~320 |
| E5 | Source authority (6 conditions) | 2 frontier | ~1,200 |
| E6/E6' | Trace quality | 2 frontier | ~400 |
| E7 | Sequential exposure (3 rounds) | 2 frontier | ~600 |
| E8 | Capability tier (5 models) | 5 models | ~2,000 |
| E9 | Confidence elicitation | 2 frontier | ~400 |
| E10 | Recovery interventions (3 types) | 2 frontier | ~300 |
| **Total** | **28 experiments** | **5 models** | **71,168 trials** |

### Models

| Tier | Models |
|------|--------|
| Frontier | Claude Sonnet 4 (Anthropic), GPT-4o (OpenAI) |
| Mid-tier | Claude Haiku 3.5 (Anthropic), GPT-4o-mini (OpenAI) |
| Base | GPT-3.5-turbo (OpenAI) |

### Benchmarks

| Dataset | Domain | Verification Affordance |
|---------|--------|------------------------|
| GSM8K | Mathematical reasoning | Verifiable (re-computation) |
| ARC-Challenge | Science reasoning | Partially verifiable |
| CommonsenseQA | Commonsense reasoning | Verification-costly |
| HellaSwag | Commonsense completion | Verification-costly |
| StrategyQA | Strategy questions | Supplementary |

---

## Reproducing A1 Results

### Notebooks

```
notebooks/A1/
├── exp_B_domain_comparison.ipynb        # Main domain comparison (4 benchmarks × 2 models)
├── E2_complexity_stratification.ipynb   # Task complexity gradient (GSM8K)
├── E4_error_detectability.ipynb         # Error type classification
├── E5_source_authority.ipynb            # Authority manipulation (6 conditions)
├── E6_trace_quality.ipynb               # Trace quality variation
├── E7_sequential_exposure.ipynb         # Repeated contamination (3 rounds)
├── E8_capability_tier.ipynb             # 5-model capability comparison
├── E9_confidence_elicitation.ipynb      # Self-reported confidence
├── E10_recovery_interventions.ipynb     # Challenge / re-derivation / verification
├── statistical_analysis.ipynb           # All statistical tests and figures
└── figure_generation.ipynb              # Publication figures (Fig. 1–5)
```

### Data

```
data/A1/
├── exp_B/                               # Domain comparison results
│   ├── sonnet4/
│   └── gpt4o/
├── E2_complexity/                       # Complexity stratification
├── E4_detectability/                    # Error detectability
├── E5_authority/                        # Source authority conditions
├── E6_quality/                          # Trace quality variation
├── E7_sequential/                       # Sequential exposure
├── E8_capability/                       # 5-model comparison
│   ├── sonnet4/
│   ├── gpt4o/
│   ├── haiku35/
│   ├── gpt4omini/
│   └── gpt35turbo/
├── E9_confidence/                       # Confidence ratings
├── E10_recovery/                        # Recovery interventions
└── statistical_analysis/                # Aggregated results and figures
    ├── all_experiments_summary.json
    └── figures/
```

### Figures

```
figures/A1/
├── fig1_conceptual_design.pdf           # Counterfactual paradigm
├── fig2_two_regimes.pdf                 # Domain CIF + binary switching
├── fig3_continuous_cost.pdf             # Complexity + detectability gradients
├── fig4_capability.pdf                  # 5-model CIF + adoption rates
└── fig5_null_recovery.pdf               # Null predictors + recovery
```

### Reproducibility Settings

- **Temperature**: 0
- **Deterministic inference configuration**
- **Fixed random seeds per experiment**

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

## Data & Code Availability

All data and scripts necessary to reproduce the A1 results are included in this repository. A frozen release is archived via Zenodo.

DOI: *(to be updated upon archival release)*

---

## Other Related Manuscripts

The following research threads share the provided-CoT experimental paradigm and are available in this repository as separate releases:

- **Paper A0**: Backfire boundary and compliance override in provided reasoning
- **Paper A2**: Length and cue effects in trace consumption
- **Paper A2-preliminary**: Initial exploration of cue dominance (archived)

These are independent research directions and are not part of the A1 evaluation scope.

---

## Citation

```bibtex
@article{hideki2026verification,
  title={Verification affordance, not trace quality, governs language model vulnerability to corrupted reasoning},
  author={HIDEKI},
  journal={Submitted to Nature Machine Intelligence},
  year={2026}
}
```

---

## License

MIT License - see [LICENSE](LICENSE)

## Author

**HIDEKI**  
Independent Researcher, Japan  
ORCID: [0009-0002-0019-6608](https://orcid.org/0009-0002-0019-6608)  
Email: hideki@r3776.jp
