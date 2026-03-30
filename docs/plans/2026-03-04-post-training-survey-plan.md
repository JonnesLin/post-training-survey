# Post-Training Survey Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a complete LaTeX survey on LLM/VLM post-training organized as 7 evolutionary problem threads with cross-references.

**Architecture:** Hybrid Problem-Evolution Graph — 7 problem threads with chronological evolution within each thread, cross-references between threads, and a TikZ overview diagram. Each thread follows: Problem Origin → Foundation → Limitations & Branching → Convergence → Open Problems.

**Tech Stack:** LaTeX (article class), BibTeX, TikZ, xcolor, hyperref

---

### Task 1: LaTeX Scaffold — preamble.tex + main.tex

**Files:**
- Create: `preamble.tex`
- Create: `main.tex`

**Step 1: Create `preamble.tex`**

Define all packages, custom cross-reference macros (`\crossref`, `\landmark`, `\evolution`), color scheme for threads, and document settings. Mixed Chinese/English support via `ctex` or `xeCJK`.

Key macros:
- `\crossref{N}{M}` → renders "→ 见 §N.M" with hyperlink
- `\landmark{citekey}` → bold/colored citation for landmark papers
- `\evolution{from}{to}{reason}` → renders an evolution arrow annotation
- `\threadcolor{N}` → returns the color assigned to thread N

**Step 2: Create `main.tex`**

Document shell that `\input{preamble}`, sets title/author, and `\include`s all chapter files from `chapters/`. Add `\bibliography{references}` at the end.

**Step 3: Verify compilation**

Run: `xelatex main.tex`
Expected: Compiles with empty chapters (warnings about missing includes are OK)

**Step 4: Commit**

```
git init && git add preamble.tex main.tex
git commit -m "scaffold: LaTeX project with preamble and main entry"
```

---

### Task 2: Chapter 00 — Introduction + Overview Diagram

**Files:**
- Create: `chapters/00-introduction.tex`
- Create: `figures/overview.tex`

**Step 1: Write Introduction text**

Content (mixed Chinese/English):
1. Post-training 的定义与重要性（区分 pre-training 和 post-training）
2. 为什么需要这篇 survey（现有 survey 的不足 + 本文的演化视角）
3. 本文的组织方式：7 个 problem threads，每个 thread 的演化叙事，threads 之间的 cross-reference
4. 简述每个 thread 的核心问题（1 句话 × 7）

**Step 2: Create TikZ overview diagram**

`figures/overview.tex` — A timeline-based diagram showing:
- 7 horizontal lanes (one per thread), color-coded
- Key papers as nodes on each lane, positioned by year (2017-2025)
- Cross-reference arrows between threads (dashed, labeled)
- Legend explaining landmark vs normal papers

**Step 3: Verify compilation**

Run: `xelatex main.tex`
Expected: Introduction renders with the overview figure

**Step 4: Commit**

```
git add chapters/00-introduction.tex figures/overview.tex
git commit -m "content: introduction and overview evolution diagram"
```

---

### Task 3: Chapter 01 — Thread 1: Instruction Following & SFT

**Files:**
- Create: `chapters/01-sft.tex`
- Update: `references.bib` (add relevant citations)

**Step 1: Write §1.1 Problem Origin**

Pre-trained LLMs (GPT-3) 能生成流畅文本但不遵循指令。Few-shot prompting 是临时方案但不 scalable。需要一种方法让模型从"语言模型"变成"助手"。

**Step 2: Write §1.2 Foundational Approach**

[DEEP] FLAN (Wei et al., 2022) — instruction tuning across tasks
- 核心思想：将多种 NLP 任务统一为 instruction format 进行 fine-tuning
- 关键发现：instruction tuning 在 unseen tasks 上泛化
- 数据格式设计、task 数量 scaling

[DEEP] InstructGPT-SFT (Ouyang et al., 2022) — human-written demonstrations
- 区别于 FLAN：用人类标注员写示范而非转换已有数据集
- SFT 阶段的具体流程、数据量、效果

