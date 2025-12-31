# When Reasoning Traces Backfire

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18050394.svg)](https://doi.org/10.5281/zenodo.18050394)

**Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning**

## Overview

This repository contains code, data, and materials for two related papers:

### Paper A0: Backfire Boundary
> HIDEKI. "When Reasoning Traces Backfire: Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning." *Preprint* (2025).  
> DOI: [10.5281/zenodo.18050394](https://doi.org/10.5281/zenodo.18050394)

### Paper A1: The Capability–Compliance Paradox
> HIDEKI. "The Capability–Compliance Paradox: Vulnerability to External Guidance Revealed by Language Models." *Preprint* (2025).  
> DOI: [10.5281/zenodo.18105329](https://doi.org/10.5281/zenodo.18105329)

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
│   └── a3_statistical_analysis/          # A1 Study 4 analysis
│       ├── a3_complete_analysis.json
│       ├── table1_summary.csv
│       └── figures/
├── notebooks/
│   ├── cot_experiment_full_v3.ipynb      # A0 Exp 1
│   ├── cot_experiment_kappa_v3.1.ipynb   # A0 Exp 2
│   ├── cot_experiment_direct_v3.ipynb    # A0 Exp 3
│   ├── cot_experiment_acrit_v3.ipynb     # A0 Exp 4
│   ├── experiment_gpt4o.ipynb            # A0 Exp 5
│   ├── e2_contamination_type.ipynb       # A1 Study 3
│   ├── e3_verify_ignore.ipynb            # A1 Study 1-2
│   ├── e4_hard_subset.ipynb              # Extended
│   ├── a3_scaling_law_claude_full.ipynb  # A1 Study 4
│   ├── a3_scaling_law_gpt_full.ipynb     # A1 Study 4
│   ├── a3_scaling_law_gpt35_pilot.ipynb  # A1 Study 4 pilot
│   ├── a3_statistical_analysis.ipynb     # A1 Study 4
│   └── analysis.ipynb                    # Reproduction
├── figures/
└── docs/
```
Note: The repository also contains exploratory analyses, pilot experiments, and artifacts
not directly used in the papers. These are included for transparency and reproducibility.

The figures/ directory contains only figures that appear in the corresponding papers.
All other plots generated during experimentation are kept in data/ as exploratory artifacts.

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

## A1 Results Summary

### Study 1: Transition Patterns (N=99, λ=0.8)

| Model | DIRECT | USE | CIF | Recovery | Asymmetry |
|-------|--------|-----|-----|----------|-----------|
| Claude 4 Sonnet | 96.0% | 57.6% | 38 (40.0%) | 0 (0%) | 38:0*** |
| GPT-4o | 60.6% | 56.6% | 19 (31.7%) | 15 (38.5%) | 19:15 ns |
| Claude 3.5 Haiku | 44.4% | 89.9% | 5 (11.4%) | 50 (90.9%) | 5:50*** |

### Study 2: Instruction Gating (N=99, λ=0.8)

| Model | DIRECT | USE | VERIFY | IGNORE |
|-------|--------|-----|--------|--------|
| Claude 4 Sonnet | 96.0% | 57.6% | 96.0% | 94.9% |
| GPT-4o | 60.6% | 56.6% | 90.9% | 65.7% |
| Claude 3.5 Haiku | 44.4% | 89.9% | 87.9% | 92.9% |

### Study 3: Contamination Type (N=198, λ=1.0)

| Model | Baseline | IRR | LOC | WRONG |
|-------|----------|-----|-----|-------|
| Claude 4 Sonnet | 92.5% | 93.4% | 89.4% | 91.9% |
| Claude 3.5 Haiku | 46.7% | 86.4% | 77.8% | 31.3% |
| GPT-4o | 56.3% | 56.1% | 42.9% | 7.1% |

### Study 4: Capability-Compliance Paradox (N=199)

| Model | Baseline | Harm Ratio |
|-------|----------|------------|
| Claude 4 Sonnet | 92.5% | 40.8% |
| GPT-4o | 56.3% | 4.8% |
| Claude 3.5 Haiku | 46.7% | 0% |
| GPT-3.5 Turbo | 34.7% | 9.5% |
| Claude 3 Haiku | 32.2% | 6.4% |
| GPT-4o-mini | 30.7% | 3.3% |

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
  doi={後で追記}
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
