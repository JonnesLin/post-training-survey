# Related Work Findings from Paper PDFs — Complete Consolidated Analysis

## Status
- Thread 1 (SFT): ✅ Done (FLAN, Self-Instruct, LIMA, InstructGPT, Flan-v2, Orca)
- Thread 2 (Preference): ✅ Done (DPO, Christiano 2017, PRM, Llama-2)
- Thread 3 (Safety): ✅ Done (CAI, GCG, RepE)
- Thread 4 (Reasoning): ✅ Done (CoT, STaR, ToT, ReST)
- Thread 5 (Multimodal): ✅ Done (LLaVA, InstructBLIP, LLaVA-RLHF)
- Thread 6 (Agentic): ✅ Done (ReAct, Toolformer, Voyager, ToolLLM)
- Thread 7 (Efficiency): ✅ Done (LoRA, QLoRA, Zephyr, phi-1)

---

## MASTER PRIORITY TABLE — All Threads

### HIGH PRIORITY (Should definitely integrate)

| Thread | Paper | Why It Matters | Where to Add |
|--------|-------|----------------|--------------|
| T1 | **Vicuna** (Chiang et al. 2023) | Most popular open-source chatbot, ShareGPT data, contrasts with synthetic data | §1.2 alongside Alpaca |
| T1 | **Prompt tuning** (Li & Liang 2021; Lester et al. 2021) | Important contrast to instruction tuning; FLAN showed IT facilitates PT | §1.1 问题起源 |
| T1 | **Schick & Schütze 2021** | Direct precursor to synthetic data generation with LMs | Branch A intro |
| T2 | **Uesato et al. 2022** | Only direct comparison of process vs outcome supervision before PRM | §2 PRM subsection |
| T2 | **Cobbe et al. 2021 (GSM8K verifiers)** | Introduced ORM approach for math, direct precursor to PRM | §2 PRM subsection |
| T2 | **Ziegler et al. 2020** | Original "fine-tuning LMs from human preferences" — pre-Stiennon | §2.1 问题起源 |
| T3 | **Glaese et al. 2022 (Sparrow)** | DeepMind's rule-based aligned dialogue agent; parallel to CAI | §3 alongside CAI |
| T3 | **Burns et al. 2022 (CCS)** | Unsupervised truth discovery in LLM internals; foundational for RepE | §3 RepE subsection |
| T3 | **Shin et al. 2020 (AutoPrompt)** | Automated discrete prompt attacks; direct precursor to GCG | §3 GCG subsection |
| T3 | **Christiano et al. 2018 / Irving et al. 2018** | Scalable oversight theory (weak-to-strong, debate) — foundational for alignment | §3.1 理论基础 |
| T4 | **Least-to-Most Prompting** (Zhou et al. 2022) | Compositional reasoning decomposition — key prompting strategy | §4 CoT subsection |
| T4 | **RAP** (Hao et al. 2023) | MCTS-based reasoning — key alternative to ToT | §4 ToT subsection |
| T4 | **RAFT** (Dong et al. 2023) | Reward-ranked finetuning — bridges RL and SFT for reasoning | §4 ReST subsection |
| T4 | **PAL** (Gao et al. 2022) | Program-aided language models — code as reasoning tool | §4 reasoning strategies |
| T5 | **BLIP-2** (Li et al. 2023) | Foundation for InstructBLIP; Q-Former architecture | §5 InstructBLIP subsection |
| T5 | **PaLM-E** (Driess et al. 2023) | Embodied multimodal LLM | §5 multimodal architectures |
| T5 | **IDEFICS** (Laurencon et al. 2023) | Open-source Flamingo variant | §5 open-source multimodal |
| T5 | **Rohrbach et al. 2018** | Object hallucination in image captioning — foundational | §5 LLaVA-RLHF subsection |
| T5 | **MMBench** (Liu et al. 2023b) | Major multimodal evaluation benchmark | §5 evaluation |
| T6 | **SayCan** (Ahn et al. 2022) | Grounding LLMs in robotic affordances — cited by ReAct + Voyager | §6 embodied agents |
| T6 | **Inner Monologue** (Huang et al. 2022) | Closed-loop agent reasoning — direct precursor to ReAct | §6 ReAct subsection |
| T6 | **Generative Agents** (Park et al. 2023) | Landmark agent simulation work | §6 agent paradigms |
| T6 | **PAL** (Gao et al. 2022) | Program-aided LMs — tool use via code execution | §6 Toolformer subsection |
| T6 | **Codex** (Chen et al. 2021) | Foundation of code generation + tool use | §6 code agents |
| T6 | **Code as Policies** (Liang et al. 2023) | LLMs generating executable robot policies | §6 embodied agents |
| T7 | **Adapters** (Houlsby et al. 2019) | Original adapter method that LoRA improves upon | §7 LoRA subsection |
| T7 | **Prefix Tuning** (Li & Liang 2021) | Major PEFT comparator to LoRA | §7 PEFT landscape |
| T7 | **Mistral-7B** (Jiang et al. 2023) | Key base model for Zephyr and many efficiency works | §7 Zephyr subsection |
| T7 | **TinyStories** (Eldan & Li 2023) | Direct precursor to phi-1's data quality approach | §7 phi-1 subsection |
| T7 | **False Promise** (Gudibande et al. 2023) | Critical analysis of distillation limitations — cited by Zephyr + phi-1 | §7 distillation discussion |
| T7 | **Model Collapse** (Shumailov et al. 2023) | Critical risk for synthetic data paradigm | §7 open problems |
| T7 | **Chinchilla** (Hoffmann et al. 2022) | Compute-optimal scaling laws — foundational for efficiency | §7.1 问题起源 |

