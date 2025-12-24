# When Reasoning Traces Backfire: Identifying the Backfire Boundary of Provided Chain-of-Thought Reasoning

**HIDEKI**

Affiliation: Independent Researcher, Japan  
ORCID: 0009-0002-0019-6608  
Correspondence: hideki@r3776.jp

Preprint DOI:  
Code & Data DOI:  
Repository:  

---

## Abstract

Chain-of-thought (CoT) prompting has become a standard technique for improving reasoning in large language models. However, conditions under which CoT fails—or becomes counterproductive—remain poorly characterized. Here we introduce a "provided-CoT" paradigm that separates reasoning generation from reasoning following, enabling controlled study of how models respond to reasoning traces of varying quality.

Using systematic corruption of reasoning steps (N = 5,373 inferences across 200 mathematical problems), we identify a backfire boundary where provided CoT performs worse than no CoT. This boundary is not a universal threshold; it is the crossing point between trace-aided performance and the model's direct baseline, shifting with baseline capability. Cross-model validation with GPT-4o confirmed this: despite different boundary positions (Claude λ* ≈ 0.45 vs. GPT-4o λ* ≈ 0.87), both models exhibited comparably strong compliance to provided traces.

Strikingly, models follow corrupted reasoning even for problems they could solve without CoT, causing 40% of such problems to fail at high corruption. Longer traces provide resilience through redundancy rather than deeper integration. A trajectory curvature probe (κ) did not distinguish successful from failing reasoning under our measurement protocol, constraining geometric hypotheses for failure mechanisms.

These findings establish that the backfire boundary depends on both trace quality and baseline capability, with practical implications for retrieval-augmented and multi-agent systems.

---

## Introduction

Chain-of-thought (CoT) prompting represents one of the most significant advances in large language model capabilities. By eliciting explicit intermediate reasoning steps, CoT dramatically improves performance on mathematical, logical, and commonsense reasoning tasks [1, 2]. This success has led to widespread adoption across applications, from educational tutoring to scientific problem-solving, and has spawned numerous extensions including self-consistency methods [3], tree-structured reasoning [4], and least-to-most prompting [5].

Yet research on CoT has focused almost exclusively on improvement: how to generate better reasoning traces, how to select among multiple traces, and how to extend CoT to new domains. A complementary question—under what conditions does CoT fail, or even hurt performance?—remains largely unexplored. Recent work has raised concerns about the faithfulness of CoT explanations [6, 7] and the reliability of step-by-step reasoning [8], but systematic quantification of failure conditions is lacking.

This gap has practical significance. In retrieval-augmented generation (RAG) systems [9, 10], reasoning traces may be retrieved from noisy sources. In multi-agent systems [11, 12], one agent's reasoning becomes another's input. In both cases, the quality of provided reasoning cannot be guaranteed, yet the consequences of following corrupted reasoning remain poorly understood.

To address this gap, we introduce a **provided-CoT paradigm** that separates reasoning generation from reasoning following. Rather than having models generate their own CoT, we provide reasoning traces as input and measure only the model's ability to follow them to a correct answer. This separation enables precise experimental control: we can systematically corrupt traces while holding problem content constant, quantifying exactly how reasoning quality affects downstream performance.

Our approach manipulates two key variables. **Alignment** (A) measures the fraction of reasoning steps that are correct, ranging from A = 1.0 (all steps correct) to A = 0.0 (all steps corrupted). **Trace depth** (I) is the number of reasoning steps provided. We corrupt traces through three mechanisms: irrelevant insertions, local computational errors, and incorrect constraint impositions.

Our experiments reveal three main findings. First, we identify a **backfire boundary**: a point where provided CoT performs worse than no CoT at all. Crucially, this boundary is not a universal threshold but depends on the model's baseline (direct) capability—models with weaker direct performance remain above their baseline longer even under corrupted traces. Second, models exhibit remarkably high **compliance** with provided traces; even when models could solve problems without CoT, corrupted traces cause systematic failures. Third, the benefit of longer traces reflects **redundancy** rather than deeper integration: more steps provide more opportunities for correct information to survive corruption.

