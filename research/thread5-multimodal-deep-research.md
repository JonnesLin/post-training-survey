# Thread 5: Multimodal Post-Training — Deep Research Report (2025-03-30)

## Executive Summary

5 parallel research agents searched 12 directions, returning ~138 candidate papers.
After deduplication (cross-agent + against existing chapter's ~100 cite keys),
**~95 new papers** are identified for potential integration.

The chapter currently covers 43 papers in the paper list + ~40 additional papers in the frontier sections.
This research report proposes expanding coverage with **~50 high-priority papers** across
6 new subsections and 4 deepened existing sections.

---

## Recommended Chapter Structure Changes

### New Subsections to Add (in 收敛与前沿)

| # | Proposed Subsection | Papers | Justification |
|---|---------------------|--------|---------------|
| **NEW-1** | 2025: Multimodal Data Synthesis & Intelligent Curation | 6-8 | Currently only ShareGPT4V and Molmo mentioned. A systematic treatment of data pipelines (synthesis, selection, mixing, collapse) is a critical missing piece. |
| **NEW-2** | 2025: VLM Distillation & Efficient Deployment | 5-7 | Completely absent from the chapter. SmolVLM (256M outperforms 80B), FastVLM (Apple), MoVE-KD, CASP represent a major practical direction. |
| **NEW-3** | 2025: Medical & Scientific Domain VLM Post-Training | 5-6 | The "R1 wave" in medical VLM is a significant phenomenon (2B models outperforming 72B via GRPO). Domain-specific RL is only covered for GUI/charts/documents. |
| **NEW-4** | 2025: Generation Alignment — RLHF/DPO for Diffusion & Video | 6-8 | Currently the chapter discusses unified generation architectures but NOT how to align generation quality. This is a fast-growing field with its own DPO variants. |
| **NEW-5** | 2025--2026: Multimodal RAG | 4-5 | Entirely missing. VisRAG (ICLR 2025), MMGraphRAG, adaptive retrieval gating represent a new interaction paradigm for VLMs. |
| **NEW-6** | 2025--2026: Multimodal Continual Learning | 3-4 | Missing. MLLM-CL and VLM-CL survey identify VLM-specific challenges (cross-modal drift, zero-shot erosion). |

### Existing Sections to Deepen

| Section | Current State | Proposed Additions |
|---------|---------------|-------------------|
| **Multimodal Reasoning via RL** (§5.4.1) | R1-VL, MM-Eureka, VLM-R1, URSA, VisualPRM, MVoT | +Perception-R1 (visual perception reward), VL-Rethinker (vanishing advantages), OpenVLThinker (iterative SFT-RL), Virgo (text-only thought transfers), PEARL (perception checklist), MiMo-VL, GPRO (gated reasoning) |
| **Multimodal Safety** (Open Problem 3) | Only 4 papers (Qi et al., FigStep, MM-SafetyBench, VLGuard) | +OmniGuard, GuardReasoner-Omni (GRPO for guardrails), Safe RLHF-V (NeurIPS 2025), COMET (94% ASR), JRS-Rem (representation-level defense), ShiftDC, Omni-SafetyBench |
| **VLM Preference Optimization** (Branch A) | LLaVA-RLHF, mDPO, RLAIF-V, Silkie, HA-DPO, SeVa | +MoD-DPO (omni-modal decoupled), DA-DPO (difficulty-aware), SymMPO, MISP-DPO (multi-negative), OmniDPO, UnifiedReward-Think (NeurIPS 2025), Generative RLHF-V |
| **Long Video Understanding** (Open Problem 2) | Video-XL, VideoLLaMA3, MLVU, OmniLive | +Eagle 2.5 (NeurIPS 2025), Long-VITA (1M tokens), StreamingVLM (infinite streams), Temporal CoT (NeurIPS 2025), MARC (ICLR 2026), PVC (CVPR 2025) |

### Open Problems to Update/Add

| # | Problem | Status |
|---|---------|--------|
| OP2 (Long Video) | **Deepen** — add Eagle 2.5, StreamingVLM, TCoT as partial solutions |
| OP3 (Omni-Modal Safety) | **Major expansion** — promote from open problem to full subsection with attack/defense taxonomy |
| OP5 (Unified Generation Post-Training) | **Deepen** — now has concrete work: RecA, SeGroS, GRPO for interleaved generation |
| **NEW OP7** | **VLM Self-Improvement Without Human Labels** — Vision-Zero, Vision-R1, Vision-SR1, Perception-R1 establish a new paradigm |
| **NEW OP8** | **Multimodal Reward Hacking & Robustness** — DDRL, Diffusion-SDPO expose fundamental pathologies |
| **NEW OP9** | **Cross-Domain Transfer of RL Post-Training** — Medical R1 wave shows 2B models beating 72B, but transfer mechanisms unclear |

---

## Direction-by-Direction Findings

### Direction 1: Multimodal Reasoning via RL — Updates to §5.4.1

**Already in chapter**: R1-VL, MM-Eureka, VLM-R1, MM-RLHF, TLDR, URSA, VisualPRM, MVoT, o3/o4-mini

**New papers (sorted by importance):**

| Paper | Importance | arXiv | Key Contribution |
|-------|-----------|-------|-----------------|
| **Perception-R1** | IMPORTANT | 2506.07218 | Visual perception reward — explicitly rewards accurate visual understanding, not just reasoning. SOTA with 1,442 samples. Addresses "thinking over seeing" bias. |
| **VL-Rethinker** | IMPORTANT | 2504.08837 | Identifies "vanishing advantages" failure mode in VLM GRPO. Selective Sample Replay + Forced Rethinking. MathVista 80.4%. |
| **OpenVLThinker** | IMPORTANT | 2503.17352 | Iterative SFT-RL cycling amplifies reasoning beyond single-round RL. First open-source demonstration. |
| **Virgo** | IMPORTANT | 2501.01904 | Text-only long-form thought data transfers slow-thinking to VLMs. Slow-thinking is fundamentally language-model-driven. |
| **R1-Onevision** | IMPORTANT | 2503.10615 | ICCV 2025. Cross-modal formalization — transforms images to formal text for precise reasoning. Surpasses GPT-4o. |
| **PEARL** | IMPORTANT | 2511.18437 | Perception checklist anchors reasoning to verified visual evidence. +9.7% over baseline. |
| **MiMo-VL** | IMPORTANT | 2506.03569 | Xiaomi. 4-stage pretraining + Mixed On-policy RL. SOTA GUI grounding. Shows on-policy RL critical for stability. |
| **GPRO** | IMPORTANT | 2601.04442 | Meta-reasoning controller routing among 3 decision paths. Learns when NOT to reason. Reduces overthinking. |
| **Mirage** | MINOR | 2506.17218 | Latent visual tokens for "mental imagery" during reasoning — internal visualization without pixel generation. |
| **VisionThink** | MINOR | 2507.13348 | RL controls visual input resolution itself — dynamic token compression via RL. |
| **ReVisual-R1** | MINOR | 2506.04207 | Staged RL (text→multimodal→text) fixes gradient stagnation. |
| **R1-Reward** | IMPORTANT | 2505.02835 | First to train reward models themselves via RL. +8.4% VL-RewardBench, +14.3% MM-RewardBench. |
| **VRPRM** | MINOR | 2508.03556 | Data-efficient PRM: 3.6K CoT + 50K non-CoT surpasses 400K-sample PRMs. |

**Evolution chain for §5.4.1 expansion:**
```
Rule-based GRPO (R1-VL, MM-Eureka)
→ Process-level reward (URSA PS-GRPO, VisualPRM)
→ Perception-aware reward (Perception-R1, PEARL)
→ Self-rewarding (Vision-SR1, Vision-R1)
→ Meta-reasoning / when-to-think (GPRO, VisionThink)
```

---

### Direction 2: Generation Alignment (RLHF/DPO for Diffusion/Video) — **NEW §5.4.X**

**Currently absent from chapter.** This is a parallel track to VLM understanding alignment.

| Paper | Importance | arXiv | Key Contribution |
|-------|-----------|-------|-----------------|
| **DSPO** | LANDMARK | ICLR 2025 Oral | Direct Score Preference Optimization — aligns pretraining and fine-tuning objectives for diffusion. Theoretical foundation. |
| **VideoReward / Flow-DPO** | IMPORTANT | 2501.13918 | First systematic RLHF framework for flow-based video generation. Multi-dimensional reward. |
| **Diffusion-SDPO** | IMPORTANT | 2511.03317 | Fixes fundamental flaw in Diffusion-DPO where both preferred and dispreferred degrade. |
| **DDRL** | IMPORTANT | 2512.04332 | Data-regularized RL prevents reward hacking in diffusion via forward KL anchoring. |
| **Focus-N-Fix** | IMPORTANT | CVPR 2025 | Google. Region-aware fine-tuning — corrects only problematic regions. |
| **RecA** | IMPORTANT | 2509.07295 | Architecture-agnostic alignment for unified multimodal models. GenEval 0.73→0.90 in 27 GPU-hours. |
| **Unified Interleaved GRPO** | IMPORTANT | 2603.09538 | First GRPO application to multimodal generation (not just understanding). |
| **VisionReward** | MINOR | 2412.21059 | Unified image+video reward model with interpretable dimensions. |
| **SUDO** | MINOR | 2504.14534 | Self-supervised DPO — eliminates preference data collection entirely for diffusion. |
| **Curriculum-DPO++** | MINOR | 2602.13055 | Joint model-capacity and data-difficulty scheduling. |
| **PG-DPO** | MINOR | 2511.19049 | First to diagnose likelihood displacement in video diffusion DPO. |
| **PRFL** | MINOR | 2511.21541 | Video generators as their own latent-space reward models. |
| **Diffusion-DRF** | MINOR | 2601.04153 | VLMs as critics for diffusion, no trained reward model needed. |
| **SeGroS** | MINOR | 2603.19807 | Semantically-grounded supervision for unified model alignment. Improves upon RecA. |

**Proposed evolution chain:**
```
Text-only RLHF/DPO (Thread 2)
→ Diffusion-DPO (adapt DPO to denoising process)
→ Score-matching alignment (DSPO, ICLR 2025)
→ Pathology fixes (Diffusion-SDPO, PG-DPO: likelihood displacement/winner degradation)
→ Video generation alignment (Flow-DPO, PRFL)
→ Unified model alignment (RecA, SeGroS, Interleaved GRPO)
```

---

### Direction 3: Multimodal Data Synthesis & Curation — **NEW §5.4.X**

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **SAIL-VL** | IMPORTANT | ACL 2025 | 655B-token pipeline with logarithmic scaling laws. SAIL-Caption is highest-quality open-source caption dataset. |
| **DataProphet** | IMPORTANT | ICLR 2026 | Training-free metric predicts which supervision data generalizes best. 86% Kendall's tau with actual rankings. |
| **MoDoMoDo** | IMPORTANT | 2505.24871 | First systematic study of data mixing for multimodal RL post-training. +5.24% OOD over uniform mixing. |
| **Oasis** | IMPORTANT | ICCV 2025 | Synthesizes instruction data from images alone — no paired metadata needed. 500K+ data points. |
| **PreSel** | IMPORTANT | CVPR 2025 Highlight | Inverts pipeline: select images first, generate instructions later. Cuts cost while maintaining quality. |
| **Model Collapse in Multimodal** | MINOR | ICMI 2025 | First study of model collapse in VLM/diffusion. Frozen-model relabeling slows collapse. |
| **Instructify** | MINOR | 2505.18115 | Open unified framework converting metadata to VisIT using open LLMs. Removes GPT-4 dependency. |
| **L2T** | MINOR | 2503.22215 | Loss on instruction + response sequences. +9% relative improvement with zero additional data. |

**Proposed evolution chain:**
```
GPT-4 generated data (LLaVA, ShareGPT4V)
→ Human-only data (Molmo PixMo)
→ Image-only synthesis (Oasis)
→ Intelligent selection before generation (PreSel)
→ Scaling laws & mixture optimization (SAIL-VL, MoDoMoDo, DataProphet)
→ Collapse awareness (Model Collapse study)
```

---

### Direction 4: VLM Distillation & Efficient Deployment — **NEW §5.4.X**

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **SmolVLM** | IMPORTANT | 2504.05299 | HuggingFace. 256M model outperforms Idefics-80B (300x larger). <1GB VRAM, true edge deployment. |
| **FastVLM** | IMPORTANT | CVPR 2025 | Apple. FastViTHD encoder: 85x faster time-to-first-token, 3.4x smaller than LLaVA-OneVision. |
| **ACED** | IMPORTANT | CVPR 2025 | Active data curation + explicit distillation. Shows curation is complementary to KD. |
| **MoVE-KD** | IMPORTANT | CVPR 2025 | Distills multiple encoder capabilities (CLIP, EVA-02, ConvNeXt) into one via LoRA + MoE routing. |
| **CASP** | IMPORTANT | CVPR 2025 Highlight | Attention-sparsity-based compression. +21% avg improvement on 2-bit quantization for VLMs. |
| **LAid** | MINOR | AAAI 2026 | Distills long-range attention. 3.2x longer effective context in small models. |
| **EPIC** | MINOR | 2510.00515 | Progressive consistency distillation for aggressive visual token reduction. |

**Proposed evolution chain:**
```
Full fine-tuning of large VLMs
→ LoRA/QLoRA adaptation (Thread 7)
→ Multi-teacher/multi-encoder distillation (MoVE-KD, ACED)
→ Architecture-aware compression (CASP attention sparsity, FastVLM hybrid encoder)
→ Ultra-small deployment (SmolVLM 256M, edge VLMs)
```

---

### Direction 5: Long-Context Multimodal — Updates to Open Problem 2

**Already in chapter**: Video-XL, VideoLLaMA3, MLVU, OmniLive

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **Eagle 2.5** | IMPORTANT | NeurIPS 2025 | NVIDIA. 8B model matches GPT-4o on Video-MME (72.4%). Automatic Degrade Sampling for long-context post-training. |
| **Long-VITA** | IMPORTANT | 2502.05177 | First open model processing 1M tokens (4K+ frames) across image/video/text simultaneously. |
| **Temporal Chain of Thought** | IMPORTANT | NeurIPS 2025 | Google. Training-free: 32K context with TCoT outperforms same VLM at 700K on 1-hour videos. |
| **StreamingVLM** | IMPORTANT | 2510.09608 | MIT Han Lab. Infinite-stream video, 8 FPS on single H100, stable memory. |
| **MARC** | IMPORTANT | ICLR 2026 | RL-based token compression. 95% token reduction at near-zero accuracy cost. |
| **PVC** | MINOR | CVPR 2025 | Progressive Visual Token Compression. Unifies image/video with 64 tokens/frame. |
| **BOLT** | MINOR | CVPR 2025 | Training-free frame selection via inverse transform sampling. |
| **VideoScan** | MINOR | 2503.09387 | 1 semantic token/frame, memory independent of video length. |

**Key insight**: Clear bifurcation — (a) scale raw context to 1M+ (Long-VITA) vs (b) smarter frame/token selection (TCoT, MARC). Streaming (StreamingVLM) is emerging as its own subfield.

---

### Direction 6: Medical & Scientific Domain VLM Post-Training — **NEW §5.4.X**

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **MedVLM-R1** | LANDMARK | 2502.19634 | 2B model + 600 MRI samples outperforms Qwen2-VL-72B via GRPO. 35% OOD improvement. |
| **Med-R1** | IMPORTANT | 2503.13939 | 8 imaging modalities, 5 diagnostic tasks. Surprising: omitting rationales improves generalization. |
| **BioReason** | IMPORTANT | NeurIPS 2025 | DNA foundation model + LLM + GRPO. KEGG pathway prediction 86%→98%. |
| **Patho-R1** | IMPORTANT | 2505.11404 | Pathology-specific 3-stage pipeline with diagnostic reasoning from pathologist paradigms. |
| **RadFlow** | IMPORTANT | 2511.10065 | First RL for long-form radiology reports. Hierarchical reward modeling clinical workflow. |
| **PathVLM-R1** | MINOR | 2504.09258 | Process supervision for pathology + strong cross-domain transfer. |
| **ChemVLM** | MINOR | AAAI 2025 | First open-source chemistry VLM with domain-specific encoder. |
| **GeoVLM-R1** | MINOR | 2509.25026 | First RL-based reasoning for remote sensing. |
| **Lingshu** | MINOR | 2506.07044 | Four-stage "shallow-to-deep" medical post-training + MedEvalKit. |

**The "R1 Wave" pattern**: GRPO applied to medical/scientific VLMs produces disproportionate gains:
- 2B models routinely outperform 72B models
- Minimal data required (600 samples for MedVLM-R1)
- Strong cross-domain transfer (pathology→CT, dermoscopy, fundus)
- Well-defined correctness signals make RL especially effective

**Proposed evolution chain:**
```
General SFT on medical data (LLaVA-Med, etc.)
→ Domain-specific SFT + evaluation (ChemVLM, Lingshu)
→ GRPO with rule-based rewards (MedVLM-R1, Med-R1)
→ Hierarchical/process rewards (RadFlow, PathVLM-R1)
→ Cross-domain reasoning (BioReason: genomics + language)
```

---

### Direction 7: Multimodal RAG — **NEW §5.4.X**

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **VisRAG** | IMPORTANT | ICLR 2025 | VLM-based RAG bypassing text parsing. Documents as images for retrieval. 20-40% gain over text RAG. |
| **MMGraphRAG** | IMPORTANT | 2507.20804 | Fuses visual scene graphs with text KGs. 76.8% DocBench vs 52.3% GraphRAG. |
| **MMA-RAG** | IMPORTANT | 2603.00511 | Adaptive retrieval gating based on internal model confidence. |
| **MRAG Survey** | MINOR | ACL 2025 Findings | First comprehensive survey of multimodal RAG. |
| **M4-RAG** | MINOR | 2512.05959 | 80K+ samples, 42 languages. Reveals "retrieval hurts large models" phenomenon. |

---

### Direction 8: Multimodal Safety — Expand Open Problem 3 → Full Subsection

**Already in chapter**: Qi et al., FigStep, MM-SafetyBench, VLGuard (4 papers)

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **OmniGuard** | IMPORTANT | 2512.02306 | First omni-modal guardrail (text+image+video+audio) with deliberate reasoning. 210K training samples. |
| **GuardReasoner-Omni** | IMPORTANT | 2602.03328 | 2B guardrail trained with GRPO. First reasoning-based video moderation with temporal understanding. |
| **Safe RLHF-V** | IMPORTANT | NeurIPS 2025 | First multimodal safety RLHF framework. BeaverTails-V dataset. +34.2% safety + 34.3% helpfulness simultaneously. |
| **COMET** | IMPORTANT | 2602.10148 | 94%+ ASR across 9 VLMs. Exploits reasoning capabilities as attack surface. |
| **JRS-Rem** | IMPORTANT | 2603.17372 | Representation-level defense: jailbreaks form separable internal states. |
| **ShiftDC** | MINOR | 2502.13095 | Safety perception distortion — multimodal inputs shift activations toward "safer" direction paradoxically. |
| **BlueSuffix** | MINOR | ICLR 2025 | RL-trained defensive suffix generator for VLMs. Black-box. |
| **Omni-SafetyBench** | MINOR | 2508.07173 | First parallel safety benchmark for omni-modal LLMs. 24 modality variations. |

**Proposed evolution chain:**
```
Image-level attacks (adversarial perturbation → Qi et al. 2023)
→ Typographic attacks (FigStep 2023)
→ Reasoning-exploiting attacks (COMET 2026: 94% ASR)
→ Binary safety classifiers (VLGuard, MM-SafetyBench)
→ Representation-level defense (JRS-Rem, ShiftDC)
→ Reasoning-based omni-modal guardrails (OmniGuard, GuardReasoner-Omni)
→ Constrained optimization (Safe RLHF-V: Lagrangian helpfulness-safety)
```

---

### Direction 9: Native Multimodal Pre-Training — Updates to §5.4.3 (Unified Generation)

**Already in chapter**: Chameleon, Emu3, Show-o, Transfusion, Janus/Janus-Pro

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **Scaling Laws for Native Multimodal Models** | IMPORTANT | 2504.07951 | Apple. 457-model study. Early fusion stronger at lower params. First rigorous scaling laws. |
| **ERNIE 5.0** | IMPORTANT | 2602.04705 | Baidu. 2.4T params. Next-Group-of-Tokens Prediction (NGTP) generalizes next-token prediction across modalities. |
| **FuseLIP** | MINOR | 2506.03096 | Early fusion for embeddings/retrieval. Outperforms CLIP-style late fusion. |
| **Unified Models Survey** | MINOR | 2505.02567 | Taxonomy by sequence representation: pixel/token/multiple-tokens. |
| **Discrete Tokenization Survey** | MINOR | 2507.22920 | First dedicated survey on tokenization infrastructure for native multimodal models. |

---

### Direction 10: VLM Evaluation — Update Paper List

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **MMMU-Pro** | IMPORTANT | ACL 2025 | Filters text-only-solvable items. Performance drops to 16.8-26.9%. |
| **Video-MMMU** | IMPORTANT | 2501.13826 | First benchmark treating videos as knowledge source. Delta-knowledge metric. |
| **VLM-RobustBench** | IMPORTANT | 2603.06148 | 49 augmentation types. VLMs "semantically strong but spatially fragile." |
| **VidHalluc** | MINOR | CVPR 2025 | 5,002 videos for temporal hallucination evaluation. |
| **ODE** | MINOR | CVPR 2025 | Dynamic graph-based hallucination evaluation. Solves stale benchmark problem. |
| **GEOBench-VLM** | MINOR | ICCV 2025 | 31 geospatial tasks. Best VLM only 41.7% accuracy. |
| **MINDCUBE** | MINOR | 2506.21458 | Spatial mental models from limited views. Li Fei-Fei lab. |

---

### Direction 11: Emerging Directions — Self-Improvement & Continual Learning

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **Vision-Zero** | IMPORTANT | 2509.25541 | Gamified self-play ("Who Is the Spy") for VLM self-improvement. Zero human labels. |
| **Vision-R1** | IMPORTANT | 2503.18013 | Vision-guided RL without reward models or preference datasets. +50% improvement on 7B models. |
| **Vision-SR1** | IMPORTANT | 2508.19652 | Self-rewarding via reasoning decomposition into perception + reasoning. |
| **Argos** | IMPORTANT | 2512.03438 | Microsoft. Agentic reward verifier for multimodal agent RL. Shows SFT alone causes ungrounded collapse. |
| **MLLM-CL** | MINOR | 2506.05453 | First benchmark distinguishing domain CL from ability CL in VLMs. |
| **VLM-CL Survey** | MINOR | 2508.04227 | First survey on VLM continual learning. Cross-modal drift, zero-shot erosion. |

---

### Direction 12: Multimodal Preference Learning — Updates to Branch A

**Already in chapter**: LLaVA-RLHF, mDPO, RLAIF-V, Silkie, HA-DPO, SeVa, DRESS, LLaVA-Critic

| Paper | Importance | arXiv/Venue | Key Contribution |
|-------|-----------|-------------|-----------------|
| **UnifiedReward-Think** | IMPORTANT | NeurIPS 2025 | CoT-based multi-dimensional reward. First unified understanding+generation reward model. |
| **Generative RLHF-V** | IMPORTANT | 2505.18531 | Generative reward models learn principles from preferences. +18.1% vs 5.3% baseline. |
| **MoD-DPO** | IMPORTANT | 2603.03192 | Modality-decoupled preference for omni-modal. First to tackle cross-modal hallucination in omni LLMs. |
| **DA-DPO** | MINOR | 2601.00623 | Difficulty-aware reweighting of preference pairs. Curriculum-like approach. |
| **SymMPO** | MINOR | 2506.11712 | Theoretically grounded symmetric DPO extension for multimodal. |
| **MISP-DPO** | MINOR | 2509.25717 | Multi-negative DPO via Plackett-Luce model. Richer negative signal from diverse images. |
| **OmniDPO** | MINOR | 2509.00723 | First DPO for omni-modal (text+video+audio) hallucination. |

---

## Cross-Cutting Themes

### Theme 1: The "R1 Wave" — GRPO Everywhere
GRPO has become the universal RL algorithm for multimodal post-training in 2025. Applied to:
- General VLM reasoning (R1-VL, MM-Eureka, VLM-R1)
- Medical imaging (MedVLM-R1, Med-R1, Patho-R1)
- Remote sensing (GeoVLM-R1)
- 3D spatial reasoning (3D-R1, Smooth Operator)
- GUI agents (UI-TARS-2)
- Document navigation (SCoPE VLM)
- Safety guardrails (GuardReasoner-Omni)
- Reward model training (R1-Reward)
- Multimodal generation (Interleaved GRPO)

The key innovations are NOT in the RL algorithm itself (GRPO remains largely unchanged)
but in **reward design** (perception rewards, process rewards, self-rewards, domain-specific rewards)
and **training stability** (staged RL, selective replay, on-policy updates, gated reasoning).

### Theme 2: Self-Improvement Without Human Labels
A new paradigm is emerging where VLMs train themselves:
- Vision-Zero: gamified self-play
- Vision-R1: vision-intrinsic reward
- Vision-SR1: self-rewarding via decomposition
- Perception-R1: visual perception as reward
- DPE/VisPlay: self-generated training data
- SUDO: self-supervised preference pairs for diffusion

This points toward a future where post-training requires zero human annotation.

### Theme 3: The Perception Bottleneck
Multiple independent findings converge on: **VLM failures are primarily perceptual, not reasoning.**
- VL-RewardBench: judge failures concentrate on visual perception, not logic
- Test-time scaling analysis: gains only on reasoning tasks, not perception tasks
- Perception-R1: standard RLVR improves reasoning but NOT perception
- PEARL: reasoning chains built on hallucinated visual evidence
- VLM-RobustBench: low-severity spatial perturbations hurt more than severe photometric ones

This suggests the next breakthrough in VLM post-training must come from
**improving visual perception during/after training**, not just improving reasoning.

### Theme 4: Alignment for Generation ≠ Alignment for Understanding
The chapter already notes this tension. New work confirms and deepens it:
- RecA/SeGroS: reconstruction-based alignment for unified models
- Diffusion-DPO variants expose fundamentally different pathologies than text DPO
- VisionReward: multi-dimensional reward needed (aesthetic, factual, semantic)
- Unified Interleaved GRPO: first attempt to bridge both

---

## Priority Integration Recommendation

**Phase 1 (highest priority — fills critical gaps):**
1. NEW subsection: Generation Alignment (DSPO, Flow-DPO, RecA) — 6-8 papers
2. NEW subsection: Medical/Scientific Domain VLM RL — 5-6 papers
3. Deepen: Multimodal Reasoning via RL (Perception-R1, VL-Rethinker, Virgo) — 5-7 papers
4. Expand: Multimodal Safety → full subsection (OmniGuard, Safe RLHF-V, COMET) — 6-8 papers

**Phase 2 (high priority — new directions):**
5. NEW subsection: Data Synthesis & Curation (SAIL-VL, DataProphet, PreSel) — 5-6 papers
6. NEW subsection: VLM Distillation (SmolVLM, FastVLM, MoVE-KD) — 5-7 papers
7. Deepen: Long Video (Eagle 2.5, StreamingVLM, TCoT) — 4-5 papers

**Phase 3 (important but smaller):**
8. NEW subsection: Multimodal RAG (VisRAG, MMGraphRAG) — 3-4 papers
9. Deepen: Preference Learning (UnifiedReward-Think, MoD-DPO) — 4-5 papers
10. Update: Open Problems (add OP7 self-improvement, OP8 reward hacking, OP9 cross-domain transfer)

**Estimated total**: ~50 new papers integrated, chapter grows from ~1800 lines to ~2800-3000 lines.

---

## Key Surveys for Reference

| Survey | arXiv | Coverage |
|--------|-------|---------|
| Reinforced MLLM: RL-Based Reasoning in MLLMs | 2504.21277 | Comprehensive categorization of multimodal RL |
| Preference Alignment on Diffusion Models | 2502.07829 | DPO/RLHF for diffusion |
| Survey of Safety on Large VLMs | 2502.14881 | Full LVLM safety landscape |
| Unified Multimodal Understanding & Generation | 2505.02567 | Taxonomy of unified models |
| Multimodal RAG Survey | ACL 2025 | First structured MRAG taxonomy |
| VLM Continual Learning Survey | 2508.04227 | VLM-specific CL challenges |
| Post-training of LLMs Survey | 2503.06072 | Definitive post-training taxonomy |
| Discrete Tokenization Survey | 2507.22920 | Tokenization infrastructure |