### MEDIUM PRIORITY (Valuable additions)

| Thread | Paper | Why It Matters |
|--------|-------|----------------|
| T1 | McCann et al. 2018 (DecaNLP) | QA-based task unification as conceptual precursor |
| T1 | Aghajanyan et al. 2021 (Muppet) | Multi-task pre-finetuning bridge |
| T2 | Wang et al. 2022 (Self-Consistency) | Major alternative to reward model verification |
| T2 | Thoppilan et al. 2022 (LaMDA) | Google's dialog alignment, parallel to InstructGPT |
| T2 | Lewkowycz et al. 2022 (Minerva) | Domain-specific fine-tuning for reasoning |
| T3 | Saunders et al. 2022 | Self-critiquing models; mechanism used by CAI |
| T3 | Carlini et al. 2023 | "Are aligned LMs robust?" — key empirical fragility result |
| T3 | Meng et al. 2023 (ROME/MEMIT) | Knowledge editing; related representation manipulation |
| T3 | Bowman et al. 2022 | Empirical progress on scalable alignment |
| T3 | Kadavath et al. 2022 | LLM calibration / self-knowledge |
| T4 | MATH dataset (Hendrycks et al. 2021) | Core reasoning benchmark |
| T4 | Zero-Shot CoT (Kojima et al. 2022) | "Let's think step by step" |
| T4 | Nye et al. 2021 (Scratchpads) | Precursor to CoT — intermediate computation |
| T5 | mPLUG-Owl (Ye et al. 2023) | Modular multimodal approach |
| T5 | PaLI/PaLI-X (Chen et al. 2022; 2023) | Major vision-language scaling work |
| T5 | OpenFlamingo (Awadalla et al. 2023) | Open-source Flamingo |
| T5 | Otter (Li et al. 2023b) | Multi-modal in-context instruction tuning |
| T6 | AutoGPT (Richards 2023) | Highly influential autonomous agent system |
| T6 | Creator (Qian et al. 2023) | LLMs creating their own tools |
| T6 | Tree of Thoughts (Yao et al. 2023) | Advanced reasoning for complex planning |
| T6 | LaMDA (Thoppilan et al. 2022) | Google's tool-using dialogue model |
| T7 | OASST1 (Kopf et al. 2023) | Key open-source RLHF dataset |
| T7 | AlpacaFarm (Dubois et al. 2023) | Simulation framework for RLHF methods |
| T7 | MT-Bench / Chatbot Arena (Zheng et al. 2023) | De facto evaluation standard |
| T7 | WizardCoder (Luo et al. 2023) | Evol-Instruct for code |
| T7 | Unnatural Instructions (Honovich et al. 2022) | Novel synthetic instruction approach |

### LOW PRIORITY (Brief mentions or cross-references)

