# Design: LLM/VLM Post-Training Survey

**Date**: 2026-03-04
**Author**: Jinhong Lin
**Status**: Approved

## Goal

A personal survey for PhD research covering the full landscape of LLM/VLM post-training, organized as an evolutionary narrative tracing problem-solution chains.

## Format

- LaTeX, academic style
- Mixed Chinese/English (Chinese text, English technical terms)
- Deep analysis for landmark papers (1-2 pages), 1 paragraph for important follow-ups, 1-2 sentences for minor papers

## Architecture: Hybrid Problem-Evolution Graph

7 problem threads, each with chronological evolution. Cross-references model idea flow between threads.

### Threads

| # | Thread | Core Question | Time Span |
|---|--------|--------------|-----------|
| 1 | Instruction Following & SFT | How to make pre-trained models follow human instructions? | 2021-2024 |
| 2 | Reward Modeling & Preference Optimization | How to learn and optimize human preferences? | 2017-2025 |
| 3 | Safety & Alignment | How to ensure safe and controllable model behavior? | 2022-2025 |
| 4 | Reasoning & Verification | How to improve and verify reasoning capabilities? | 2022-2025 |
| 5 | Multimodal Post-training | How to extend alignment to multimodal models? | 2023-2025 |
| 6 | Tool Use & Agentic Capabilities | How to train models to use tools and act autonomously? | 2023-2025 |
| 7 | Efficiency & Data Engineering | How to reduce the cost of post-training? | 2023-2025 |

### Cross-Reference Map

```
Thread 1 (SFT) ──────→ Thread 5 (Multimodal: visual instruction tuning inherits SFT paradigm)
Thread 2 (RLHF/DPO) ──→ Thread 3 (Safety: Safe RLHF, Constitutional AI uses RL)
Thread 2 (RLHF/DPO) ──→ Thread 4 (Reasoning: RL for reasoning, GRPO)
Thread 2 (RLHF/DPO) ──→ Thread 5 (Multimodal: VLM RLHF)
Thread 4 (Reasoning) ──→ Thread 6 (Agentic: reasoning enables planning & tool use)
Thread 7 (Efficiency) ──→ ALL (LoRA, data selection applicable everywhere)
```

### Thread Internal Structure

Each thread follows:

1. **Problem Origin** — Why did this problem emerge? What gap did pre-training leave?
2. **Foundational Approach** — Deep analysis of landmark paper(s) that defined the approach
3. **Limitations & Branching** — What problems did the foundation have? How did the field branch?
4. **Convergence & State-of-the-Art** — How did branches converge? Current best practices?
5. **Open Problems** — What core problems remain unsolved? Cross-refs to other threads.

### Thread Content Sketches

**Thread 1: Instruction Following & SFT**
- Origin: Pre-trained LLMs don't follow instructions
- Foundation: FLAN (2022), InstructGPT-SFT (2022)
- Branches: data scaling (Self-Instruct, Alpaca), data quality (LIMA, less is more), multi-task (T0, Flan-T5)
- Convergence: High-quality SFT data + scaling
- Open: Data curation automation, SFT ceiling

**Thread 2: Reward Modeling & Preference Optimization**
- Origin: SFT alone can't capture nuanced human preferences
- Foundation: Christiano et al. 2017, InstructGPT RLHF pipeline (2022)
- Branches: reward hacking (Gao et al.), RL-free methods (DPO, IPO, KTO, SimPO, ORPO), online vs offline, AI feedback (RLAIF)
- Convergence: DPO variants + iterative training
- Open: Reward generalization, preference diversity

**Thread 3: Safety & Alignment**
- Origin: Models can produce harmful content
- Foundation: Constitutional AI (2022)
- Branches: RLAIF, Safe RLHF, red-teaming, refusal training
- Convergence: Multi-layered safety (training + inference)
- Open: Jailbreak robustness, over-refusal

**Thread 4: Reasoning & Verification**
- Origin: LLMs struggle with multi-step reasoning
- Foundation: Chain-of-Thought (Wei et al. 2022)
- Branches: PRM vs ORM, self-consistency, CoT distillation, RL for reasoning (o1, DeepSeek-R1, GRPO)
- Convergence: RL + verifiable rewards + long CoT
- Open: Generalization beyond math, process supervision cost

**Thread 5: Multimodal Post-training**
- Origin: VLMs need alignment too
- Foundation: LLaVA visual instruction tuning (2023)
- Branches: VLM RLHF, hallucination reduction, grounding, multi-image/video
- Convergence: SFT + DPO pipeline for VLMs
- Open: Fine-grained visual understanding, video alignment

**Thread 6: Tool Use & Agentic Capabilities**
- Origin: LLMs can't interact with external world
- Foundation: Toolformer (2023)
- Branches: Function calling training, agent tuning (FireAct), code generation alignment, multi-tool orchestration
- Convergence: Function calling + planning training
- Open: Reliable multi-step agent behavior, error recovery

**Thread 7: Efficiency & Data Engineering**
- Origin: Post-training is expensive (compute + human annotation)
- Foundation: LoRA for alignment, synthetic data generation
- Branches: Parameter-efficient (LoRA/QLoRA), data-efficient (data selection, curriculum), compute-efficient (distillation)
- Convergence: Efficient SFT + synthetic preference data
- Open: Quality-efficiency tradeoff, scaling laws for post-training data

## File Structure

```
Post-training/
├── main.tex
├── preamble.tex
├── references.bib
├── chapters/
│   ├── 00-introduction.tex
│   ├── 01-sft.tex
│   ├── 02-preference.tex
│   ├── 03-safety.tex
│   ├── 04-reasoning.tex
│   ├── 05-multimodal.tex
│   ├── 06-agentic.tex
│   ├── 07-efficiency.tex
│   └── 08-conclusion.tex
└── figures/
    └── overview.tex
```

## Special Elements

1. **Introduction** — Overview evolution diagram (TikZ) showing all 7 threads and their relationships
2. **Cross-reference macros** — `\crossref{thread}{section}`, `\landmark{citekey}`, `\evolution{from}{to}{reason}`
3. **Conclusion** — Macro-trend analysis: RL-based → RL-free, text → multimodal, human → AI feedback