We also probed whether trajectory curvature in hidden states might serve as a geometric signature distinguishing successful from failing reasoning. Under our measurement protocol, this probe yielded no significant differentiation, constraining hypotheses about internal mechanisms of CoT failure.

These findings have immediate practical implications. They establish that the backfire boundary depends on both trace quality and baseline capability, provide a framework for assessing when CoT should be trusted, and highlight previously underappreciated risks in systems that aggregate or retrieve reasoning from external sources.

---

## Results

### Provided CoT achieves high accuracy when aligned

We first established that the provided-CoT paradigm successfully leverages high-quality reasoning traces. When given clean (uncorrupted) traces, the model achieved 97.0% accuracy across all trace depths (I = 5, 10, 15), compared to 75.9% for the Direct condition where no reasoning trace was provided (Fig. 1; Extended Data Table 1). This 21-percentage-point improvement (Cohen's h = 0.68 [13], medium-to-large effect) confirms that models effectively utilize provided reasoning when it is reliable.

Notably, accuracy at A = 1.0 was invariant to trace depth: whether given 5, 10, or 15 reasoning steps, the model achieved identical 97.0% accuracy. This ceiling effect indicates that, for this task, the quality of reasoning matters more than its quantity when traces are fully aligned.

### Accuracy degrades monotonically with corruption

As alignment decreased, accuracy declined in a clear monotonic pattern (Fig. 2). The correlation between corruption rate (λ = 1 − A) and accuracy was strongly negative (r = −0.44, p < 10⁻¹⁰⁵). At moderate corruption (λ = 0.4, A = 0.6), accuracy remained above the Direct baseline at 83.9%. However, at high corruption (λ = 0.8, A = 0.2), accuracy fell to 56.3%—substantially below what the model achieved with no reasoning trace at all.

The degradation was not linear. Performance remained relatively stable at low corruption levels (A ≥ 0.8), then declined more steeply as corruption increased. At complete corruption (λ = 1.0, A = 0.0), accuracy reached 34.7%—a 62-percentage-point drop from clean traces and 41 percentage points below the Direct baseline (Extended Data Table 3).

### The backfire boundary depends on baseline capability

The backfire boundary is not a universal alignment threshold; it is the crossing point between provided-trace performance and the model's direct baseline, and therefore shifts with baseline capability.

For Claude Sonnet, using bootstrap resampling [14] (N = 1,000 iterations) with linear interpolation, we estimated:

- **λ* = 0.449** [95% CI: 0.429–0.472]
- **A* = 0.551** [95% CI: 0.528–0.571]

This boundary represents a point where, for this model, provided CoT shifts from beneficial to harmful. Analysis of problem-level collapse points corroborated this estimate: among the 193 problems correctly solved with clean traces, the median collapse point was λ = 0.5, with 37% of problems collapsing within the narrow band λ ∈ (0.4, 0.6].

We further examined whether this boundary generalizes across models. Extended Data Fig. 2 shows results from GPT-4o tested under identical conditions. Despite substantially lower direct accuracy (56.3% vs. 75.9%), GPT-4o achieved comparable clean-trace accuracy (96.5%). The boundary position differed markedly (λ* ≈ 0.87), consistent with the principle that models with weaker baseline performance remain above their baseline longer under corrupted traces (Extended Data Table 4).

### Models exhibit high compliance with provided traces

A critical question is whether models blindly follow provided reasoning or independently verify it against the problem. Our data strongly support the compliance hypothesis.

We identified 151 problems that Claude Sonnet solved correctly in the Direct condition (no CoT). At λ = 0.8, 61 of these problems (40.4%) were answered incorrectly when given corrupted traces. These are problems the model demonstrably *could* solve—yet corrupted reasoning caused failure.

McNemar's test [15] confirmed this asymmetry: 82 problems flipped from correct (at λ = 0) to incorrect (at λ = 0.8), while only 1 flipped in the reverse direction (χ² = 77.1, p < 10⁻¹⁸; Extended Data Table 2). The net loss of 81 problems represents compliance-induced failure: the model followed corrupted reasoning rather than falling back on its own capabilities.

This compliance pattern was consistent across models. Extended Data Table 4 shows that GPT-4o exhibited similar flip rates (36.5% vs. 42.7% for Claude), with comparably significant McNemar statistics (χ² = 65.1, p < 10⁻¹⁵). The consistency of compliance strength across models, despite different baseline capabilities, suggests that high compliance is a general property of how these models process provided reasoning traces.

### Trace depth provides redundancy, not deeper integration

Contrary to an intuition that longer reasoning enables deeper cognitive integration, our data suggest that trace depth primarily provides redundancy (Fig. 3). A logistic regression with an I × λ interaction term confirmed a significant negative interaction (β < 0, p < 0.05), indicating that the advantage of longer traces increases at higher corruption levels.

At high corruption (λ = 0.8), longer traces showed significantly better accuracy: I = 15 achieved 69.3% compared to 56.3% for I = 10 and 59.3% for I = 5 (χ² = 7.88, p = 0.019). However, this advantage has a simple explanation: at fixed corruption rate λ, longer traces retain more absolute correct steps.

| λ | I = 5 clean steps | I = 10 clean steps | I = 15 clean steps |
|---|-------------------|--------------------|--------------------|
| 0.8 | 1 | 2 | 3 |
| 0.6 | 2 | 4 | 6 |
| 0.4 | 3 | 6 | 9 |

When we reanalyzed accuracy as a function of residual clean steps (regardless of I or λ), a clear relationship emerged: more correct steps predicted higher accuracy (r = 0.36, p < 10⁻¹¹²). This relationship held across all trace depths (Fig. 3B), suggesting that models selectively utilize available correct information rather than requiring an unbroken chain of reasoning.

This redundancy interpretation has practical implications. Longer traces are not inherently better; they are better only when they contain more correct information. A short, fully-correct trace will outperform a long, partially-corrupted one.

### A simple curvature probe does not distinguish reasoning conditions

We tested whether hidden-state trajectory curvature (κ) might provide a geometric signature distinguishing successful from failing reasoning. This probe was motivated by the hypothesis that corrupted reasoning would produce more erratic internal trajectories, manifesting as higher curvature.

Under our measurement protocol—mean curvature across layers L/2, 3L/4, and L, computed over tokens between reasoning markers—we found no meaningful differentiation (Extended Data Fig. 1). Mean κ values were 1.942 (Direct), 1.975 (Clean), and 1.968 (Corrupted). While Clean and Corrupted differed from Direct (confirming that CoT presence affects trajectories), the difference between Clean and Corrupted was minimal (Δκ = 0.007) despite their large behavioral gap (97% vs. 57% accuracy).

This negative result is informative: under this measurement approach, simple trajectory curvature does not capture CoT failure. The collapse from correct to incorrect reasoning may involve mechanisms not reflected in average path curvature—perhaps selective attention patterns [16], specific layer computations [17], or token-level rather than trajectory-level features. This constrains the space of candidate geometric signatures and motivates future work on alternative internal probes.

---

## Discussion

### Summary of findings

This study introduces the provided-CoT paradigm and uses it to characterize conditions under which chain-of-thought reasoning fails. Our central finding is a backfire boundary: a point where provided CoT becomes counterproductive, performing worse than providing no reasoning at all.

Three key insights emerge. First, the backfire boundary is not universal but depends on baseline capability—the crossing point shifts with how well models perform without CoT. Second, models exhibit high compliance with provided reasoning, following traces rather than independently verifying them; this compliance is consistent across models despite different baseline capabilities. Third, longer traces provide resilience through redundancy (more surviving correct steps) rather than through deeper integrative processing.

### Compliance as a double-edged property

The compliance we observe parallels well-documented phenomena in human cognition, where provided frameworks strongly influence judgment [18, 19]. For language models, compliance has a dual nature. When traces are correct, compliance enables the dramatic performance gains of CoT. When traces are corrupted, the same compliance causes systematic failure. The backfire boundary marks where this tradeoff tips from net benefit to net harm.

The consistency of compliance strength across Claude and GPT-4o (similar flip rates despite different baselines) suggests this is not model-specific but reflects how current language models process sequential reasoning inputs. This has implications for system design: compliance cannot be "tuned down" for one model without likely affecting others similarly.

### The curvature probe as a constraining result

Our trajectory curvature analysis yielded null results: κ did not differentiate clean from corrupted reasoning despite large behavioral differences. This is not a failure but an informative constraint on mechanistic theories.

Specifically, our findings rule out a simple hypothesis: that CoT failure manifests as geometrically erratic internal processing detectable via trajectory curvature under our measurement protocol. The mechanisms underlying compliance-induced failure likely operate through more selective channels. Our null result narrows the search space for future mechanistic investigations.

### Practical implications

Our findings have immediate relevance for systems that use reasoning traces from external sources.

**Retrieval-augmented generation**: When retrieving reasoning traces from databases or prior interactions, our results suggest that quality thresholds must be calibrated to the specific model's baseline capability. A threshold appropriate for a strong-baseline model may be too conservative for a weaker one.

**Multi-agent systems**: In architectures where one agent's reasoning becomes another's input, our compliance findings highlight propagation risks. A single agent producing corrupted reasoning can cause downstream failures across the system, and the consistency of compliance across models means this risk is not mitigated by using diverse model types.

**Quality assessment**: Rather than seeking a universal quality threshold, practitioners should estimate where their specific model's provided-CoT performance crosses its direct baseline. This crossing point defines the model-specific backfire boundary.

### Limitations

Several limitations bound our conclusions. First, we tested two model families (Claude, GPT-4); generalization to other architectures requires further validation. Second, GSM8K represents one task type (grade-school mathematics); other reasoning domains may show different boundary dynamics. Third, our corruption types (irrelevant, local error, wrong constraint) do not exhaust possible failure modes. Fourth, our curvature probe represents one geometric approach; null results do not preclude other internal signatures.

### Future directions

This work opens several research directions. Extension to diverse reasoning tasks would map how boundaries vary across domains. Alternative internal probes—attention patterns, activation clustering, layer-wise representations—might succeed where curvature failed. Theoretical work could formalize the relationship between trace quality, baseline capability, and compliance, potentially enabling predictive rather than empirical boundary estimation.

---

## Methods

### Dataset and problem selection

We used the GSM8K dataset [20], a collection of grade-school mathematics word problems requiring multi-step reasoning. From the test set (1,319 problems), we selected 200 problems using deterministic random sampling (seed = 20251224). Final answers were extracted from the standard "####" delimiter format. After excluding problems with parsing failures, 199 problems entered analysis.

### Trace generation

Clean reasoning traces were generated using Claude Sonnet with explicit formatting instructions. Each trace contained I reasoning steps in the format:

```
[[COT_START]]
Step 1: [reasoning content]
Step 2: [reasoning content]
...
Step I: [reasoning content]
[[COT_END]]
```

We generated traces at I = 10 as the base depth, then created I = 5 (condensed) and I = 15 (expanded) variants through targeted regeneration. The [[COT_START]] and [[COT_END]] markers enabled precise token-range identification for curvature measurement.

### Corruption protocol

Corruption was controlled by the parameter λ ∈ [0, 1], representing the fraction of steps to corrupt. For a trace of depth I, K = round(λ × I) steps were selected for corruption via deterministic hashing of (problem_id, I, λ).

Three corruption types were applied:
- **Irrelevant (IRR, 20%)**: Replace step with task-unrelated computation
- **Local error (LOC, 40%)**: Introduce small numerical errors in existing calculations  
- **Wrong constraint (WRONG, 40%)**: Impose incorrect intermediate values as constraints

Type assignment was deterministic, ensuring reproducibility. Alignment A = 1 − λ indicates the fraction of steps remaining correct.

### Experimental conditions

Four experiments were conducted with Claude Sonnet:

1. **I × λ Grid** (Experiment 1): Full factorial design with I ∈ {5, 10, 15} and λ ∈ {0.0, 0.2, 0.4, 0.6, 0.8, 1.0}. Total: 18 conditions × 199 problems = 3,582 inferences.

2. **Curvature measurement** (Experiment 2): κ computed for Direct, Clean (λ = 0), and Corrupted (λ = 0.8) conditions using Mistral-7B. Total: 3 conditions × 199 problems = 597 measurements.

3. **Direct baseline** (Experiment 3): No CoT provided; model answers directly. Total: 199 inferences.

4. **Fine-grained λ** (Experiment 4): λ ∈ {0.1, 0.3, 0.5, 0.7, 0.9} at I = 10 to refine boundary estimation. Total: 5 conditions × 199 problems = 995 inferences.

For cross-model validation, GPT-4o was tested under identical conditions: Direct baseline plus I = 10 with λ ∈ {0.0, 0.2, 0.4, 0.6, 0.8, 1.0}. Total: 1,393 additional inferences.

All experiments used temperature = 0 for deterministic outputs.

### Statistical analysis

**Boundary estimation**: The backfire boundary (λ*) was estimated by linear interpolation between observed λ values, finding where accuracy crossed the Direct baseline. Bootstrap confidence intervals [14] (N = 1,000, cluster resampling by problem) provided uncertainty quantification.

**Effect sizes**: Cohen's h [13] was computed for accuracy comparisons, appropriate for binary outcomes.

**Interaction testing**: χ² tests assessed whether trace depth effects varied across λ levels. A logistic regression with I × λ interaction term confirmed significant negative interaction (p < 0.05).

**Paired comparisons**: McNemar's test [15] compared same-problem outcomes across conditions, accounting for within-problem correlation.

**Correlation**: Pearson correlations assessed relationships between continuous predictors (λ, residual clean steps) and accuracy.

### Curvature measurement

Trajectory curvature κ was computed using Mistral-7B (32 layers). For each problem-condition pair:

1. Extract hidden states at layers 16 (L/2), 24 (3L/4), and 32 (L)
2. Identify token range between [[COT_START]] and [[COT_END]] markers
3. Compute local curvature at each token position using the discrete curvature formula with numerical stability (ε = 10⁻⁸)
4. Average across tokens and layers to obtain κ_mean

Direct condition (no markers) used the full response token range.

### Reproducibility

All analyses used fixed random seeds. Complete code, data, and computational environments are available at [repository]. The global seed 20251224 ensures identical problem selection and corruption patterns across replications.

---

## Data Availability

All experimental results are available at Zenodo [DOI]:
- results_full_v3.json (Experiment 1: I × λ grid)
- direct_results_v3.json (Experiment 3: Direct baseline, Claude)
- acrit_results_v3.json (Experiment 4: Fine-grained λ)
- kappa_results_v3_1.json (Experiment 2: Curvature)
- direct_results_gpt.json (Direct baseline, GPT-4o)
- cot_results_gpt.json (CoT conditions, GPT-4o)

## Code Availability

Complete reproduction code including experimental notebooks, analysis scripts, and figure generation is available at GitHub [repository].

---

## Acknowledgements

AI writing assistants were used for manuscript preparation.

---

## Author Contributions

HIDEKI conceived the study, designed experiments, conducted analyses, and wrote the manuscript.

---

## Competing Interests

The author declares no competing interests.

---

## References

[1] Wei, J. et al. Chain-of-thought prompting elicits reasoning in large language models. *NeurIPS* (2022).

[2] Kojima, T. et al. Large language models are zero-shot reasoners. *NeurIPS* (2022).

[3] Wang, X. et al. Self-consistency improves chain of thought reasoning in language models. *ICLR* (2023).

[4] Yao, S. et al. Tree of thoughts: Deliberate problem solving with large language models. *NeurIPS* (2023).

[5] Zhou, D. et al. Least-to-most prompting enables complex reasoning in large language models. *ICLR* (2023).

[6] Turpin, M. et al. Language models don't always say what they think: Unfaithful explanations in chain-of-thought prompting. *NeurIPS* (2023).

[7] Lanham, T. et al. Measuring faithfulness in chain-of-thought reasoning. *arXiv:2307.13702* (2023).

[8] Ye, X. & Durrett, G. The unreliability of explanations in few-shot prompting for textual reasoning. *NeurIPS* (2022).

[9] Lewis, P. et al. Retrieval-augmented generation for knowledge-intensive NLP tasks. *NeurIPS* (2020).

[10] Gao, L. et al. Retrieval-augmented generation for large language models: A survey. *arXiv:2312.10997* (2023).

[11] Xi, Z. et al. The rise and potential of large language model based agents: A survey. *arXiv:2309.07864* (2023).

[12] Park, J.S. et al. Generative agents: Interactive simulacra of human behavior. *UIST* (2023).

[13] Cohen, J. *Statistical Power Analysis for the Behavioral Sciences*. 2nd ed. Lawrence Erlbaum (1988).

[14] Efron, B. & Tibshirani, R.J. *An Introduction to the Bootstrap*. Chapman & Hall/CRC (1993).

[15] McNemar, Q. Note on the sampling error of the difference between correlated proportions or percentages. *Psychometrika* 12, 153–157 (1947).

[16] Elhage, N. et al. A mathematical framework for transformer circuits. *Transformer Circuits Thread* (2021).

[17] Nanda, N. et al. Progress measures for grokking via mechanistic interpretability. *ICLR* (2023).

[18] Tversky, A. & Kahneman, D. Judgment under uncertainty: Heuristics and biases. *Science* 185, 1124–1131 (1974).

[19] Cialdini, R.B. & Goldstein, N.J. Social influence: Compliance and conformity. *Annu. Rev. Psychol.* 55, 591–621 (2004).

[20] Cobbe, K. et al. Training verifiers to solve math word problems. *arXiv:2110.14168* (2021).

[21] Lightman, H. et al. Let's verify step by step. *ICLR* (2024).

[22] Liu, P. et al. Pre-train, prompt, and predict: A systematic survey of prompting methods in natural language processing. *ACM Computing Surveys* 55, 1–35 (2023).

[23] Bubeck, S. et al. Sparks of artificial general intelligence: Early experiments with GPT-4. *arXiv:2303.12712* (2023).

[24] Liang, P. et al. Holistic evaluation of language models. *arXiv:2211.09110* (2022).

[25] Srivastava, A. et al. Beyond the imitation game: Quantifying and extrapolating the capabilities of language models. *arXiv:2206.04615* (2022).

[26] Huang, J. et al. Large language models cannot self-correct reasoning yet. *ICLR* (2024).

[27] Uesato, J. et al. Solving math word problems with process- and outcome-based feedback. *arXiv:2211.14275* (2022).

[28] Bills, S. et al. Language models can explain neurons in language models. *OpenAI Technical Report* (2023).

[29] Asch, S.E. Effects of group pressure upon the modification and distortion of judgments. In *Groups, Leadership and Men* (1951).

---

## Figure Captions

**Fig. 1 | Accuracy across trace depth and corruption rate.** Heatmap showing accuracy (%) for each combination of trace depth (I = 5, 10, 15) and corruption rate (λ = 0.0 to 1.0) in Claude Sonnet. Clean traces (λ = 0) achieve 97% accuracy regardless of depth. Performance degrades monotonically with corruption, reaching 35–45% at full corruption. Direct baseline (no CoT) achieves 75.9%.

**Fig. 2 | Backfire boundary for provided CoT.** Accuracy versus corruption rate (λ) for I = 10 traces across 11 data points (Claude Sonnet). Orange marker indicates estimated boundary λ* = 0.449 [95% CI: 0.429–0.472]. Below this point (above this λ), provided CoT performs worse than the Direct baseline (red dashed line). Green and red shading indicate CoT-beneficial and CoT-harmful regimes, respectively. Error bars show 95% confidence intervals.

**Fig. 3 | Trace depth effects reflect redundancy.** (A) Accuracy by corruption rate for each trace depth in Claude Sonnet. I = 15 shows greatest resilience at high corruption (λ = 0.8: 69.3% vs. 56.3% for I = 10). (B) Accuracy as a function of residual correct steps. The relationship holds across all I values (pooled r = 0.36), indicating that models utilize available correct information regardless of total trace length.

---

## Extended Data

### Extended Data Fig. 1 | Trajectory curvature does not distinguish reasoning conditions.

(A) Boxplot of mean curvature (κ) by condition. Despite large behavioral differences (97% vs. 57% accuracy), Clean and Corrupted traces show similar curvature (Δκ = 0.007). The small numerical difference, while detectable given sample size, is not practically meaningful for distinguishing reasoning quality. (B) Layer-wise analysis confirms this pattern across model depth. This null result under our measurement protocol constrains geometric hypotheses for CoT failure mechanisms.

### Extended Data Fig. 2 | Cross-model comparison of backfire boundaries.

Accuracy versus corruption rate for Claude Sonnet (blue) and GPT-4o (red). Dashed horizontal lines indicate Direct baselines (Claude: 75.9%, GPT-4o: 56.3%). Dotted vertical lines mark estimated boundaries (Claude: λ* ≈ 0.45, GPT-4o: λ* ≈ 0.87). Despite different boundary positions, both models achieve similar clean-trace accuracy (~97%) and exhibit comparable compliance to provided traces. The boundary difference is attributable to baseline capability: GPT-4o's lower Direct performance means corrupted CoT remains above baseline longer.

### Extended Data Table 1 | Full accuracy results by condition (Claude Sonnet)

| I | λ = 0.0 | λ = 0.2 | λ = 0.4 | λ = 0.6 | λ = 0.8 | λ = 1.0 |
|---|---------|---------|---------|---------|---------|---------|
| 5 | 97.0% | 91.0% | 86.4% | 74.4% | 59.3% | 37.2% |
| 10 | 97.0% | 96.0% | 83.9% | 73.9% | 56.3% | 34.7% |
| 15 | 97.0% | 91.5% | 84.9% | 77.9% | 69.3% | 45.2% |

Direct (no CoT): 75.9%

### Extended Data Table 2 | Statistical test results (Claude Sonnet)

| Test | Statistic | p-value | Interpretation |
|------|-----------|---------|----------------|
| λ–accuracy correlation | r = −0.44 | < 10⁻¹⁰⁵ | Strong negative relationship |
| I effect at λ = 0.8 | χ² = 7.88 | 0.019 | Significant depth effect |
| I × λ interaction (logistic) | β < 0 | < 0.05 | Confirmed interaction |
| McNemar (λ = 0 vs 0.8) | χ² = 77.1 | < 10⁻¹⁸ | Asymmetric change |
| Clean steps correlation | r = 0.36 | < 10⁻¹¹² | Redundancy effect |

### Extended Data Table 3 | Effect sizes (Cohen's h, Claude Sonnet)

| Comparison | Cohen's h | Size |
|------------|-----------|------|
| Clean (λ = 0) vs Direct | +0.68 | Medium-large |
| λ = 0.8 vs Direct | −0.42 | Medium |
| λ = 1.0 vs Direct | −0.86 | Large |

### Extended Data Table 4 | Cross-model compliance comparison

| Metric | Claude Sonnet | GPT-4o |
|--------|---------------|--------|
| Direct accuracy | 75.9% | 56.3% |
| Clean CoT accuracy (λ = 0) | 97.0% | 96.5% |
| λ* (backfire boundary) | 0.45 | 0.87 |
| A* (alignment at boundary) | 0.55 | 0.13 |
| Flip rate (Clean→Wrong, λ = 0.8) | 42.7% | 36.5% |
| McNemar χ² (λ = 0 vs 0.8) | 77.1 | 65.1 |
| McNemar p-value | < 10⁻¹⁸ | < 10⁻¹⁵ |

Cross-model validation with GPT-4o indicates that the backfire boundary is model-dependent. Although GPT-4o showed substantially lower direct accuracy (56.3% vs. 75.9% in Claude), both models exhibited comparably strong compliance to provided traces, evidenced by similar clean→corrupted flip rates at λ = 0.8 (Claude: 42.7%, GPT-4o: 36.5%; McNemar χ² > 65, p < 10⁻¹⁵). Accordingly, the difference in the estimated boundary (Claude: λ* ≈ 0.45, GPT-4o: λ* ≈ 0.87) is attributable primarily to baseline capability: models with weaker direct performance remain above their baseline for longer even under substantially corrupted traces.

---

*Manuscript word count: approximately 4,800 words (excluding methods, references, and extended data)*