| Thread | Paper | Notes |
|--------|-------|-------|
| T1 | Self-training vs Self-Instruct distinction | Clarify difference (task-agnostic vs task-specific) |
| T1 | Cross-ref: Flan-v2 CoT finetuning → Thread 4 | Strengthen cross-thread link |
| T2 | Shumailov et al. 2023 (Model collapse) | Risk when training on LLM data |
| T2 | Madaan et al. 2023 (Self-Refine) | Iterative self-improvement |
| T3 | Solaiman & Dennison 2021 (PALMS) | Values-targeted dataset adaptation |
| T3 | Xu et al. 2020 | Early safety recipes for chatbots |
| T3 | Rimsky 2023 | Activation steering for safety |
| T3 | Belinkov 2022 | Probing classifiers survey |
| T5 | Ji et al. 2023 (Hallucination survey) | Comprehensive hallucination taxonomy |
| T6 | BlenderBot, SimpleTOD, Sparrow | Conversational AI baselines |
| T6 | MineDojo, VPT, DreamerV3 | Minecraft-specific RL |
| T7 | COMPACTER (Mahabadi et al. 2021) | Kronecker product adapters |
| T7 | Aghajanyan et al. 2020 (intrinsic dim) | Theoretical motivation for LoRA |

---

## CROSS-THREAD CONNECTIONS DISCOVERED

1. **RLHF infrastructure shared across threads**: InstructGPT/PPO/Stiennon appears in Thread 2, 4 (ReST), 5 (LLaVA-RLHF), connecting preference optimization with reasoning and multimodal alignment.

2. **Self-training paradigm spans T1→T4→T7**: Self-Instruct (T1) → STaR/ReST (T4) → distilled alignment in Zephyr (T7) all share the bootstrap-from-self idea.

3. **Tool use connects T4 and T6**: PAL (program-aided reasoning) bridges reasoning strategies and tool-use paradigms. ToT reasoning also referenced by ToolLLM for complex planning.

4. **Embodied agents bridge T5 and T6**: PaLM-E, SayCan, Code as Policies connect multimodal perception with agentic capabilities.

5. **Data quality theme spans T1→T5→T7**: LIMA's "less is more" (T1) → multimodal data curation (T5) → phi-1's textbook quality (T7) → False Promise critique of imitation data.

6. **Scalable oversight theory (T3) motivates T2's AI feedback**: Christiano 2018, Irving 2018, Bowman 2022 provide theoretical foundations for why CAI/RLAIF (T2/T3) use AI feedback instead of human labels.

7. **Hallucination connects T4 and T5**: LLaVA-RLHF's multimodal hallucination reduction draws on both reasoning verification (T4 PRM) and safety alignment (T3 RLHF) techniques.

---

## Detailed Findings by Thread

### Thread 1: SFT (from FLAN, Self-Instruct, LIMA, InstructGPT, Flan-v2, Orca)

1. **QA-based task formulation as precursor to instruction tuning**
   - Kumar et al. 2016, McCann et al. 2018 — unifying NLP tasks as QA over context
   - **Relevance**: Could strengthen §1.1 问题起源

2. **Prompt tuning vs instruction tuning**
   - Li & Liang 2021 (prefix tuning), Lester et al. 2021 (prompt tuning)
   - FLAN explicitly showed instruction tuning facilitates prompt tuning
   - **Relevance**: Important contrast not currently discussed

3. **Multi-task pre-finetuning**
   - Aghajanyan et al. 2021 (Muppet)
   - **Relevance**: Bridge between MTL and instruction tuning

4. **LMs for data generation (pre-Self-Instruct)**
   - Schick & Schütze 2021 — generating labeled data with LMs
   - **Relevance**: Direct precursor to synthetic data paradigm

5. **Self-training vs Self-Instruct distinction**
   - Self-Instruct is task-agnostic bootstrap; self-training is task-specific pseudo-labeling

6. **Knowledge distillation framing**
   - Survey doesn't frame synthetic data through KD lens explicitly

7. **Vicuna** (Chiang et al. 2023)
   - Major missing reference — one of most popular open-source chatbots
   - Trained on ShareGPT conversations, contrasts synthetic data approaches

---

### Thread 2: Preference Optimization (from DPO, Christiano 2017, PRM, Llama-2)

**From DPO (Rafailov et al. 2023):**
- Dueling bandit framework (Yue et al. 2012) — theoretical basis for preference learning
- Preference-based RL literature (Wirth et al. 2017 survey, Sadigh et al. 2017, Novoseller et al. 2020)
- BPR/Bayesian approaches (Chu & Ghahramani 2005)

**From Christiano et al. 2017:**
- Foundational RLHF pipeline references
- Ziegler et al. 2020 — first application to NLP

**From PRM (Lightman et al. 2023):**
- **Uesato et al. 2022** — only prior direct comparison of process vs outcome supervision
- **Cobbe et al. 2021** — introduced ORM/verifier approach for math (GSM8K)
- Lewkowycz et al. 2022 (Minerva) — domain-specific finetuning for math reasoning