**Step 3: Write §1.3 Limitations & Branching**

Branch A: Data Quantity — Self-Instruct (Wang et al., 2023), Alpaca (Taori et al., 2023)
- Problem: 人类标注 expensive → 用 LLM 自动生成 instruction data
- Self-Instruct 的 pipeline, Alpaca 的简化版

Branch B: Data Quality — LIMA (Zhou et al., 2023)
- Problem: 数据越多越好吗？→ "Less is More" 发现
- 1000 条高质量数据 vs 大量低质量数据

Branch C: Multi-task Scaling — Flan-T5/Flan-PaLM, T0
- 更大规模的 task 数量 + CoT 数据混合

**Step 4: Write §1.4 Convergence + §1.5 Open Problems**

Convergence: 高质量数据 + 适度规模 + 多样性 = 当前最佳实践
Open: SFT 的上限在哪？SFT vs RLHF 的角色分工？→ \crossref{2}{1}

**Step 5: Add BibTeX entries**

Add all cited papers to `references.bib`

**Step 6: Commit**

```
git add chapters/01-sft.tex references.bib
git commit -m "content: Thread 1 — Instruction Following & SFT"
```

---

### Task 4: Chapter 02 — Thread 2: Reward Modeling & Preference Optimization

**Files:**
- Create: `chapters/02-preference.tex`
- Update: `references.bib`

**Step 1: Write §2.1 Problem Origin**

SFT 让模型遵循指令，但无法捕捉微妙的质量偏好。人类判断"好坏"比"写出好回答"更容易。

**Step 2: Write §2.2 Foundational Approach**

[DEEP] Christiano et al. (2017) — Learning from human feedback
- 核心框架：reward model from comparisons + RL optimization
- 公式：Bradley-Terry model, reward loss

[DEEP] InstructGPT (Ouyang et al., 2022) — The SFT → RM → PPO pipeline
- 三阶段 pipeline 的完整流程
- PPO objective, KL penalty
- 关键实验：1.3B RLHF > 175B SFT

**Step 3: Write §2.3 Limitations & Branching**

Branch A: Reward Hacking
- [DEEP] Gao et al. (2023) — Scaling laws for reward model overoptimization
- KL-reward frontier, overoptimization 的 root cause

Branch B: RL-Free Preference Optimization
- [DEEP] DPO (Rafailov et al., 2023) — Direct Preference Optimization
  - 推导过程：从 RLHF objective 到 closed-form solution
  - DPO loss function 的直觉解释
  - DPO vs PPO 实验对比
- [MEDIUM] IPO (Azar et al., 2023) — 修复 DPO 的 overfitting
- [MEDIUM] KTO (Ethayarajh et al., 2024) — 不需要 paired preferences
- [MEDIUM] SimPO (Meng et al., 2024) — reference-free
- [MEDIUM] ORPO (Hong et al., 2024) — 合并 SFT 和 preference

Branch C: Online vs Offline
- [DEEP] Online DPO / iterative DPO
- On-policy vs off-policy 的 trade-off

Branch D: AI Feedback
- [MEDIUM] RLAIF (Lee et al., 2023) → \crossref{3}{2}
- Self-reward (Yuan et al., 2024)

**Step 4: Write §2.4 Convergence + §2.5 Open Problems**

**Step 5: Add BibTeX entries + Commit**

```
git add chapters/02-preference.tex references.bib
git commit -m "content: Thread 2 — Reward Modeling & Preference Optimization"
```

---

### Task 5: Chapter 03 — Thread 3: Safety & Alignment

**Files:**
- Create: `chapters/03-safety.tex`
- Update: `references.bib`

**Step 1: Write §3.1 Problem Origin**

Post-training 让模型变得有用，但也可能产生 harmful content。需要在"有用"和"安全"之间平衡。

**Step 2: Write §3.2 Foundational Approach**

[DEEP] Constitutional AI (Bai et al., 2022)
- 核心思想：用一组 principles (constitution) 让 AI 自我批评和修正
- RLAIF pipeline: critique → revision → preference data → RL
- 与 RLHF 的区别：减少人类标注依赖

