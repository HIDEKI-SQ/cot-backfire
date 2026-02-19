# When Reasoning Traces Backfire (A0)

**Compliance Overrides Capability in Provided Chain-of-Thought Reasoning**

This repository contains code, data, and materials supporting Paper A0.

## Scope of This Release

This release corresponds specifically to **Paper A0**.

Other research threads (A1, A2, etc.) exist in this repository but are not required to reproduce A0 results and are considered separate research directions with independent releases.

---

## Paper A0: Core Contribution

> HIDEKI. "Compliance Overrides Capability: When Provided Reasoning Traces Make Stronger Models More Fragile." *Preprint* (2026).

### Research Question

What happens when language models consume externally provided reasoning traces of varying quality?

### Central Claim

Models do not verify provided reasoning traces—they comply with them. This compliance can override independent problem-solving capability and generate a backfire boundary (c\*), where provided reasoning becomes harmful.

### Key Findings

1. **Compliance, Not Verification**: When traces degrade from clean to corrupted, correct→wrong flips dominate (82:1 asymmetry).
2. **Capability Override**: Among problems solvable in Direct mode (no trace), 40.4% fail under corrupted traces.
3. **Backfire Boundary (c\*)**: Each model has a corruption threshold where trace consumption reduces accuracy below Direct baseline.
4. **Minimal Theory**: A simple mixture model explains the emergence of backfire. The criterion Δ = q₀ − p₀ < 0 predicts immediate fragility.

---

## Experimental Paradigm

### Provided-CoT Framework

We separate:
- **Trace generation**: A model produces step-by-step reasoning traces.
- **Trace consumption**: A target model receives (possibly corrupted) traces and produces answers.

Trace corruption rate `c` controls reasoning quality. Accuracy is measured relative to clean trace performance and Direct (no-trace) baseline.

### Data Summary (A0)

| Experiment | Model | Inferences |
|------------|-------|------------|
| I × c Grid | Claude Sonnet | 3,582 |
| Direct Baseline | Claude Sonnet | 199 |
| Fine-grained c | Claude Sonnet | 995 |
| Cross-model | GPT-4o | 1,393 |
| Curvature Probe | Mistral-7B | 597 |
| **Total** | | **5,373 primary + 597 auxiliary** |

---

## Reproducing A0 Results

### Notebooks

```
notebooks/
├── cot_experiment_full_v3.ipynb      # Exp 1: I × c Grid
├── cot_experiment_direct_v3.ipynb    # Exp 3: Direct Baseline
├── cot_experiment_acrit_v3.ipynb     # Exp 4: Fine-grained c
├── cot_experiment_kappa_v3.1.ipynb   # Exp 2: Curvature Probe
├── experiment_gpt4o.ipynb            # Exp 5: Cross-model
└── analysis.ipynb                    # Reproduction & figures
```

### Data

```
data/
├── claude/                           # Claude Sonnet results
│   ├── results_full_v3.json
│   ├── direct_results_v3.json
│   ├── acrit_results_v3.json
│   ├── kappa_results_v3_1.json
│   ├── clean_traces_I10_v3.json
│   └── problems_v3.json
└── gpt4o/                            # GPT-4o results
    ├── direct_results_gpt.json
    ├── cot_results_gpt.json
    └── summary_gpt.json
```

### Reproducibility Settings

- **Global seed**: `20251224`
- **Temperature**: `0`
- **Deterministic inference configuration**

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

All data and scripts necessary to reproduce the A0 results are included in this repository. A frozen release is archived via Zenodo.

DOI: *(to be inserted upon archival release)*

---

## Other Related Manuscripts

The following research threads share the provided-CoT experimental paradigm and are available in this repository as separate releases:

- **Paper A1**: Cross-model scaling of compliance vulnerability
- **Paper A2**: Length and cue effects in trace consumption
- **Paper A2-preliminary**: Initial exploration of cue dominance (archived)

These are independent research directions and are not part of the A0 evaluation scope.

---

## License

MIT License - see [LICENSE](LICENSE)

## Author

**HIDEKI**  
Independent Researcher, Japan  
ORCID: [0009-0002-0019-6608](https://orcid.org/0009-0002-0019-6608)  
Email: hideki@r3776.jp
