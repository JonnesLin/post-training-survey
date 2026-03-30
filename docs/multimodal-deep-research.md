# Multimodal Post-Training: Comprehensive Research Document

> **Date**: 2025-03-30
> **Purpose**: Reference document for expanding Chapter 5 (Multimodal Post-Training)
> **Scope**: ~95 new papers across 12 research directions, with detailed descriptions, evolution chains, and integration recommendations
> **Note**: All paper descriptions below are based on web search results and should be fact-checked against the original arXiv papers before integration into LaTeX.

---

## Table of Contents

1. [Overview & Statistics](#1-overview--statistics)
2. [Direction 1: Multimodal Reasoning via RL — The Post-GRPO Landscape](#2-direction-1)
3. [Direction 2: Generation Alignment — RLHF/DPO for Diffusion & Video](#3-direction-2)
4. [Direction 3: Multimodal Data Synthesis & Intelligent Curation](#4-direction-3)
5. [Direction 4: VLM Distillation & Efficient Deployment](#5-direction-4)
6. [Direction 5: Long-Context Multimodal Models](#6-direction-5)
7. [Direction 6: Medical & Scientific Domain VLM Post-Training](#7-direction-6)
8. [Direction 7: Multimodal RAG](#8-direction-7)
9. [Direction 8: Multimodal Safety — From Open Problem to Full Narrative](#9-direction-8)
10. [Direction 9: Native Multimodal Pre-Training & Early Fusion](#10-direction-9)
11. [Direction 10: VLM Evaluation & Benchmarks](#11-direction-10)
12. [Direction 11: Emerging — Self-Improvement Without Human Labels](#12-direction-11)
13. [Direction 12: Multimodal Preference Learning & DPO Variants](#13-direction-12)
14. [Cross-Cutting Themes](#14-cross-cutting-themes)
15. [Proposed Chapter Structure](#15-proposed-chapter-structure)

---

## 1. Overview & Statistics

**Research scope**: 5 parallel agents searched 12 directions using web search, returning ~138 candidate papers. After deduplication (cross-agent overlap + existing chapter coverage of ~100 cite keys), **~95 new papers** remain. Of these, **~50 are recommended for integration** based on importance classification.

**Importance distribution of recommended papers**:
- LANDMARK: 2 (DSPO, MedVLM-R1)
- IMPORTANT: 35
- MINOR: 13
- Surveys (for reference): 8

**Chapter impact estimate**: Current chapter is ~1800 lines with ~80 papers. Integration of ~50 new papers would bring it to ~2800–3000 lines (~110–130 papers), making it the most comprehensive thread in the survey.

---

## 2. Direction 1: Multimodal Reasoning via RL — The Post-GRPO Landscape {#2-direction-1}

### Narrative Context

The existing chapter (§5.4.1) covers the first wave of multimodal reasoning via RL: R1-VL introduced GRPO to VLMs, URSA added process-level reward (PS-GRPO), VisualPRM provided an open-source process reward model, and MVoT explored "thinking in images." The section concludes with the observation that 2025 multimodal reasoning pushed along three axes: training signal refinement, test-time scaling, and reasoning modality expansion.

Since that writing, the field has exploded. The key insight from new work is that **the bottleneck is no longer the RL algorithm (GRPO works everywhere) but the reward signal design and training stability**. Three sub-themes have emerged:

1. **Perception-aware rewards**: Standard RLVR improves reasoning but not visual perception. New work explicitly rewards perceptual accuracy.
2. **Training stability innovations**: Naive GRPO application to VLMs suffers from gradient stagnation, vanishing advantages, and overthinking. Multiple papers propose fixes.
3. **Self-rewarding / zero-annotation**: VLMs generate their own training signal, eliminating the need for human labels or external reward models.

### Paper Descriptions

#### Perception-R1: Visual Perception Reward (2025) — IMPORTANT
**arXiv**: 2506.07218 | **Authors**: Tong Xiao, Xin Xu, Zhenya Huang et al.

Perception-R1 addresses a critical finding that standard RLVR (including R1-VL and MM-Eureka) improves multimodal *reasoning* but fails to improve *visual perception*. When a VLM reasons correctly but "sees" incorrectly (e.g., misidentifying the number of objects in an image), standard outcome-based rewards cannot detect this failure — the answer may still be wrong, but the reward signal attributes the failure to reasoning rather than perception.

The core innovation is a **visual perception reward** that explicitly evaluates whether the model accurately perceives visual content during chain-of-thought generation. The method uses CoT trajectory visual annotations as references for reward assignment — essentially checking whether the perceptual claims made in the reasoning chain (e.g., "I see 3 red circles") are actually true in the image.

**Key result**: Achieves SOTA with only **1,442 training samples**, demonstrating that targeted perception rewards are far more data-efficient than broad reasoning rewards. This is because the reward signal directly addresses the root cause of failure (perception) rather than a downstream symptom (wrong answer).

**Position in evolution**: Directly extends URSA's PS-GRPO by adding a perception dimension to process-level supervision. Where PS-GRPO checks "is this reasoning step logically correct?", Perception-R1 checks "is this perceptual claim visually correct?"

#### PEARL: Perceptual-Evidence Anchored Reinforced Learning (2025) — IMPORTANT
**arXiv**: 2511.18437 | **Authors**: Chi Zhang et al.

PEARL takes a complementary approach to Perception-R1. Instead of adding a perception reward, PEARL restructures the reasoning process itself to **anchor every reasoning step to verified visual evidence**. The method introduces a "perception checklist" — a set of verifiable sub-questions about the image (e.g., "How many bars are in the chart?", "What color is the largest segment?") that the model must answer before reasoning.

This checklist serves dual purposes: (1) it forces the model to explicitly ground its reasoning in visual facts rather than hallucinated assumptions, and (2) it provides a natural decomposition for process-level reward — each checklist item can be independently verified.

**Key result**: +9.7% over baseline and +6.6% over standard GRPO on MathVerse. The improvement is especially pronounced on tasks requiring precise numerical reading from charts and diagrams — exactly the tasks where perceptual grounding matters most.

**Position in evolution**: If Perception-R1 adds perception to the reward, PEARL adds perception to the policy. Together they represent the "perception-aware" branch of multimodal RL.

#### VL-Rethinker: Incentivizing Self-Reflection (2025) — IMPORTANT
**arXiv**: 2504.08837 | **Authors**: Haozhe Wang et al. (Wenhu Chen group)

VL-Rethinker identifies a previously unreported failure mode in VLM GRPO training: **vanishing advantages**. In standard GRPO, the advantage signal for each sample is computed relative to the group mean. When training progresses, the model's outputs become increasingly homogeneous within each group, causing advantage signals to collapse toward zero. This destabilizes deliberate reasoning — the model stops exploring diverse reasoning paths because no path has a meaningful advantage over others.

Two mechanisms address this:
1. **Selective Sample Replay**: Reintroduces high-advantage samples from earlier training stages to maintain gradient signal diversity.
2. **Forced Rethinking**: At certain training steps, the model is forced to generate an alternative reasoning chain starting from "Wait, let me reconsider..." This explicitly promotes self-reflection and prevents premature convergence to a single reasoning strategy.

**Key result**: MathVista 80.4%, MathVerse 63.5% — competitive with much larger models.

**Position in evolution**: This is a *training stability* contribution rather than a reward design contribution. It complements Perception-R1/PEARL (which improve *what* is rewarded) by improving *how* the RL training converges.

#### OpenVLThinker: Iterative SFT-RL Cycling (2025) — IMPORTANT
**arXiv**: 2503.17352 | **Authors**: Yihe Deng et al. (Kai-Wei Chang group)

OpenVLThinker demonstrates that alternating between SFT and RL iteratively produces better results than a single round of either. The insight is that SFT and RL play complementary roles: **SFT surfaces latent reasoning capabilities** (by exposing the model to correct reasoning traces), while **RL refines and generalizes** those capabilities (by learning which reasoning strategies actually lead to correct answers across diverse problems).

The method distills reasoning traces from text-based R1 models (which have strong reasoning but no visual understanding) and adapts them to visual domains through iterative cycles. Each cycle: (1) SFT on curated reasoning traces → (2) RL with rule-based rewards → (3) collect model's best reasoning traces → (4) use as new SFT data for next cycle.

**Key result**: +3.8% MathVista, +2.4% EMMA over single-round baselines. The iterative cycling is critical — a single round of SFT+RL achieves only half the gain.

**Position in evolution**: This is the first open-source demonstration that iterative SFT-RL cycling works for VLMs. It connects to the DPE self-evolving paradigm (§5.4.5 in the chapter) but focuses on reasoning quality rather than data generation.

#### Virgo: Text-Only Thought Transfers to VLMs (2025) — IMPORTANT
**arXiv**: 2501.01904 | **Authors**: Yifan Du et al. (Ji-Rong Wen group)

Virgo provides a surprising and important insight: **text-only long-form thought data is more effective than multimodal data for eliciting slow-thinking in VLMs**. The authors fine-tune Qwen2-VL-72B on various combinations of text-only reasoning traces, multimodal reasoning traces, and mixed data. The text-only condition consistently outperforms.

The explanation is that slow-thinking capability (long chains of reasoning, self-correction, backtracking) is **fundamentally a property of the language model component**. The visual encoder provides perceptual input, but the reasoning process happens in language space. Therefore, training the language component's reasoning capability with high-quality text-only data (which is abundant) is more efficient than using scarce multimodal reasoning data.

**Key result**: Fine-tuned Qwen2-VL-72B is competitive with commercial reasoning systems on multimodal benchmarks.

**Position in evolution**: This finding has major practical implications — it suggests that the "cold start" for multimodal reasoning can use text-only data, with visual-specific fine-tuning needed only for perception (not reasoning). This partially explains why the chapter's R1-VL (which starts from a text-reasoning model) works so well.

#### R1-Onevision: Cross-Modal Formalization (2025) — IMPORTANT
**arXiv**: 2503.10615 | **Venue**: ICCV 2025

R1-Onevision takes a different approach entirely: instead of improving the reward or stabilizing RL training, it improves the **input representation**. The method transforms visual content into formal textual representations (e.g., converting a geometric diagram into coordinate specifications, or a chart into a data table) before reasoning. This "cross-modal formalization" allows the model to reason in a purely symbolic domain where its language-based reasoning is strongest.

**Key result**: Surpasses GPT-4o on multiple multimodal reasoning benchmarks. Accepted at ICCV 2025.

**Position in evolution**: Complementary to Virgo's finding — both suggest that the reasoning bottleneck is in the perception-to-language interface, not in the reasoning itself. R1-Onevision solves this by making the interface explicit.

#### MiMo-VL (2025) — IMPORTANT
**arXiv**: 2506.03569 | **Authors**: Xiaomi LLM-Core Team

MiMo-VL is Xiaomi's comprehensive VLM with a four-stage pretraining pipeline on 2.4T tokens, followed by **Mixed On-policy RL**. The key technical contribution is demonstrating that **on-policy RL is critical for stability in large-scale multimodal GRPO training**. Off-policy methods (using cached rollouts) introduce distribution mismatch that causes training instability at scale.

**Key result**: Surpasses Qwen2.5-VL-7B on 35/40 benchmarks. SOTA for GUI grounding (56.1 OSWorld-G).

#### GPRO: Gated Perception-Reasoning Optimization (2026) — IMPORTANT
**arXiv**: 2601.04442 | **Authors**: Xingjian Diao et al.

GPRO introduces a **meta-reasoning controller** that dynamically routes among three decision paths based on the input:
1. **Direct answer** (perception is clear, no reasoning needed)
2. **Short reasoning** (moderate complexity)
3. **Full reasoning chain** (complex multi-step problem)

The key insight is that "always think harder" is wasteful and can hurt performance. On simple perceptual questions (e.g., "What color is the car?"), generating a long reasoning chain introduces unnecessary opportunities for hallucination. GPRO distinguishes **perception failures** from **reasoning errors** and only applies heavy reasoning when it's actually needed.

**Position in evolution**: This is the "efficiency" branch of multimodal RL — optimizing when to reason, not just how to reason.

#### Other Notable Papers (MINOR)

- **ReVisual-R1** (2506.04207): Staged RL (text→multimodal→text) fixes gradient stagnation specific to multimodal settings. The key idea is to "warm up" reasoning in text space before introducing visual complexity.
- **Mirage** (2506.17218): Enables "mental imagery" — VLMs generate latent visual tokens during reasoning without explicit pixel generation. Analogous to how humans visualize spatial transformations.
- **VisionThink** (2507.13348): RL controls the visual input resolution itself — the model learns to request high-resolution crops of specific image regions when needed for reasoning.
- **R1-Reward** (2505.02835): Trains reward models themselves via RL (meta-RL for reward modeling). +8.4% on VL-RewardBench.
- **VRPRM** (2508.03556): Data-efficient PRM — 3.6K CoT + 50K non-CoT surpasses 400K-sample PRMs like VisualPRM.

### Evolution Chain

```
Wave 1: Applying GRPO to VLMs (2025 Q1)
├── R1-VL, MM-Eureka, VLM-R1: rule-based rewards, answer correctness
│
Wave 2: Process-Level Reward (2025 Q1-Q2)
├── URSA PS-GRPO: step-level correctness checking
├── VisualPRM: open-source multimodal PRM
│
Wave 3: Perception-Aware Reward (2025 Q2-Q3)
├── Perception-R1: explicitly reward visual perception accuracy
├── PEARL: perception checklist anchors reasoning to visual evidence
│
Wave 4: Training Stability (2025 Q2-Q3)
├── VL-Rethinker: vanishing advantages → selective replay + forced rethinking
├── OpenVLThinker: iterative SFT-RL cycling
├── MiMo-VL: on-policy RL is critical at scale
├── ReVisual-R1: staged RL (text→multimodal→text)
│
Wave 5: Efficiency & Meta-Reasoning (2025 Q4 — 2026)
├── GPRO: when NOT to reason (gated decision routing)
├── VisionThink: RL controls resolution (adaptive visual input)
├── VRPRM: data-efficient PRM (3.6K beats 400K)
│
Wave 6: Self-Rewarding (2025 Q3+) → see Direction 11
├── Vision-R1, Vision-SR1, Perception-R1 variants
```

### Integration Recommendation

**Expand §5.4.1** with ~7 papers organized as two new wave descriptions after the existing content:
1. "Perception-aware reward" paragraph block: Perception-R1 + PEARL
2. "Training stability" paragraph block: VL-Rethinker + OpenVLThinker + Virgo
3. "Meta-reasoning and efficiency" brief: GPRO + VisionThink
4. Update the "范式转变总结" to reflect the 6-wave evolution

---

## 3. Direction 2: Generation Alignment — RLHF/DPO for Diffusion & Video {#3-direction-2}

### Narrative Context

The existing chapter (§5.4.3) discusses unified multimodal generation *architectures* (Chameleon, Emu3, Janus, Show-o, Transfusion) and notes the open challenge of multi-objective alignment. But it does **not** cover the rapidly growing field of aligning generation *quality* through RLHF/DPO. This is a critical gap — the same preference optimization techniques from Thread 2 are being adapted to diffusion and video generation models, but with fundamentally different pathologies and solutions.

This direction splits into two tracks:
1. **DPO-fixing track**: Diagnosing and fixing fundamental pathologies when applying DPO to diffusion/video generation (likelihood displacement, winner degradation, timestep imbalance)
2. **RL-scaling track**: Scaling RL-based alignment to video generation and unified multimodal models

### Paper Descriptions

#### DSPO: Direct Score Preference Optimization (ICLR 2025 Oral) — LANDMARK
**Authors**: Huaisheng Zhu, Teng Xiao, Vasant Honavar

DSPO is the first theoretically rigorous framework for aligning diffusion models with human preferences. The key insight is an **objective mismatch** in prior approaches: diffusion models are pre-trained using score matching (learning the gradient of the log-probability), but existing DPO adaptations operate in the sample space (comparing generated images). This mismatch introduces optimization inefficiency and instability.

DSPO resolves this by directly optimizing in the **score function space** — aligning the score function (gradient field) that guides the denoising process, rather than comparing final samples. The authors prove that under certain conditions, this is theoretically equivalent to RL-based alignment algorithms, providing a unified theoretical foundation for diffusion alignment.

**Key result**: ICLR 2025 Oral acceptance. Significant improvements in generation quality metrics while maintaining training stability.

**Why LANDMARK**: This paper establishes the theoretical foundation for diffusion alignment, analogous to how DPO established the theoretical foundation for LLM alignment. All subsequent diffusion DPO work either builds on or is compared against this framework.

#### Diffusion-SDPO: Safeguarded DPO (2025) — IMPORTANT
**arXiv**: 2511.03317 | **Authors**: Minghao Fu et al.

Diffusion-SDPO identifies a counterintuitive pathology in standard Diffusion-DPO: **enlarging the preference margin does not improve generation quality**. In LLM DPO, making the preferred response more preferred (wider margin) is generally beneficial. In diffusion models, this causes both the preferred and dispreferred outputs to degrade — a phenomenon the authors call "winner degradation."

The root cause is that in the diffusion denoising process, pushing the model away from the dispreferred denoising trajectory also corrupts nearby trajectories that lead to high-quality outputs. SDPO proposes **safeguarded updates** that adaptively scale the gradient contribution of the dispreferred (loser) sample to protect the preferred (winner) output.

**Position in evolution**: This is a "DPO-fixing" paper that diagnoses diffusion-specific pathologies absent from text DPO.

#### VideoReward / Flow-DPO (2025) — IMPORTANT
**arXiv**: 2501.13918 | **Authors**: Jie Liu et al. (Wanli Ouyang group)

The first systematic RLHF framework for **flow-based video generation** (a popular architecture choice for video diffusion). The contribution is twofold:

1. **VideoReward**: A multi-dimensional reward model that separately evaluates video quality across dimensions (motion coherence, temporal consistency, visual fidelity, text-alignment). This is critical because scalar reward for video is far more lossy than for text — a video can have perfect visual quality but terrible motion.

2. **Three alignment algorithms** covering different deployment scenarios:
   - **Flow-DPO**: Training-time preference optimization adapted for flow matching
   - **Flow-RWR** (Reward-Weighted Regression): Training-time reward maximization
   - **Flow-NRG** (Noise-Refined Guidance): Inference-time reward guidance (no fine-tuning needed)

**Position in evolution**: Extends the DPO/RLHF paradigm from images to video, where temporal coherence adds a qualitatively new dimension to alignment.

#### DDRL: Data-Regularized RL at Scale (2025) — IMPORTANT
**arXiv**: 2512.04332 | **Authors**: Haotian Ye et al. (Stefano Ermon group)

DDRL addresses the critical problem of **reward hacking** in diffusion model RL. When RL-based alignment (e.g., AlignProp, DRaFT) is applied at scale, models learn to exploit reward model weaknesses — generating images that score high on the reward model but look unnatural or distorted to humans. This is the diffusion analog of the "reward hacking" problem in LLM RLHF.

The solution is to **anchor RL policies to off-policy data distributions** via forward KL divergence. This prevents the model from drifting too far from the distribution of real, high-quality data. The forward KL (as opposed to reverse KL used in standard RLHF) is specifically chosen because it is mode-covering — it prevents the model from collapsing onto narrow reward-hacking modes.

**Key result**: Validated on video generation at scale, maintaining quality without reward hacking even with aggressive reward optimization.

#### RecA: Reconstruction Alignment for Unified Models (2025) — IMPORTANT
**arXiv**: 2509.07295

RecA directly tackles the gap identified in the chapter's Open Problem 5 (Post-Training for Unified Generation). It proposes a **reconstruction-based alignment** method that works across all three major unified model architectures (autoregressive, masked-AR, and diffusion-based).

The core mechanism uses visual encoder embeddings as dense prompts with a self-supervised reconstruction loss. Instead of needing preference pairs (expensive for generation), RecA trains the model to reconstruct visual features from its own generation, forcing it to maintain visual fidelity.

**Key result**: GenEval score from 0.73 to 0.90 in only **27 GPU-hours**. Architecture-agnostic (works with Janus, Show-o, and similar models).

**Position in evolution**: This is the first practical solution to Open Problem 5 — alignment for unified understanding+generation models.

#### Unified Interleaved Generation via GRPO (2026) — IMPORTANT
**arXiv**: 2603.09538 | **Authors**: Ming Nie et al.

This is the first application of GRPO to **multimodal generation** (not just understanding). The method extends GRPO to interleaved text+image generation with **hybrid rewards** covering:
- Textual relevance (does the text make sense?)
- Visual-text alignment (does the generated image match the text?)
- Structural fidelity (does the interleaved output follow the correct format?)

**Key result**: Achieves strong interleaved generation quality without large-scale interleaved training data.

**Position in evolution**: Bridges the gap between Direction 1 (GRPO for understanding) and Direction 2 (alignment for generation). This is the convergence point.

#### Focus-N-Fix: Region-Aware Fine-Tuning (CVPR 2025) — IMPORTANT
**arXiv**: 2501.06481 | **Authors**: Google

Focus-N-Fix solves the "global degradation" problem in diffusion RLHF: when training the model to fix problems in one image region (e.g., distorted hands), the quality of other regions degrades. The method trains models to correct **only problematic regions** using localized reward signals, leaving unaffected regions untouched.

**Position in evolution**: This is the "surgical alignment" approach — targeted fixes rather than global optimization.

#### Other Notable Papers (MINOR)

- **SUDO** (2504.14534): Self-supervised DPO for diffusion — generates preference pairs on-the-fly via image transformations. No human ranking needed.
- **Curriculum-DPO++** (2602.13055): Joint model-capacity and data-difficulty scheduling for diffusion DPO.
- **PG-DPO** (2511.19049): Diagnoses "likelihood displacement" in video diffusion DPO — chosen sample probabilities paradoxically decrease during training.
- **PRFL** (2511.21541): Video generators serve as their own latent-space reward models — process reward feedback in latent space.
- **Diffusion-DRF** (2601.04153): VLMs as critics for diffusion — decomposes prompts into multi-dimensional VQA queries for dense feedback without trained reward models.
- **VisionReward** (2412.21059): Unified image+video reward model with interpretable hierarchical dimensions.
- **SeGroS** (2603.19807): Semantically-grounded supervision for unified model alignment — improves upon RecA by focusing reconstruction on text-aligned regions.

### Evolution Chain

```
Text-only RLHF/DPO (Thread 2)
│
├── Diffusion-DPO (adapt DPO loss to denoising score matching)
│   ├── DSPO (ICLR 2025 Oral): theoretically grounded score-space optimization
│   ├── Pathology fixes:
│   │   ├── Diffusion-SDPO: winner degradation → safeguarded updates
│   │   ├── PG-DPO: likelihood displacement → adaptive rejection scaling
│   │   └── Curriculum-DPO++: joint model/data curriculum
│   └── Self-supervised: SUDO (no human preferences needed)
│
├── Video generation alignment:
│   ├── VideoReward / Flow-DPO: first systematic video RLHF
│   ├── PRFL: process reward in latent space
│   ├── DDRL: reward hacking prevention via data regularization
│   └── Diffusion-DRF: VLM-as-critic (no trained reward model)
│
└── Unified model alignment:
    ├── RecA: reconstruction-based, architecture-agnostic (27 GPU-hours)
    ├── SeGroS: semantically-grounded reconstruction
    └── Interleaved GRPO: GRPO for multimodal generation
```

### Integration Recommendation

**Create new §5.4.X "2025: Generation Alignment"** (~150-200 lines):
- Opening: bridge from §5.4.3 (unified generation architectures) to alignment methods
- DSPO as the theoretical foundation (LANDMARK treatment)
- DPO pathology fixes as a group paragraph (Diffusion-SDPO, PG-DPO)
- Video alignment paragraph (Flow-DPO, DDRL)
- Unified model alignment paragraph (RecA, Interleaved GRPO)
- Connection to Open Problem 5 (now partially addressed)

---

## 4. Direction 3: Multimodal Data Synthesis & Intelligent Curation {#4-direction-3}

### Narrative Context

The existing chapter mentions data construction only in the context of specific models: LLaVA's GPT-4-generated data, ShareGPT4V, and Molmo's human-only PixMo dataset. There is no systematic treatment of how multimodal training data is synthesized, selected, mixed, or curated at scale. This is a critical gap because **data quality is consistently identified as having first-order impact on post-training outcomes** (InternVL2.5 achieves better results with 1/10 the training tokens; Molmo's human-only data beats synthetic data at scale).

The new papers reveal a paradigm shift: from "generate as much data as possible" to "select, mix, and schedule data intelligently." Three sub-themes:

1. **Synthesis pipelines**: New methods for generating multimodal instruction data
2. **Intelligent selection**: Choosing which data to generate or train on
3. **Mixing & scaling laws**: Optimizing the composition of multi-source data

### Paper Descriptions

#### SAIL-VL: Scalable Vision Language Model Training (ACL 2025) — IMPORTANT
**arXiv**: 2501.05952 | **Authors**: ByteDance

SAIL-VL makes two contributions that establish it as a reference for VLM data curation:

1. **SAIL-Caption**: A data construction pipeline enabling **hundred-million-scale** high-quality recaption annotation. The pipeline involves multi-stage quality filtering, where each caption is scored for accuracy, detail, and relevance. The resulting SAIL-Caption dataset is validated as the highest-quality open-source caption dataset available.

2. **Logarithmic scaling laws for VLM data**: The authors demonstrate that VLM performance scales logarithmically with training data size in 2B models — meaning that going from 1M to 10M samples gives roughly the same improvement as going from 10M to 100M. This has major practical implications: it suggests diminishing returns for massive data collection and underscores the importance of data quality over quantity.

**Key result**: SOTA on 18 VLM benchmarks. First to quantify scaling laws for multimodal SFT data.

#### DataProphet: Training-Free Data Selection (ICLR 2026) — IMPORTANT
**arXiv**: 2603.19688

DataProphet addresses a fundamental question: **before training a VLM, can we predict which supervision data will generalize best?** The answer is yes — the method combines three signals into a training-free metric:
- **Multimodal perplexity**: How surprised is the model by this data?
- **Similarity**: How related is this data to the target task distribution?
- **Diversity**: How much does this data cover the space of possible inputs?

A critical finding: **intuitive task similarity is an unreliable predictor** of data generalization. Data that "looks" relevant to a target task may not actually improve performance on that task, while seemingly unrelated data sometimes helps significantly.

**Key result**: 86% Kendall's tau correlation with actual post-training rankings. Yields up to 6.9% improvement over uniform data selection and 1.4% over SOTA training-based selection methods — all without any training.

**Position in evolution**: This is the "meta-learning for data" direction — learning which data to use before any model training.

#### MoDoMoDo: Multi-Domain Data Mixtures for RLVR (2025) — IMPORTANT
**arXiv**: 2505.24871 | **Authors**: Liang, Qiu, Ding et al.

MoDoMoDo is the first systematic study of **data mixing for multimodal RL post-training**. When training a VLM with GRPO across multiple domains (math, science, OCR, charts, etc.), the relative proportion of each domain in the training data significantly affects both in-domain and out-of-domain performance.

The method learns to predict RL fine-tuning outcomes from the mixture distribution, then optimizes the mixture using this predictive model. This is far more efficient than grid search over mixture ratios.

**Key result**: Best mixture improves OOD benchmark accuracy by 5.24% over uniform mixing and 20.74% over pre-finetuning baseline.

**Position in evolution**: This fills a gap between DPE (which diagnoses capability weaknesses) and standard RLVR training (which uses fixed data mixtures). MoDoMoDo provides the "what proportion" answer to DPE's "what to train on" question.

#### Oasis: Image-Only Instruction Synthesis (ICCV 2025) — IMPORTANT
**arXiv**: 2503.08741 | **Authors**: Zhang, Cui, Zhao, Yang

Oasis pushes the data synthesis paradigm further than LLaVA or ShareGPT4V: it generates high-quality instruction-following data from **images alone**, without any paired text metadata (captions, bounding boxes, etc.). The method prompts a multimodal LLM with only images and generates diverse instruction-response pairs, with a multi-stage quality control pipeline.

**Key result**: 500K+ high-quality data points. Models trained on Oasis data match or exceed those trained on metadata-dependent pipelines.

**Position in evolution**: Extends LLaVA's "GPT-4 + image metadata" → ShareGPT4V's "GPT-4V + images" → Oasis's "any MLLM + images only". Each step removes a dependency.

#### PreSel: Select Images First, Generate Instructions Later (CVPR 2025 Highlight) — IMPORTANT
**arXiv**: 2503.07591 | **Authors**: Safaei et al. (JHU, Honda Research)

PreSel inverts the standard data construction pipeline. Instead of:
1. Collect images → 2. Generate instructions for all images → 3. Filter low-quality pairs

PreSel does:
1. Collect images → 2. **Select beneficial images first** → 3. Generate instructions only for selected images

This dramatically reduces the cost of instruction generation (which requires expensive MLLM API calls) while maintaining or improving downstream quality. The selection criterion is based on the image's information content and diversity relative to the existing dataset.

**Key result**: CVPR 2025 Highlight. Cuts instruction generation costs while maintaining quality.

#### Model Collapse in Multimodal (ICMI 2025) — MINOR
**arXiv**: 2505.08803

First systematic study of **model collapse** in multimodal VLM/diffusion settings — when models are recursively trained on their own synthetic outputs. Discovers that collapse exhibits distinct multimodal characteristics: VL alignment may paradoxically improve even as individual modality quality degrades. Frozen-model relabeling (using a fixed model to generate labels rather than the evolving model) significantly slows collapse.

**Practical implication**: As more VLM training data is synthetically generated (Oasis, ShareGPT4V, etc.), this work provides guidelines for avoiding recursive quality degradation.

#### Instructify (2025) — MINOR
**arXiv**: 2505.18115

Open, unified framework for converting image metadata to visual instruction tuning (VisIT) data using **open LLMs** (not GPT-4). Reproduces or enhances existing VisIT datasets using open models, removing the closed-source API dependency that LLaVA's original pipeline required.

#### L2T: Learning to Instruct (2025) — MINOR
**arXiv**: 2503.22215

Incorporates loss on both instruction and response sequences (standard SFT only computes loss on responses). This expanded training signal regularizes MLLMs from over-reliance on language priors. +9% relative improvement with zero additional data.

### Evolution Chain

```
Manual data curation (early VQA datasets)
→ GPT-4 text-only generation (LLaVA: text metadata → instructions)
→ GPT-4V multimodal generation (ShareGPT4V: images → captions)
→ Human-only annotation (Molmo PixMo: speech-based descriptions)
→ Image-only synthesis (Oasis: just images, no metadata)
→ Intelligent selection before generation (PreSel: select-then-generate)
→ Scaling laws & mixture optimization (SAIL-VL: log scaling, MoDoMoDo: optimal mixing)
→ Training-free data selection (DataProphet: predict generalization without training)
→ Collapse awareness (Model Collapse: guidelines for recursive synthetic data)
```

### Integration Recommendation

**Create new §5.4.X "2025: Multimodal Data Synthesis & Curation"** (~100-130 lines):
- Opening: data quality as the underappreciated first-order factor (connect to Molmo, InternVL2.5 findings)
- Synthesis evolution: LLaVA → ShareGPT4V → Oasis (image-only)
- Intelligent selection: PreSel (select-then-generate), DataProphet (training-free)
- Mixing & scaling: SAIL-VL (log scaling laws), MoDoMoDo (RLVR mixing)
- Warning: Model Collapse study

---

## 5. Direction 4: VLM Distillation & Efficient Deployment {#5-direction-4}

### Narrative Context

The existing chapter mentions parameter-efficient methods only in passing (Phi-4's Mixture-of-LoRAs, cross-reference to Thread 7). There is **no treatment of VLM distillation** — transferring capabilities from large VLMs to small ones. This is a critical practical direction: edge deployment (mobile, robotics, embedded) requires models in the 256M–2B range, far smaller than the 7B–72B models discussed throughout the chapter.

### Paper Descriptions

#### SmolVLM: Redefining Small VLMs (2025) — IMPORTANT
**arXiv**: 2504.05299 | **Authors**: HuggingFace team

SmolVLM systematically explores the design space of **ultra-small VLMs** (256M to 2.2B parameters). The key findings challenge assumptions about minimum viable model size:

- **256M model outperforms Idefics-80B** (300x larger) on multiple benchmarks
- Uses less than 1GB VRAM, enabling true edge deployment (mobile phones, embedded devices)
- Architectural choices matter enormously at small scale: tokenization strategy, vision encoder selection, and data curation are all more impactful than at 7B+ scale

The paper provides a systematic ablation across architectural configurations, representing the first comprehensive "recipe" for building competitive sub-1B VLMs.

#### FastVLM: Efficient Vision Encoding (CVPR 2025) — IMPORTANT
**arXiv**: 2412.13303 | **Authors**: Apple

FastVLM introduces **FastViTHD**, a hybrid convolution-Transformer vision encoder specifically designed for VLM deployment. The key innovation is eliminating the need for additional token pruning/compression — the encoder itself produces a compact representation.

**Key result**: **85x faster time-to-first-token** and **3.4x smaller encoder** compared to LLaVA-OneVision at comparable performance. The optimal balance between token count and resolution is achieved solely by scaling input image resolution — no complex multi-stage compression needed.

**Position in evolution**: While the chapter's discussion of vision encoders focuses on scaling up (InternViT 6B), FastVLM shows that scaling down the encoder is equally important for deployment.

#### MoVE-KD: Multi-Encoder Distillation via MoE (CVPR 2025) — IMPORTANT
**arXiv**: 2501.01709 | **Authors**: Cao et al.

The chapter's Cambrian-1 section reveals that no single visual encoder is optimal across all tasks (CLIP for semantics, DINOv2 for spatial, SigLIP for others). MoVE-KD addresses this by **distilling multiple encoder capabilities into a single encoder** using LoRA + MoE routing + attention-based adaptive weighting.

During training, the student encoder receives knowledge from CLIP, EVA-02, and ConvNeXt simultaneously, with MoE routing dynamically selecting which teacher's knowledge to prioritize for each input. At inference, only the single student encoder is used — eliminating the multi-encoder computational overhead.

**Position in evolution**: Directly solves the problem identified by Cambrian-1: multi-encoder VLMs are impractical for deployment, but single encoders sacrifice capability. MoVE-KD achieves multi-encoder quality with single-encoder cost.

#### CASP: Compression via Attention Sparsity (CVPR 2025 Highlight) — IMPORTANT
**arXiv**: 2503.05936 | **Authors**: Gholami et al.

CASP discovers that **multimodal models have distinct attention sparsity patterns** compared to text-only models. Visual tokens create structured sparsity in the attention matrices that can be exploited for compression. The method performs data-aware low-rank decomposition on Q/K weight matrices followed by optimal bit-allocation quantization.

**Key result**: Enhances SOTA 2-bit quantization methods (AQLM, QuIP#) by an **average of 21%** on image/video-language benchmarks. CVPR 2025 Highlight.

#### ACED: Active Data Curation + Distillation (CVPR 2025) — IMPORTANT
**arXiv**: 2411.18674

ACED demonstrates that **data curation and knowledge distillation are complementary, not substitutes**. The method combines online batch selection (ACID) with explicit feature-level distillation. The ACID component actively selects the most informative training batches for the student, while the KD component transfers dark knowledge from the teacher.

**Key result**: SOTA across 27 zero-shot tasks with up to 11% fewer inference FLOPs.

#### Other Notable Papers (MINOR)

- **LAid** (2512.21576, AAAI 2026): Distills long-range attention mechanisms. Distilled models achieve 3.2x longer effective context windows.
- **EPIC** (2510.00515): Progressive consistency distillation for aggressive visual token reduction.

### Evolution Chain

```
Full fine-tuning of large VLMs (7B–72B)
→ LoRA/QLoRA adaptation (Thread 7)
→ Multi-teacher/multi-encoder distillation (MoVE-KD: CLIP+EVA+ConvNeXt → single encoder)
→ Active data curation + KD (ACED: complementary, not substitutes)
→ Architecture-aware compression (CASP: exploit VLM-specific attention sparsity)
→ Efficient vision encoding (FastVLM: 85x faster TTFT)
→ Ultra-small deployment (SmolVLM: 256M params, <1GB VRAM)
```

### Integration Recommendation

**Create new §5.4.X "2025: VLM Distillation & Efficient Deployment"** (~100-120 lines):
- Opening: deployment gap between research models (7B–72B) and practical requirements (<2B)
- Multi-encoder distillation: MoVE-KD (connect to Cambrian-1's finding)
- Compression: CASP (VLM-specific attention sparsity)
- Efficient architecture: FastVLM (Apple, 85x faster)
- Ultra-small: SmolVLM (256M outperforms 80B)
- Cross-reference to Thread 7 (efficiency)

---

## 6. Direction 5: Long-Context Multimodal Models {#6-direction-5}

### Narrative Context

The chapter's Open Problem 2 discusses long video understanding, citing Video-XL (VST), VideoLLaMA3 (similarity-based frame merging), and MLVU (evaluation). It identifies hour-scale temporal grounding and streaming as unsolved. New work significantly advances both — and reveals a fundamental bifurcation in approach.

### Paper Descriptions

#### Eagle 2.5: Long-Context Post-Training (NeurIPS 2025) — IMPORTANT
**arXiv**: 2504.15271 | **Authors**: Guo Chen et al. (NVIDIA)

Eagle 2.5 directly targets the post-training gap for long-context multimodal learning (as opposed to architectural changes). Two key innovations:
1. **Automatic Degrade Sampling**: Prevents catastrophic forgetting during long-context SFT by automatically sampling both long and short examples
2. **Image Area Preservation**: Maintains visual fidelity when processing many frames by preserving the total image area budget

The paper also introduces **Eagle-Video-110K** — a dataset with story-level and clip-level annotations for videos up to 3 hours long.

**Key result**: Eagle 2.5-8B achieves **72.4% on Video-MME**, matching GPT-4o and models 10x larger (Qwen2.5-VL-72B, InternVL2.5-78B).

#### Long-VITA: 1M Token Multimodal (2025) — IMPORTANT
**arXiv**: 2502.05177 | **Authors**: Yunhang Shen et al.

Long-VITA scales multimodal context to **1 million tokens** (4,000+ video frames) while maintaining short-context accuracy. The multi-stage training schema proceeds through:
1. Vision-language alignment
2. General knowledge learning
3. Medium-length fine-tuning
4. Long-sequence fine-tuning

Key technical contribution: context-parallelism distributed inference and logits-masked language modeling head for efficient 1M-token processing. Built entirely on 17M public samples.

**Key result**: First open model to concurrently process image, video, and text over 1M tokens with leading short-context accuracy. Demonstrates that long-context and short-context performance need not be traded off.

#### Temporal Chain of Thought (NeurIPS 2025) — IMPORTANT
**arXiv**: 2507.02001 | **Authors**: Anurag Arnab et al. (Google)

TCoT is a **training-free** inference strategy that uses the VLM itself to iteratively identify and extract the most relevant frames before answering. Rather than processing all frames uniformly (which wastes context on irrelevant content), TCoT performs multi-round frame selection:
1. Sample a sparse set of frames
2. Ask the VLM which time ranges are most relevant to the question
3. Sample more densely from those ranges
4. Repeat until budget is exhausted

**Key result**: A **32K context with TCoT outperforms the same VLM at 700K context by 2.8 points** on 1-hour+ videos. SOTA on 4 video QA datasets. NeurIPS 2025.

**Position in evolution**: This is the "smarter selection" approach — fundamentally different from "scale raw context." It demonstrates inference-time compute scaling for video QA analogous to LLM scaling laws.

#### StreamingVLM: Infinite Video Streams (2025) — IMPORTANT
**arXiv**: 2510.09608 | **Authors**: Ruyi Xu et al. (MIT Han Lab)

StreamingVLM addresses a qualitatively different problem from the above: **infinite-length streaming video** where the total video length is unknown at inference start. The method maintains a compact KV cache using:
- Attention sinks (keep the first few tokens that accumulate attention mass)
- Short window of recent vision tokens
- Long window of recent text tokens

SFT on short overlapped video chunks mimics inference-time streaming attention.

**Key result**: 66.18% win rate vs. GPT-4O mini on infinite-stream videos. Runs at **8 FPS on a single H100** with stable memory regardless of video length. Introduces Inf-Streams-Eval benchmark (average 2+ hour videos).

**Position in evolution**: This is a new sub-field. Previous models have fixed context windows; StreamingVLM has no upper bound.

#### MARC: RL-Based Token Compression (ICLR 2026) — IMPORTANT
**arXiv**: 2510.07915 | **Authors**: Peiran Wu et al.

MARC combines RL-based distillation with structured retrieval for video token compression:
1. **Visual Memory Retriever** selects key clips from the video
2. **Compression-GRPO (C-GRPO)** distills reasoning from teacher to student model

**Key result**: **95% token reduction** with near-zero accuracy cost. 72% GPU memory reduction. ICLR 2026.

**Position in evolution**: This is the first work to apply RL (specifically GRPO) to the token compression problem itself — not to improve reasoning, but to improve efficiency.

#### Other Notable Papers (MINOR)

- **PVC** (CVPR 2025, 2412.09613): Progressive Visual Token Compression — unifies images and videos by treating images as static videos. 64 tokens per frame.
- **BOLT** (CVPR 2025, 2503.21483): Training-free frame selection via inverse transform sampling based on query-frame similarity.
- **VideoScan** (2503.09387): 1 semantic carrier token per frame, memory independent of video length.

### Evolution Chain

```
Fixed context VLMs (LLaVA: 224px, ~2K tokens)
│
├── Raw context scaling:
│   ├── Video-XL: VST compression, 16x ratio, hour-scale
│   ├── Long-VITA: 1M tokens, 4K+ frames
│   └── Eagle 2.5: long-context post-training specifically (NeurIPS 2025)
│
├── Smart frame/token selection:
│   ├── TCoT: iterative VLM-guided frame selection (NeurIPS 2025)
│   ├── BOLT: query-based inverse transform sampling (CVPR 2025)
│   └── MARC: RL-based compression via C-GRPO (ICLR 2026)
│
└── Streaming (infinite length):
    ├── StreamingVLM: KV-cache with attention sinks, 8 FPS
    └── VideoScan: 1 token/frame, O(1) memory
```

### Integration Recommendation

**Expand Open Problem 2** and add content to the convergence section (~100-120 lines):
- The bifurcation thesis: raw scaling vs. smart selection
- Raw scaling: Long-VITA (1M), Eagle 2.5 (NeurIPS 2025)
- Smart selection: TCoT (NeurIPS 2025), MARC (ICLR 2026)
- Streaming as new sub-field: StreamingVLM
- Update OP2 to reflect what is now partially solved

---

## 7. Direction 6: Medical & Scientific Domain VLM Post-Training {#7-direction-6}

### Narrative Context

The chapter's §5.4.8 covers domain-specialized VLM RL for documents, charts, and GUI. But **medical and scientific domains are entirely absent** — a significant omission given the remarkable "R1 wave" phenomenon: GRPO-based RL applied to medical/scientific VLMs produces disproportionate gains, with 2B models routinely outperforming 72B ones using minimal training data.

This phenomenon deserves dedicated treatment because it reveals something fundamental about RL post-training: **domains with well-defined correctness signals and structured reasoning patterns are disproportionately amenable to RL**. Medical diagnosis has right/wrong answers. Pathology follows diagnostic workflows. Radiology reports have structured formats. These properties make RL reward design natural and effective.

### Paper Descriptions

#### MedVLM-R1: Medical Reasoning via GRPO (2025) — LANDMARK
**arXiv**: 2502.19634 | **Authors**: Pan et al.

MedVLM-R1 is the paper that catalyzed the medical R1 wave. Built on Qwen2-VL-2B (a tiny model), trained on **only 600 MRI VQA samples** with GRPO, it achieves:
- Accuracy jump from 55.11% to **78.22%** across MRI, CT, and X-ray benchmarks
- **Outperforms Qwen2-VL-72B** (a model 36x larger)
- **35% OOD improvement** on X-ray (not seen during training) over SFT baseline

This result is striking because it demonstrates that RL post-training is **disproportionately effective in expert domains with well-defined correctness signals**. Medical imaging has binary correct/incorrect answers, structured diagnostic reasoning, and domain-specific patterns — all properties that make RL reward design straightforward and the learning signal strong.

**Why LANDMARK**: This paper spawned an entire wave of medical VLM R1 papers (Med-R1, Patho-R1, PathVLM-R1, RadFlow, etc.) and demonstrated a general principle about when RL post-training is most effective.

#### Med-R1: Generalizable Medical Reasoning (2025) — IMPORTANT
**arXiv**: 2503.13939 | **Authors**: Yuxiang Lai et al.

Med-R1 extends MedVLM-R1 across **8 imaging modalities and 5 diagnostic tasks** via GRPO. Its most surprising finding challenges a core assumption of the reasoning-via-RL paradigm:

**Omitting intermediate rationales ("No-Thinking" mode) improves both in-domain and cross-domain generalization.** When the model is trained to directly produce answers without chain-of-thought, it performs better than when forced to generate reasoning chains. The authors hypothesize that in medical imaging, the reasoning patterns are simpler and more pattern-matching-based than in math — forcing the model to "show its work" introduces unnecessary noise.

**Key result**: Med-R1 (2B) achieves 69.91% average accuracy, a 29.94% gain over base Qwen2-VL-2B, outperforming 72B models.

**Position in evolution**: Directly extends MedVLM-R1 but challenges the universality of CoT reasoning in VLM RL.

#### BioReason: Genomic Reasoning via RL (NeurIPS 2025) — IMPORTANT
**arXiv**: 2505.23579 | **Authors**: Bowang Lab

BioReason pushes the boundary of "what counts as multimodal" by integrating **DNA foundation models with LLMs** for genomic reasoning. The model takes DNA sequences (not images) as the non-text modality and uses GRPO for biological reasoning refinement.

**Key result**: KEGG disease pathway prediction from 86% to **98%**; 15% average improvement in variant effect prediction. NeurIPS 2025.

**Position in evolution**: Demonstrates that the RL post-training paradigm works for any modality with verifiable outcomes — not just vision.

#### Patho-R1: Pathology Reasoning (2025) — IMPORTANT
**arXiv**: 2505.11404 | **Authors**: Wenchuan Zhang et al.

Three-stage pipeline specifically designed for pathology:
1. **Continued pretraining** on 3.5M pathology image-text pairs
2. **SFT** on 500K chain-of-thought samples derived from pathology textbooks
3. **RL** via GRPO with a novel "Decoupled Clip and Dynamic Sampling Policy Optimization"

The innovation is in Stage 2: instead of generic CoT, the training data is constructed from real-world pathologist diagnostic paradigms — mimicking the structured reasoning workflow of expert pathologists.

#### RadFlow: Hierarchical Reward for Radiology Reports (2025) — IMPORTANT
**arXiv**: 2511.10065

RadFlow is the first work applying RL to **long-form medical report generation** (as opposed to QA). The key innovation is a **hierarchical reward** that models the actual clinical reporting workflow:
- **Global reward**: Fluency + cross-section consistency (Findings and Impression sections must agree)
- **Local reward**: Section-specific quality (Impression section quality is evaluated separately)
- **Critical-aware policy optimization**: Higher reward weight for high-risk findings

Prior methods treated radiology reports as flat text sequences. RadFlow respects the structured clinical workflow.

#### Other Notable Papers (MINOR)

- **PathVLM-R1** (2504.09258): Dual reward for pathology — cross-modal process reward + outcome accuracy. Strong cross-domain transfer (pathology → CT, dermoscopy, fundus).
- **ChemVLM** (AAAI 2025, 2408.07246): First open-source chemistry-specific VLM with domain-specific vision encoder.
- **GeoVLM-R1** (2509.25026): First RL-based reasoning for remote sensing with task-aware rewards.
- **Lingshu** (2506.07044): Four-stage "shallow-to-deep" medical post-training + MedEvalKit evaluation framework.

### Evolution Chain

```
General VLM applied to medical tasks (poor performance)
→ Domain-specific SFT (LLaVA-Med, ChemVLM, Lingshu: medical data fine-tuning)
→ GRPO with rule-based rewards (MedVLM-R1: 600 samples, 2B beats 72B)
→ Multi-modality medical RL (Med-R1: 8 imaging modalities)
→ Structured diagnostic reasoning (Patho-R1: pathologist workflow as CoT template)
→ Hierarchical reward for generation (RadFlow: clinical report structure as reward)
→ Cross-domain reasoning (BioReason: genomics + language, NeurIPS 2025)
→ Non-vision multimodal RL (BioReason: DNA sequences as "visual" modality)
```

### Integration Recommendation

**Create new §5.4.X "2025: Medical & Scientific Domain VLM RL"** (~120-150 lines):
- Opening: the "R1 wave" phenomenon and why medical domains are disproportionately amenable to RL
- MedVLM-R1 as the catalyst (LANDMARK treatment): 600 samples, 2B beats 72B
- Med-R1's surprising "no-thinking" finding
- Structured domain workflows: Patho-R1 (pathology), RadFlow (radiology)
- Beyond vision: BioReason (NeurIPS 2025, genomics)
- Brief mentions: ChemVLM, GeoVLM-R1, PathVLM-R1
- Cross-reference to §5.4.8 (domain RL for GUI/charts) — same phenomenon, different domains
- Insight box: "Why does RL work so well in expert domains?"

---

## 8. Direction 7: Multimodal RAG {#8-direction-7}

### Narrative Context

Multimodal RAG is **entirely absent** from the chapter. This is a growing direction where VLMs augment their responses with retrieved multimodal information — particularly important for knowledge-intensive tasks where the VLM's parametric knowledge is insufficient (e.g., identifying rare objects, answering about specific documents).

### Paper Descriptions

#### VisRAG: Vision-Based RAG (ICLR 2025) — IMPORTANT
**arXiv**: 2410.10594 | **Authors**: Shi Yu et al.

VisRAG proposes a fundamentally different approach to document RAG: instead of parsing documents into text (losing layout, table structure, and figure information), it **directly embeds documents as images** for both retrieval and generation. The entire pipeline operates on visual representations.

**Key result**: **20-40% end-to-end gain** over text-based RAG. Accepted at ICLR 2025.

**Why this matters for post-training**: VisRAG implies that VLM post-training should include document-as-image understanding as a first-class capability, not just text extraction.

#### MMGraphRAG: Cross-Modal Knowledge Graphs (2025) — IMPORTANT
**arXiv**: 2507.20804 | **Authors**: Xueyao Wan, Hang Yu

MMGraphRAG fuses **visual scene graphs** (structured representations of objects and relationships in images) with **text knowledge graphs** via SpecLink (spectral-clustering entity linking). The cross-modal graph enables path-based retrieval across modalities.

**Key result**: 76.8% on DocBench vs. 59.5% NaiveRAG and 52.3% GraphRAG.

#### MMA-RAG: Adaptive Retrieval Gating (2026) — IMPORTANT
**arXiv**: 2603.00511 | **Authors**: Ruoshuang Du et al.

MMA-RAG addresses a fundamental question: **when should a VLM use retrieved external information, and when should it rely on its own knowledge?** The method trains a decision classifier via layer-wise analysis of joint visual+textual representations to dynamically gate external retrieval.

**Position in evolution**: Goes beyond "always retrieve" to "retrieve only when beneficial" — adaptive RAG for VLMs.

#### Other Notable Papers (MINOR)

- **MRAG Survey** (ACL 2025 Findings, 2504.08748): First comprehensive survey of multimodal RAG.
- **M4-RAG** (2512.05959): 80K+ samples, 42 languages. Reveals "retrieval hurts large models" phenomenon — large VLMs may perform worse with retrieval because they ignore or misuse external context.

### Integration Recommendation

**Create new §5.4.X "2025: Multimodal RAG"** (~80-100 lines):
- Opening: when parametric knowledge is insufficient
- VisRAG: documents as images, ICLR 2025
- MMGraphRAG: cross-modal knowledge graphs
- MMA-RAG: adaptive retrieval gating
- M4-RAG finding: retrieval can hurt large models
- Connection to post-training: what training data/objectives support effective RAG?

---

## 9. Direction 8: Multimodal Safety — From Open Problem to Full Narrative {#9-direction-8}

### Narrative Context

The chapter's Open Problem 3 discusses multimodal safety in only ~40 lines, citing 4 papers (Qi et al., FigStep, MM-SafetyBench, VLGuard). Since that writing, multimodal safety has exploded into a major research direction with:
- **Attack methods** achieving 94%+ success rates on production VLMs
- **Mechanistic understanding** of why VLMs are unsafe (representation-level analysis)
- **Omni-modal guardrails** that reason about safety across all modalities

This warrants promotion from a brief open problem to a **full subsection**.

### Paper Descriptions

#### OmniGuard: Omni-Modal Guardrails with Reasoning (2025) — IMPORTANT
**arXiv**: 2512.02306 | **Authors**: Boyu Zhu et al.

OmniGuard is the **first omni-modal guardrail** covering text, image, video, and audio simultaneously. Unlike binary safety classifiers (VLGuard: safe/unsafe), OmniGuard uses **deliberate reasoning** — it generates chain-of-thought explanations for its safety decisions.

The model is trained on 210K+ diverse omni-modal safety samples, covering both direct harmful content and indirect attacks (typographic, adversarial, cross-modal embedding).

**Position in evolution**: Extends safety from text-only (Thread 3) → text+image (VLGuard) → all modalities with reasoning.

#### GuardReasoner-Omni: GRPO for Guardrails (2026) — IMPORTANT
**arXiv**: 2602.03328

A 2B-parameter guardrail model trained with **GRPO reinforcement learning** — the same GRPO used throughout this chapter for reasoning and alignment. The model employs explicit temporal reasoning for video threats (understanding that a sequence of benign frames can compose into harmful content).

**Key result**: Surpasses runner-up by 5.3% F1 score.

**Position in evolution**: Demonstrates that RL post-training techniques developed for reasoning (GRPO) transfer directly to safety — the "R1 wave" reaches safety.

#### Safe RLHF-V: Multimodal Safety RLHF (NeurIPS 2025) — IMPORTANT
**arXiv**: 2503.17682 | **Authors**: Jiaming Ji et al.

Safe RLHF-V is the **first multimodal safety alignment framework** that jointly optimizes helpfulness and safety via **Lagrangian constrained optimization**. Prior work (LLaVA-RLHF, mDPO) conflated safety with general preference — treating "unsafe" as just another kind of "bad response." Safe RLHF-V instead treats safety as a **hard constraint** rather than a soft preference.

The method also introduces:
- **BeaverTails-V**: Large-scale multimodal dataset with dual preference annotations (helpfulness and safety scored independently)
- **Beaver-Guard-V**: A deployable safety guardrail

**Key result**: **+34.2% safety improvement + 34.3% helpfulness improvement** simultaneously — demonstrating that safety and helpfulness are not inherently opposed.

#### COMET: Reasoning-Exploiting Attacks (2026) — IMPORTANT
**arXiv**: 2602.10148

COMET achieves **94%+ attack success rate across 9 VLMs** (including GPT-4o) by exploiting the VLMs' own multimodal reasoning capabilities as the attack surface. Three techniques:
1. Knowledge-scalable reframing into multi-hop chains
2. Cross-modal clue entangling (migrating key entities into images)
3. Cross-modal scenario nesting

**Position in evolution**: This represents a qualitative shift in attacks — from adversarial perturbations (Qi et al.) and typographic tricks (FigStep) to **exploiting reasoning capabilities**. More capable VLMs are actually more vulnerable.

#### JRS-Rem: Representation-Level Defense (2026) — IMPORTANT
**arXiv**: 2603.17372

JRS-Rem discovers that jailbreak samples form a **distinct, separable internal state** in VLM representation space. The defense operates directly on internal representations — removing the jailbreak-related activation shift at inference time.

**Position in evolution**: Goes beyond input-level defenses (filters, paraphrasing) to representation-level interventions. Provides a unified mechanistic explanation for diverse jailbreak types.

#### ShiftDC: Safety Perception Distortion (2025) — MINOR
**arXiv**: 2502.13095

Identifies "safety perception distortion" — multimodal inputs shift activations toward a "safer" direction, causing VLMs to **systematically overestimate safety** of harmful inputs. Proposes training-free calibration.

#### Other Notable Papers (MINOR)

- **BlueSuffix** (ICLR 2025, 2410.20971): RL-trained defensive suffix generator for VLMs.
- **Omni-SafetyBench** (2508.07173): First parallel safety benchmark for omni-modal LLMs (24 modality variations × 972 samples).

### Evolution Chain

```
Image-level adversarial attacks (Qi et al. 2023: adversarial perturbation)
→ Typographic attacks (FigStep 2023: render harmful text as image)
→ Reasoning-exploiting attacks (COMET 2026: 94% ASR via multi-hop reasoning)
│
Binary safety classifiers (VLGuard: safe/unsafe, minimal cost)
→ Mechanistic understanding:
│   ├── ShiftDC: safety perception distortion (why VLMs are unsafe)
│   └── JRS-Rem: jailbreak forms separable representation states
→ Reasoning-based guardrails:
│   ├── OmniGuard: omni-modal + deliberate reasoning
│   └── GuardReasoner-Omni: GRPO-trained, temporal video reasoning
→ Constrained optimization (Safe RLHF-V: Lagrangian helpfulness-safety, NeurIPS 2025)
```

### Integration Recommendation

**Promote Open Problem 3 to a full subsection** "2025-2026: Multimodal Safety Alignment" (~120-150 lines):
- Move existing OP3 content as the opening
- Attack evolution: adversarial → typographic → reasoning-exploiting (COMET)
- Mechanistic understanding: ShiftDC, JRS-Rem
- Defense evolution: VLGuard → OmniGuard → GuardReasoner-Omni
- Safety as constraint: Safe RLHF-V (NeurIPS 2025)
- New OP: the arms race acceleration problem

---

## 10. Direction 9: Native Multimodal Pre-Training & Early Fusion {#10-direction-9}

### Narrative Context

The chapter's §5.4.3 covers unified generation architectures (Chameleon, Emu3, Janus, Show-o, Transfusion). New work adds **empirical scaling laws** and **industrial-scale implementations**.

### Paper Descriptions

#### Scaling Laws for Native Multimodal Models (2025) — IMPORTANT
**arXiv**: 2504.07951 | **Authors**: Apple (Mustafa Shukor et al.)

A **457-model scaling study** comparing early-fusion vs. late-fusion architectures across parameter counts. Key findings:
1. **Early fusion shows stronger performance at lower parameter counts**
2. Early fusion is more efficient to train and easier to deploy
3. **MoE significantly benefits native multimodal models**

This is the **first rigorous scaling law study** specifically for native multimodal models, providing empirical guidance for architecture selection.

**Position in evolution**: Provides the theoretical backing for the architectural choices made by Chameleon (early fusion) and Janus-Pro (decoupled but sharing backbone).

#### ERNIE 5.0: Industrial-Scale Native Multimodal (2025) — IMPORTANT
**arXiv**: 2602.04705 | **Authors**: Baidu

ERNIE 5.0 is a **2.4 trillion parameter** unified autoregressive model trained natively on text/image/video/audio in a shared token space. The key innovation is **Next-Group-of-Tokens Prediction (NGTP)** with modality-specific variants:
- **NFSP** (Next-Frame-Sequence Prediction) for vision
- **NCP** (Next-Chunk Prediction) for audio

NGTP generalizes next-token prediction across modalities rather than using modality-specific training objectives.

**Position in evolution**: Largest publicly documented native multimodal model. Demonstrates industrial viability of the approach.

#### Other Notable Papers (MINOR)

- **FuseLIP** (2506.03096): Early fusion for embeddings/retrieval setting. Outperforms CLIP-style late fusion on VQA.
- **Unified Models Survey** (2505.02567): Taxonomy by sequence representation (pixel/token/multiple-tokens).
- **Discrete Tokenization Survey** (2507.22920): First dedicated survey on tokenization infrastructure.

### Integration Recommendation

**Expand §5.4.3** with ~40 lines:
- Apple's scaling laws (empirical guidance for early vs. late fusion)
- ERNIE 5.0 (NGTP: generalized next-token prediction, 2.4T scale)
- Brief mention of surveys for interested readers

---

## 11. Direction 10: VLM Evaluation & Benchmarks {#11-direction-10}

### Paper Descriptions

#### MMMU-Pro (ACL 2025) — IMPORTANT
**arXiv**: 2409.02813

Filters out text-only-solvable questions from MMMU, adds augmented candidate options, and introduces vision-only input. Model performance drops to 16.8-26.9% vs. original MMMU. Exposes shortcut exploitation.

#### Video-MMMU (2025) — IMPORTANT
**arXiv**: 2501.13826

First benchmark treating videos as a **knowledge source** (not just visual content). 300 expert-level videos, 900 questions. Proposes "Delta-knowledge" metric measuring improvement after watching video lectures.

#### VLM-RobustBench (2026) — IMPORTANT
**arXiv**: 2603.06148

49 augmentation types, 133 corrupted settings. Key finding: VLMs are "**semantically strong but spatially fragile**" — low-severity spatial perturbations hurt more than visually severe photometric corruptions.

#### Other Notable Papers (MINOR)

- **VidHalluc** (CVPR 2025): 5,002 videos for temporal hallucination evaluation.
- **ODE** (CVPR 2025): Dynamic graph-based hallucination evaluation that continuously generates new test samples.
- **GEOBench-VLM** (ICCV 2025): 31 geospatial tasks, best VLM only 41.7%.
- **MINDCUBE** (2506.21458): Spatial mental models from limited views (Li Fei-Fei lab).

### Integration Recommendation

Update the paper list (thread5-multimodal.md) and add brief mentions in relevant sections. No new subsection needed — benchmarks are best cited where their findings are relevant.

---

## 12. Direction 11: Emerging — Self-Improvement Without Human Labels {#12-direction-11}

### Narrative Context

A new paradigm is emerging where VLMs generate their own training signal, eliminating the need for human annotation, preference datasets, or external reward models. This connects to the chapter's DPE/VisPlay work (§5.4.5) but goes further — from self-generated *data* to self-generated *rewards*.

### Paper Descriptions

#### Vision-Zero: Gamified Self-Play (2025) — IMPORTANT
**arXiv**: 2509.25541 | **Authors**: Qinsi Wang et al.

Vision-Zero is the first **gamified self-play framework** for VLM self-improvement. The method trains VLMs via "Who Is the Spy" games on arbitrary image pairs:
- Two images are shown, one with a subtle difference
- VLM agents must identify which is the "spy" image
- The game generates its own reward signal (correct identification)

This completely eliminates human annotation. The training signal comes from competitive visual games.

Combined with **Iterative-SPO** (alternating self-play with RLVR), Vision-Zero achieves significant improvements using only unlabeled images.

#### Vision-R1: Vision-Guided RL Without Reward Models (2025) — IMPORTANT
**arXiv**: 2503.18013

Vision-R1 eliminates human involvement entirely for VLM alignment by using **definitive vision feedback** as the reward signal. For tasks with verifiable visual outputs (matching, counting, spatial reasoning), the correctness of the VLM's response can be determined programmatically from the image.

The method includes **progressive rule refinement** that dynamically adjusts reward criteria during training.

**Key result**: Up to **50% improvement** on 7B models without any human labels or preference data.

#### Vision-SR1: Self-Rewarding via Decomposition (2025) — IMPORTANT
**arXiv**: 2508.19652

Vision-SR1 decomposes VLM reasoning into two stages:
1. **Visual perception**: Generate a self-contained description of what the model "sees"
2. **Language reasoning**: Use only the text description (not the image) to answer

The reward is computed by comparing the model's answer (stage 2, text-only) against the ground truth. If the text-only reasoning would give the right answer but the full multimodal pipeline doesn't, the failure is in perception. If both fail, the failure is in reasoning.

This decomposition enables **self-rewarding without external models** and reveals "thinking over seeing" shortcuts in R1-style VLMs.

#### Argos: Agentic Reward Verifier (2025) — IMPORTANT
**arXiv**: 2512.03438 | **Authors**: Microsoft Research

Argos introduces an **agentic reward verifier** for multimodal agent training. The key finding is that **SFT alone causes agents to collapse to ungrounded solutions** — agents learn to produce plausible-looking action sequences that don't actually interact with the environment correctly. Online RL with verified rewards prevents this collapse.

**Position in evolution**: Bridges multimodal post-training and agentic AI, connecting to Thread 6.

#### Other Notable Papers (MINOR)

- **MLLM-CL** (2506.05453): First benchmark for continual learning in MLLMs. Distinguishes domain CL (IID) from ability CL (non-IID).
- **VLM-CL Survey** (2508.04227): First survey on VLM continual learning. VLM-specific challenges: cross-modal drift, zero-shot erosion.

### Integration Recommendation

**Expand §5.4.5 (Self-Evolving VLM Training)** with a new paragraph on "self-rewarding" approaches (~60-80 lines):
- Vision-Zero: gamified self-play
- Vision-R1: vision-intrinsic reward
- Vision-SR1: reasoning decomposition for self-reward
- Connect to DPE's self-evolving paradigm

**Add "VLM Continual Learning" as a new Open Problem** if space permits.

---

## 13. Direction 12: Multimodal Preference Learning & DPO Variants {#13-direction-12}

### Narrative Context

Branch A (§5.3.1) covers early multimodal preference optimization: LLaVA-RLHF, mDPO, RLAIF-V, Silkie, HA-DPO, SeVa, DRESS. Since that writing, preference learning for VLMs has matured significantly — moving from "apply text DPO to VLMs" to "design modality-aware preference optimization."

### Paper Descriptions

#### UnifiedReward-Think (NeurIPS 2025) — IMPORTANT
**arXiv**: 2505.03318 (builds on 2503.05236)

Two contributions:
1. **UnifiedReward**: First **unified reward model** that jointly assesses both understanding and generation tasks. Prior reward models were specialized for one or the other.
2. **UnifiedReward-Think**: Adds **CoT-based multi-dimensional step-by-step reasoning** to reward evaluation via reinforcement fine-tuning. The reward model doesn't just output a score — it reasons about why a response is better or worse across multiple dimensions.

**Position in evolution**: This directly addresses the "blind judge" problem identified in §5.4.7 — by making the reward model reason about its judgments, it becomes more reliable and interpretable.

#### Generative RLHF-V: Generative Reward Models (2025) — IMPORTANT
**arXiv**: 2505.18531

Replaces discriminative reward models (output a scalar score) with **generative reward models** (output a natural language critique + score). The generative reward model is trained via RL to learn principles from multimodal preferences.

**Key result**: **+18.1% average improvement** across 4 MLLMs, vs. only +5.3% for standard discriminative RLHF pipeline. The 3.4x improvement suggests that generative rewards provide fundamentally richer training signal.

#### MoD-DPO: Modality-Decoupled Preference (2026) — IMPORTANT
**arXiv**: 2603.03192 | **Authors**: USC

MoD-DPO is the first DPO variant designed for **omni-modal (audio+video+text) LLMs**. Two innovations:
1. **Modality-aware regularization**: Forces the model to be invariant to irrelevant-modality corruptions but sensitive to relevant-modality perturbations
2. **Language-prior debiasing**: Penalizes the model for hallucinations that could be generated from text alone (without looking at the image/audio)

**Position in evolution**: Extends mDPO (which addressed image conditioning) to the omni-modal setting.

#### Other Notable Papers (MINOR)

- **DA-DPO** (2601.00623): Difficulty-aware reweighting of preference pairs. Curriculum-like approach.
- **SymMPO** (2506.11712): Theoretically grounded symmetric DPO extension.
- **MISP-DPO** (2509.25717): Multi-negative DPO via Plackett-Luce model.
- **OmniDPO** (2509.00723): First DPO for omni-modal hallucination.

### Integration Recommendation

**Expand Branch A (§5.3.1)** with ~60-80 lines or add to the convergence section:
- Evolution from naive DPO → modality-aware DPO (mDPO → MoD-DPO → OmniDPO)
- Reward model evolution: scalar → generative (Generative RLHF-V) → unified + reasoning (UnifiedReward-Think)
- Brief mentions of DA-DPO, SymMPO, MISP-DPO

---

## 14. Cross-Cutting Themes {#14-cross-cutting-themes}

### Theme 1: The "R1 Wave" — GRPO as Universal Post-Training Algorithm

GRPO (Group Relative Policy Optimization) has become the de facto standard for RL-based multimodal post-training. As of early 2026, it has been applied to:

| Domain | Representative Work |
|--------|-------------------|
| General VLM reasoning | R1-VL, MM-Eureka, VLM-R1 |
| Process-level supervision | URSA PS-GRPO, VisualPRM |
| Perception-aware reasoning | Perception-R1, PEARL |
| Medical imaging | MedVLM-R1, Med-R1, Patho-R1 |
| Pathology | Patho-R1, PathVLM-R1 |
| Radiology | RadFlow |
| Genomics | BioReason |
| Remote sensing | GeoVLM-R1 |
| 3D spatial reasoning | 3D-R1, Smooth Operator |
| GUI agents | UI-TARS-2, UI-AGILE |
| Document navigation | SCoPE VLM |
| Chart understanding | BigCharts-R1 |
| Safety guardrails | GuardReasoner-Omni |
| Reward model training | R1-Reward |
| Multimodal generation | Interleaved GRPO |
| Token compression | MARC C-GRPO |
| Self-improvement | DPE, VisPlay, Vision-Zero |

**The innovation is NOT in the RL algorithm** (GRPO remains largely unchanged from the original DeepSeek-R1 formulation). The innovations are in:
1. **Reward design**: perception rewards, process rewards, self-rewards, domain-specific rewards, hierarchical rewards
2. **Training stability**: staged RL, selective replay, on-policy updates, gated reasoning
3. **Data efficiency**: 600-sample medical RL, 1,442-sample perception RL

### Theme 2: The Perception Bottleneck

Multiple independent findings converge on a single insight: **VLM failures are primarily perceptual, not reasoning-based**.

Evidence:
- **VL-RewardBench** (§5.4.7): Judge failures concentrate on visual perception, not logic
- **Test-time scaling analysis** (§5.4.6): Gains only on reasoning tasks, not perception tasks
- **Perception-R1**: Standard RLVR improves reasoning but NOT perception
- **PEARL**: Reasoning chains built on hallucinated visual evidence
- **VLM-RobustBench**: Low-severity spatial perturbations hurt more than severe photometric ones
- **Med-R1**: Removing explicit reasoning (No-Thinking mode) actually helps in perception-heavy medical tasks
- **Virgo**: Slow-thinking capability transfers from text-only training, confirming it's a language property

**Implication**: The next breakthrough in multimodal post-training must come from **improving visual perception during/after training**, not just improving reasoning chains. Perception-R1 and PEARL are early attempts, but the problem remains largely open.

### Theme 3: Self-Improvement Without Human Labels

A new paradigm is coalescing where VLMs train themselves:

| Method | Self-Signal Source |
|--------|-------------------|
| Vision-Zero | Competitive self-play games |
| Vision-R1 | Programmatic vision verification |
| Vision-SR1 | Decomposed perception/reasoning self-assessment |
| Perception-R1 | Visual annotation as reward |
| DPE | Diagnostic-driven self-generated data |
| VisPlay | Questioner/Reasoner self-play |
| SUDO | Self-supervised image transformation preferences |
| R1-Reward | RL-trained reward model (meta-RL) |

This points toward **zero-annotation post-training** — where the only human input is the architecture choice and the initial pre-trained weights. The feasibility depends on having verifiable tasks (where correctness can be determined programmatically).

### Theme 4: Alignment for Generation ≠ Alignment for Understanding

The chapter already notes this tension (Open Problem 5). New evidence:

| Property | Understanding Alignment | Generation Alignment |
|----------|----------------------|---------------------|
| Reward type | Correctness, faithfulness | Aesthetic quality, user intent |
| DPO pathology | Unconditional preference (mDPO) | Winner degradation, likelihood displacement |
| Theoretical basis | Token-level log-probability | Score function space (DSPO) |
| Data requirement | Preference pairs (text judgments) | Image/video quality ratings |
| Self-supervised option | Vision-intrinsic verification | Image transformation preferences (SUDO) |

RecA and Interleaved GRPO are the first attempts to bridge both, but the fundamental tension remains.

---

## 15. Proposed Chapter Structure {#15-proposed-chapter-structure}

### Current Structure (§5.4 收敛与前沿)

```
5.4.1  Multimodal Reasoning via RL
5.4.2  VLM Architecture & Open-Source Scaling
5.4.3  Unified Multimodal Generation
5.4.4  Omni-Modal Post-Training
5.4.5  Self-Evolving VLM Training
5.4.6  Test-Time Compute Scaling for VLMs
5.4.7  Multimodal Reward Models & VLM-as-Judge
5.4.8  Domain-Specialized VLM RL
5.4.9  Spatial & 3D Reasoning Post-Training
5.4.10 World Models as Data Engines
```

### Proposed Expanded Structure

```
5.4.1   Multimodal Reasoning via RL                     [EXPAND: +7 papers]
5.4.2   VLM Architecture & Open-Source Scaling           [minor updates]
5.4.3   Unified Multimodal Generation                    [EXPAND: +2 papers]
5.4.3b  Generation Alignment: RLHF/DPO for Diffusion    [NEW: 6-8 papers]
5.4.4   Omni-Modal Post-Training                         [minor updates]
5.4.5   Self-Evolving VLM Training                       [EXPAND: +4 self-reward papers]
5.4.6   Test-Time Compute Scaling for VLMs               [minor updates]
5.4.7   Multimodal Reward Models & VLM-as-Judge          [EXPAND: +3 papers]
5.4.8   Domain-Specialized VLM RL                        [EXPAND: add medical subsection]
5.4.8b  Medical & Scientific Domain VLM RL               [NEW: 5-6 papers]
5.4.9   Spatial & 3D Reasoning Post-Training             [minor updates]
5.4.10  World Models as Data Engines                     [minor updates]
5.4.11  Multimodal Data Synthesis & Curation             [NEW: 5-6 papers]
5.4.12  VLM Distillation & Efficient Deployment          [NEW: 5-7 papers]
5.4.13  Multimodal Safety Alignment                      [NEW from OP3: 6-8 papers]
5.4.14  Multimodal RAG                                   [NEW: 3-4 papers]
5.4.15  Multimodal Preference Learning Evolution         [NEW/moved from Branch A: 4-5 papers]

5.5 Open Problems (updated)
  OP1: Fine-Grained Visual Understanding                 [keep]
  OP2: Long Video & Temporal Alignment                   [update with Eagle 2.5, TCoT, StreamingVLM]
  OP3: → promoted to §5.4.13                            [replaced by new OP]
  OP4: Deep Multimodal Reasoning                         [update with Perception-R1 findings]
  OP5: Post-Training for Unified Generation              [update with RecA, GRPO for generation]
  OP6: Omni-Modal Cross-Modal Consistency                [keep]
  OP7: Self-Improvement Without Human Labels             [NEW]
  OP8: Perception as the True Bottleneck                 [NEW - cross-cutting theme]
  OP9: Multimodal Continual Learning                     [NEW]
```

### Estimated Effort

| Change Type | Count | Lines Added |
|-------------|-------|-------------|
| New subsections | 6 | ~700-800 |
| Expanded subsections | 4 | ~300-400 |
| Updated open problems | 5 | ~150-200 |
| Total | | ~1150-1400 |

This would bring the chapter from ~1800 to ~2950-3200 lines, making it the most comprehensive thread.
