# When Reasoning Traces Backfire

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)

**Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning**

## Overview

This repository contains code, data, and materials for the paper:

> HIDEKI. "When Reasoning Traces Backfire: Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning." *Preprint* (2025). DOI: [10.5281/zenodo.XXXXXXX](https://doi.org/10.5281/zenodo.XXXXXXX)

We introduce a **provided-CoT paradigm** that separates reasoning generation from reasoning following, enabling controlled study of how language models respond to reasoning traces of varying quality.

## Key Findings

1. **Backfire Boundary**: We identify a point where provided CoT becomes counterproductive, performing worse than no CoT at all.

2. **Model-Dependent Threshold**: The boundary is not universal—it depends on the model's baseline (direct) capability. Models with weaker direct performance remain above their baseline longer even under corrupted traces.

3. **High Compliance**: Models exhibit strong compliance to provided traces, following corrupted reasoning even for problems they could solve without CoT (~40% failure rate at high corruption).

4. **Redundancy over Integration**: Longer traces provide resilience through redundancy (more surviving correct steps) rather than deeper cognitive integration.

## Repository Structure

```
cot-backfire/
├── README.md
├── LICENSE
├── data/
│   ├── claude/
│   │   ├── results_full_v3.json        # Experiment 1: I × λ grid
│   │   ├── direct_results_v3.json      # Experiment 3: Direct baseline
│   │   ├── acrit_results_v3.json       # Experiment 4: Fine-grained λ
│   │   ├── kappa_results_v3_1.json     # Experiment 2: Curvature
│   │   ├── clean_traces_I10_v3.json    # Clean reasoning traces
│   │   └── problems_v3.json            # Selected GSM8K problems
│   └── gpt4o/
│       ├── direct_results_gpt.json     # Direct baseline
│       ├── cot_results_gpt.json        # CoT conditions
│       └── summary_gpt.json            # Summary statistics
├── notebooks/
│   ├── cot_experiment_full_v3.ipynb    # Experiment 1: I × λ grid (Claude)
│   ├── cot_experiment_kappa_v3.1.ipynb # Experiment 2: Curvature (Mistral-7B)
│   ├── cot_experiment_direct_v3.ipynb  # Experiment 3: Direct baseline (Claude)
│   ├── cot_experiment_acrit_v3.ipynb   # Experiment 4: Fine-grained λ (Claude)
│   ├── experiment_gpt4o.ipynb          # Experiment 5: Cross-model (GPT-4o)
│   └── analysis.ipynb                  # Reproduction analysis
├── figures/
│   ├── fig1_heatmap.png
│   ├── fig2_accuracy_curve.png
│   ├── fig3_redundancy.png
│   ├── extended_fig1_kappa.png
│   └── extended_fig2_comparison.png
└── docs/
    └── manuscript.md
```

## Experiments

### Experiment 1: I × λ Grid (Claude Sonnet)
Full factorial design testing trace depth (I ∈ {5, 10, 15}) and corruption rate (λ ∈ {0.0, 0.2, 0.4, 0.6, 0.8, 1.0}).
- **Notebook**: `cot_experiment_full_v3.ipynb`
- **Data**: `results_full_v3.json`
- **Inferences**: 3,582

### Experiment 2: Curvature Measurement (Mistral-7B)
Hidden-state trajectory curvature (κ) as a candidate geometric signature.
- **Notebook**: `cot_experiment_kappa_v3.1.ipynb`
- **Data**: `kappa_results_v3_1.json`
- **Measurements**: 597

### Experiment 3: Direct Baseline (Claude Sonnet)
No-CoT baseline for comparison.
- **Notebook**: `cot_experiment_direct_v3.ipynb`
- **Data**: `direct_results_v3.json`
- **Inferences**: 199

### Experiment 4: Fine-grained λ (Claude Sonnet)
Additional λ values (0.1, 0.3, 0.5, 0.7, 0.9) for precise boundary estimation.
- **Notebook**: `cot_experiment_acrit_v3.ipynb`
- **Data**: `acrit_results_v3.json`
- **Inferences**: 995

### Experiment 5: Cross-model Validation (GPT-4o)
Replication with GPT-4o to test generalizability.
- **Notebook**: `experiment_gpt4o.ipynb`
- **Data**: `cot_results_gpt.json`, `direct_results_gpt.json`
- **Inferences**: 1,393

### Total: 6,766 inferences

## Corruption Protocol

| Type | Proportion | Description |
|------|------------|-------------|
| IRR (Irrelevant) | 20% | Task-unrelated computation |
| LOC (Local Error) | 40% | Small numerical errors |
| WRONG (Wrong Constraint) | 40% | Incorrect intermediate values |

## Results Summary

### Backfire Boundary

| Model | Direct Accuracy | Clean CoT | λ* | A* |
|-------|-----------------|-----------|-----|-----|
| Claude Sonnet | 75.9% | 97.0% | 0.45 | 0.55 |
| GPT-4o | 56.3% | 96.5% | 0.87 | 0.13 |

### Compliance Analysis

| Metric | Claude Sonnet | GPT-4o |
|--------|---------------|--------|
| Flip Rate (Clean→Wrong, λ=0.8) | 42.7% | 36.5% |
| McNemar χ² | 77.1 | 65.1 |
| p-value | < 10⁻¹⁸ | < 10⁻¹⁵ |

## Reproducibility

All experiments use deterministic settings:
- **Global Seed**: 20251224
- **Temperature**: 0
- **Deterministic Hashing**: For corruption assignment

### Quick Start

```bash
# Clone repository
git clone https://github.com/HIDEKI-SQ/cot-backfire.git
cd cot-backfire

# Open notebooks in Google Colab
# Run analysis.ipynb to reproduce main figures
```

### Requirements

- Python 3.8+
- Google Colab (recommended for experiments)
- API keys: Anthropic (Claude), OpenAI (GPT-4o)

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
transformers  # For Experiment 2 (κ measurement)
torch
```

## Citation

```bibtex
@article{hideki2025backfire,
  title={When Reasoning Traces Backfire: Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning},
  author={HIDEKI},
  journal={Preprint},
  year={2025},
  doi={10.5281/zenodo.XXXXXXX}
}
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**HIDEKI**  
Independent Researcher, Japan  
ORCID: [0009-0002-0019-6608](https://orcid.org/0009-0002-0019-6608)  
Contact: hideki@r3776.jp

## Acknowledgements

AI writing assistants were used for manuscript preparation.
