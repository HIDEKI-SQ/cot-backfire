# When Reasoning Traces Backfire

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)

**Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning**

## Overview

This repository contains code, data, and materials for the paper:

> HIDEKI. "When Reasoning Traces Backfire: Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning." *Preprint* (2025).

We introduce a **provided-CoT paradigm** that separates reasoning generation from reasoning following, enabling controlled study of how language models respond to reasoning traces of varying quality.

## Key Findings

1. **Backfire Boundary**: We identify a point where provided CoT becomes counterproductive, performing worse than no CoT at all.

2. **Model-Dependent Threshold**: The boundary is not universal—it depends on the model's baseline (direct) capability. Models with weaker direct performance remain above their baseline longer even under corrupted traces.

3. **High Compliance**: Models exhibit strong compliance to provided traces, following corrupted reasoning even for problems they could solve without CoT (40% failure rate at high corruption).

4. **Redundancy over Integration**: Longer traces provide resilience through redundancy (more surviving correct steps) rather than deeper cognitive integration.

## Repository Structure

```
├── README.md
├── LICENSE
├── data/
│   ├── claude/
│   │   ├── results_full_v3.json      # Experiment 1: I × λ grid
│   │   ├── direct_results_v3.json    # Experiment 3: Direct baseline
│   │   ├── acrit_results_v3.json     # Experiment 4: Fine-grained λ
│   │   └── kappa_results_v3_1.json   # Experiment 2: Curvature
│   └── gpt4o/
│       ├── direct_results_gpt.json   # Direct baseline
│       └── cot_results_gpt.json      # CoT conditions
├── notebooks/
│   ├── experiment_claude.ipynb       # Claude experiments
│   └── experiment_gpt4o.ipynb        # GPT-4o replication
├── figures/
│   ├── fig1_heatmap.png
│   ├── fig2_accuracy_curve.png
│   ├── fig3_redundancy.png
│   ├── extended_fig1_kappa.png
│   └── extended_fig2_comparison.png
└── docs/
    └── manuscript.md
```

## Experimental Design

### Conditions

- **Models**: Claude Sonnet, GPT-4o
- **Problems**: 200 GSM8K math problems (seed = 20251224)
- **Trace Depth (I)**: 5, 10, 15 steps
- **Corruption Rate (λ)**: 0.0, 0.2, 0.4, 0.6, 0.8, 1.0
- **Alignment (A)**: A = 1 - λ

### Corruption Types

| Type | Proportion | Description |
|------|------------|-------------|
| IRR (Irrelevant) | 20% | Task-unrelated computation |
| LOC (Local Error) | 40% | Small numerical errors |
| WRONG (Wrong Constraint) | 40% | Incorrect intermediate values |

### Total Inferences

| Experiment | Model | N |
|------------|-------|---|
| I × λ Grid | Claude | 3,582 |
| Curvature (κ) | Mistral-7B | 597 |
| Direct Baseline | Claude | 199 |
| Fine-grained λ | Claude | 995 |
| Replication | GPT-4o | 1,393 |
| **Total** | | **6,766** |

## Results Summary

### Backfire Boundary

| Model | Direct Accuracy | Clean CoT | λ* | A* |
|-------|-----------------|-----------|-----|-----|
| Claude Sonnet | 75.9% | 97.0% | 0.45 | 0.55 |
| GPT-4o | 56.3% | 96.5% | 0.87 | 0.13 |

### Compliance Analysis

| Metric | Claude | GPT-4o |
|--------|--------|--------|
| Flip Rate (Clean→Wrong, λ=0.8) | 42.7% | 36.5% |
| McNemar χ² | 77.1 | 65.1 |
| p-value | < 10⁻¹⁸ | < 10⁻¹⁵ |

## Reproducibility

All experiments use deterministic settings:
- **Global Seed**: 20251224
- **Temperature**: 0
- **Deterministic Hashing**: For corruption assignment

To reproduce:

```bash
# Clone repository
git clone https://github.com/[username]/cot-backfire.git
cd cot-backfire

# Open in Google Colab
# Upload notebooks/experiment_claude.ipynb
# Run all cells
```

## Requirements

- Python 3.8+
- Google Colab (recommended)
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