**Step 3: Write §3.3 Limitations & Branching**

Branch A: Safe RLHF (Dai et al., 2024) — 将安全和有用分开建模
Branch B: Red-teaming — 主动发现模型弱点
Branch C: Refusal training — 学会拒绝 harmful requests
Branch D: Over-refusal problem — 拒绝过度导致无用

**Step 4: Write §3.4-3.5 + BibTeX + Commit**

```
git add chapters/03-safety.tex references.bib
git commit -m "content: Thread 3 — Safety & Alignment"
```

---

### Task 6: Chapter 04 — Thread 4: Reasoning & Verification

**Files:**
- Create: `chapters/04-reasoning.tex`
- Update: `references.bib`

**Step 1: Write §4.1 Problem Origin**

LLMs 在需要多步推理的任务上表现差。直接输出答案缺乏中间推理过程。

**Step 2: Write §4.2 Foundational Approach**

[DEEP] Chain-of-Thought prompting (Wei et al., 2022)
- Few-shot CoT 的效果和局限
- Zero-shot CoT ("Let's think step by step")

[DEEP] CoT Distillation — 将 CoT 能力从大模型蒸馏到小模型

**Step 3: Write §4.3 Limitations & Branching**

Branch A: Process vs Outcome Supervision
- [DEEP] PRM (Lightman et al., 2023) vs ORM
- Process supervision 的标注成本 vs 效果

Branch B: Self-Consistency & Verification
- [MEDIUM] Self-Consistency (Wang et al., 2023)
- Math-Shepherd, verification techniques

Branch C: RL for Reasoning (← \crossref{2}{3} RLHF techniques)
- [DEEP] OpenAI o1 (2024) — RL + long CoT + test-time compute scaling
- [DEEP] DeepSeek-R1 (2025) — GRPO, emergent reasoning via RL
  - GRPO 公式推导（与 PPO 的区别）
  - "Aha moment" 现象
  - R1-Zero vs R1 的对比
- [MEDIUM] Open-source reasoning models (Qwen, etc.)

**Step 4: Write §4.4-4.5 + BibTeX + Commit**

```
git add chapters/04-reasoning.tex references.bib
git commit -m "content: Thread 4 — Reasoning & Verification"
```

---

### Task 7: Chapter 05 — Thread 5: Multimodal Post-training

**Files:**
- Create: `chapters/05-multimodal.tex`
- Update: `references.bib`

**Step 1: Write §5.1 Problem Origin**

VLMs (Vision-Language Models) 同样需要 post-training 来对齐视觉和语言能力。直接将 LLM 的 post-training 方法套用到 VLM 上面临新的挑战。

**Step 2: Write §5.2 Foundational Approach**

[DEEP] LLaVA (Liu et al., 2023) — Visual Instruction Tuning
- 架构：visual encoder + projection + LLM
- 两阶段训练：alignment pre-training + instruction tuning
- 数据生成：GPT-4 生成 visual instruction data

**Step 3: Write §5.3 Limitations & Branching**

Branch A: VLM Preference Optimization (← \crossref{2}{3})
- [DEEP] LLaVA-RLHF, Silkie, RLHF-V
- VLM 场景下 reward modeling 的特殊挑战

Branch B: Hallucination Reduction
- [DEEP] Visual hallucination 的定义和评估
- POPE, CHAIR 等 benchmark
- 训练方法：negative sampling, contrastive learning

Branch C: Grounding & Fine-grained Understanding
- [MEDIUM] Grounding post-training, region-level alignment

Branch D: Multi-image & Video
- [MEDIUM] 扩展到多图和视频场景的 post-training

**Step 4: Write §5.4-5.5 + BibTeX + Commit**

```
git add chapters/05-multimodal.tex references.bib
git commit -m "content: Thread 5 — Multimodal Post-training"
```

---

### Task 8: Chapter 06 — Thread 6: Tool Use & Agentic Capabilities