**From Llama-2 (Touvron et al. 2023):**
- Extensive safety annotation methodology
- Rejection sampling fine-tuning (RLHF variant)
- Model cascading for safety evaluation

**Top gaps for Thread 2:**
1. Uesato et al. 2022 (process vs outcome — most critical gap)
2. Cobbe et al. 2021 (GSM8K verifiers — direct PRM precursor)
3. Ziegler et al. 2020 (original RLHF for NLP)
4. Self-Consistency (Wang et al. 2022) — alternative to reward model verification

---

### Thread 3: Safety (from CAI, GCG, RepE)

**From CAI (Bai et al. 2022):**
- Glaese et al. 2022 (Sparrow) — DeepMind's rule-based aligned dialogue
- Saunders et al. 2022 — self-critiquing models
- Kadavath et al. 2022 — LLM calibration/self-knowledge
- Christiano et al. 2018 — scalable oversight theory (weak-to-strong)
- Irving et al. 2018 — AI safety via debate
- Bowman et al. 2022 — empirical scalable alignment

**From GCG (Zou et al. 2023):**
- Shin et al. 2020 (AutoPrompt) — automated discrete prompt attacks, direct precursor
- Carlini et al. 2023 — "Are aligned LMs robust?"
- Wallace et al. 2019 — universal adversarial triggers
- Ebrahimi et al. 2018 — character-level adversarial examples

**From RepE (Zou et al. 2023):**
- Burns et al. 2022 (CCS) — unsupervised truth discovery in internals
- Meng et al. 2023 (ROME/MEMIT) — knowledge editing
- Azaria & Mitchell 2023 — truthfulness classifiers on hidden layers
- Rimsky 2023 — activation steering for red-teaming
- Li et al. 2023b — Othello world model
- Belinkov 2022 — probing classifiers survey

**Top gaps for Thread 3:**
1. Glaese et al. 2022 (Sparrow) — most critical gap
2. Burns et al. 2022 (CCS) — foundational for RepE
3. Shin et al. 2020 (AutoPrompt) — direct GCG precursor
4. Christiano 2018 / Irving 2018 — scalable oversight foundations
5. Carlini et al. 2023 — aligned LM fragility

---

### Thread 4: Reasoning (from CoT, STaR, ToT, ReST)

**From CoT (Wei et al. 2022):**
- Ling et al. 2017 — math with natural language rationales
- Camburu et al. 2018 (e-SNLI) — NLI with explanations
- Nye et al. 2021 (Scratchpads) — intermediate computation steps
- Cobbe et al. 2021 (GSM8K) — training verifiers

**From STaR (Zelikman et al. 2022):**
- Bootstrapping and self-training literature
- Rationale-augmented training paradigm

**From ToT (Yao et al. 2023):**
- Least-to-Most Prompting (Zhou et al. 2022) — compositional decomposition
- RAP (Hao et al. 2023) — MCTS-based reasoning
- Selection-Inference (Creswell et al. 2022) — two-step reasoning

**From ReST (Gulcehre et al. 2023):**
- RAFT (Dong et al. 2023) — reward-ranked finetuning
- Online imitation learning literature (DAgger)
- Self-training with external reward signals

**Top gaps for Thread 4:**
1. Least-to-Most Prompting (Zhou et al. 2022)
2. RAP / MCTS reasoning (Hao et al. 2023)
3. RAFT (Dong et al. 2023)
4. PAL (Gao et al. 2022) — program-aided LMs
5. MATH dataset (Hendrycks et al. 2021)

---

### Thread 5: Multimodal (from LLaVA, InstructBLIP, LLaVA-RLHF)

**From LLaVA (Liu et al. 2023):**
- GPT-4 for visual instruction data generation
- Visual chat paradigm
- Connection to text-only instruction tuning

**From InstructBLIP (Dai et al. 2023):**
- **BLIP-2** (Li et al. 2023) — Q-Former architecture, foundation
- BLIP (Li et al. 2022) — predecessor
- mPLUG-Owl (Ye et al. 2023) — modular approach
- MultiInstruct (Xu et al. 2022) — multi-modal instruction tuning

**From LLaVA-RLHF (Sun et al. 2023):**
- **IDEFICS** (Laurencon et al. 2023) — open-source Flamingo
- **PaLI/PaLI-X** (Chen et al. 2022; 2023) — vision-language scaling
- **Rohrbach et al. 2018** — object hallucination in image captioning
- **Otter** (Li et al. 2023b) — multi-modal in-context instruction
- **PaLM-E** (Driess et al. 2023) — embodied multimodal
- MMBench (Liu et al. 2023b) — evaluation benchmark
- Ji et al. 2023 — hallucination survey
- Kadavath et al. 2022 — model calibration