**Files:**
- Create: `chapters/06-agentic.tex`
- Update: `references.bib`

**Step 1: Write §6.1 Problem Origin**

LLMs 的知识截止于训练数据，无法与外部世界交互。需要训练模型学会调用工具、执行代码、进行多步规划。

**Step 2: Write §6.2 Foundational Approach**

[DEEP] Toolformer (Schick et al., 2023) — 自监督学习工具使用
- 核心思想：让模型自己决定何时、如何调用 API
- 训练 pipeline：sampling → filtering → fine-tuning

**Step 3: Write §6.3 Limitations & Branching**

Branch A: Function Calling Training
- [MEDIUM] GPT function calling, Gorilla
- 结构化输出 + tool schema

Branch B: Agent Tuning (← \crossref{4}{2} reasoning enables planning)
- [DEEP] FireAct (Chen et al., 2023), AgentTuning
- ReAct 范式的 post-training

Branch C: Code Generation Alignment
- [MEDIUM] Code-specific RLHF, execution-based rewards

**Step 4: Write §6.4-6.5 + BibTeX + Commit**

```
git add chapters/06-agentic.tex references.bib
git commit -m "content: Thread 6 — Tool Use & Agentic Capabilities"
```

---

### Task 9: Chapter 07 — Thread 7: Efficiency & Data Engineering

**Files:**
- Create: `chapters/07-efficiency.tex`
- Update: `references.bib`

**Step 1: Write §7.1 Problem Origin**

Post-training 需要大量人类标注和计算资源。如何降低成本同时保持质量？

**Step 2: Write §7.2 Foundational Approaches**

[DEEP] LoRA/QLoRA for Alignment — parameter-efficient post-training
- LoRA 在 SFT 和 RLHF 中的应用
- QLoRA 进一步降低显存需求

[DEEP] Synthetic Data Generation
- 用强模型生成弱模型的训练数据
- 质量控制方法

**Step 3: Write §7.3 Limitations & Branching**

Branch A: Data Selection & Curation
- [MEDIUM] DEITA, data quality scoring
- Active learning for preference data

Branch B: Compute-Efficient Training
- [MEDIUM] Distillation for alignment
- Smaller models via post-training transfer

Branch C: Scaling Laws for Post-training
- [MEDIUM] 有限的研究 + open questions

**Step 4: Write §7.4-7.5 + BibTeX + Commit**

```
git add chapters/07-efficiency.tex references.bib
git commit -m "content: Thread 7 — Efficiency & Data Engineering"
```

---

### Task 10: Chapter 08 — Conclusion + Final Assembly

**Files:**
- Create: `chapters/08-conclusion.tex`
- Update: `main.tex` (final adjustments)

**Step 1: Write Conclusion**

1. 宏观趋势分析：
   - RL-based → RL-free (Thread 2 的演化)
   - Text-only → Multimodal (Thread 1 → Thread 5)
   - Human feedback → AI/Verifier feedback (Thread 2, 3, 4)
   - Single-turn → Agentic multi-turn (Thread 6)

2. Threads 之间的 meta-pattern：
   - 每个 thread 都经历了"人工 → 自动化"的演变
   - 每个 thread 都面临"quality vs cost"的 trade-off

3. 未来方向综合

**Step 2: Final compilation and review**

Run: `xelatex main.tex && bibtex main && xelatex main.tex && xelatex main.tex`
Verify: All cross-references resolve, bibliography renders, TikZ diagram displays

**Step 3: Final commit**

```
git add -A
git commit -m "content: conclusion and final assembly"
```

---

## Execution Notes

- **Research required**: Each task requires looking up actual paper details (methods, formulas, results). Use `context7` and `WebSearch` to verify paper content.
- **BibTeX**: Build `references.bib` incrementally with each chapter.
- **Cross-references**: Use `\crossref` macro consistently. Verify all links resolve in final compilation.
- **Diagram**: The TikZ overview diagram (Task 2) should be updated as new threads are written, to reflect the actual content.