**Top gaps for Thread 5:**
1. BLIP-2 (Li et al. 2023) — foundation for InstructBLIP
2. PaLM-E (Driess et al. 2023) — embodied multimodal
3. IDEFICS (Laurencon et al. 2023) — open-source multimodal
4. Rohrbach et al. 2018 — hallucination measurement
5. PaLI/PaLI-X (Chen et al. 2022; 2023) — scaling work
6. MMBench (Liu et al. 2023b) — evaluation
7. mPLUG-Owl (Ye et al. 2023) — modular design

---

### Thread 6: Agentic (from ReAct, Toolformer, Voyager, ToolLLM)

**From ReAct (Yao et al. 2022):**
- SayCan (Ahn et al. 2022) — grounding in robotic affordances
- Inner Monologue (Huang et al. 2022) — embodied reasoning
- Sparrow (Glaese et al. 2022) — dialogue agent with rules

**From Toolformer (Schick et al. 2023):**
- PAL (Gao et al. 2022) — program-aided language models
- LaMDA (Thoppilan et al. 2022) — tool-using dialogue
- Cobbe et al. 2021 — training verifiers for math
- REALM/RETRO — retrieval-augmented approaches

**From Voyager (Wang et al. 2023):**
- Generative Agents (Park et al. 2023) — agent simulation
- AutoGPT (Richards 2023) — autonomous agent
- Code as Policies (Liang et al. 2023) — executable robot code
- Codex (Chen et al. 2021) — code generation
- MineDojo (Fan et al. 2022) — Minecraft benchmark

**From ToolLLM (Qin et al. 2023):**
- Tree of Thoughts (Yao et al. 2023) — advanced reasoning
- Creator (Qian et al. 2023) — LLMs creating tools
- RestGPT (Song et al. 2023) — REST API integration
- WebCPM (Qin et al. 2023a) — interactive web search

**Top gaps for Thread 6:**
1. SayCan (Ahn et al. 2022) — cited by ReAct + Voyager
2. Inner Monologue (Huang et al. 2022) — cited by ReAct + Voyager
3. Generative Agents (Park et al. 2023) — landmark agent simulation
4. PAL (Gao et al. 2022) — bridges reasoning and tool use
5. Codex (Chen et al. 2021) — code generation foundation
6. Code as Policies (Liang et al. 2023) — executable policies

---

### Thread 7: Efficiency (from LoRA, QLoRA, Zephyr, phi-1)

**From LoRA (Hu et al. 2021):**
- Adapters (Houlsby et al. 2019) — original adapter method
- Prefix Tuning (Li & Liang 2021) — PEFT comparator
- Prompt Tuning (Lester et al. 2021) — PEFT comparator
- Aghajanyan et al. 2020 — intrinsic dimensionality (theoretical motivation)

**From QLoRA (Dettmers et al. 2023):**
- OASST1 (Kopf et al. 2023) — open-source RLHF dataset
- Unnatural Instructions (Honovich et al. 2022) — synthetic instructions
- Dataset comparison methodology

**From Zephyr (Tunstall et al. 2023):**
- Mistral-7B (Jiang et al. 2023) — base model
- False Promise (Gudibande et al. 2023) — critique of distillation
- AlpacaFarm (Dubois et al. 2023) — simulation framework
- MT-Bench / Chatbot Arena (Zheng et al. 2023) — evaluation

**From phi-1 (Gunasekar et al. 2023):**
- TinyStories (Eldan & Li 2023) — precursor data quality work
- Chinchilla (Hoffmann et al. 2022) — compute-optimal scaling
- Model Collapse (Shumailov et al. 2023) — synthetic data risk
- WizardCoder (Luo et al. 2023) — Evol-Instruct for code
- StarCoder (Li et al. 2023) — open code model

**Top gaps for Thread 7:**
1. Adapters (Houlsby et al. 2019) — LoRA's predecessor
2. Prefix Tuning (Li & Liang 2021) — major PEFT alternative
3. Mistral-7B (Jiang et al. 2023) — Zephyr's base model
4. TinyStories (Eldan & Li 2023) — phi-1 precursor
5. False Promise (Gudibande et al. 2023) — distillation critique
6. Model Collapse (Shumailov et al. 2023) — synthetic data risk
7. Chinchilla (Hoffmann et al. 2022) — compute-optimal scaling
