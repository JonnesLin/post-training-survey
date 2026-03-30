# Gap Analysis: Papers Referenced in Related Work Sections

Cross-referencing related work sections from 8 key papers in Threads 6 and 7 against our current survey coverage. Papers already in our survey are marked with [COVERED]. Papers potentially missing are marked with [GAP].

---

## Thread 6: Agentic / Tool Use

### Source 1: ReAct (Yao et al., 2022) — Related Work (Section 5, pages 9-10)

**Research themes identified:**
1. Language models for reasoning (CoT, zero-shot CoT, self-consistency, STaR, Scratchpad)
2. Language models for decision making (WebGPT, Sparrow, BlenderBot, SimpleTOD)
3. LLMs for planning in interactive/embodied environments (SayCan, Inner Monologue)
4. Language as a cognitive mechanism for decision making

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| Chain-of-Thought (Wei et al., 2022) | [COVERED in Thread 4] | Reasoning thread |
| Least-to-most prompting (Zhou et al., 2022) | [GAP] | Compositional reasoning decomposition |
| Zero-shot CoT (Kojima et al., 2022) | [GAP] | "Let's think step by step" |
| Self-consistency (Wang et al., 2022a) | [GAP] | Sampling-based reasoning improvement |
| Madaan & Yazdanbakhsh, 2022 | [GAP] | Study of CoT formulation and structure |
| Selection-Inference (Creswell et al., 2022) | [GAP] | Two-step reasoning architecture |
| STaR (Zelikman et al., 2022) | [COVERED in Thread 4] | Bootstrapping reasoning |
| Faithful reasoning (Creswell & Shanahan, 2022) | [GAP] | Multi-step reasoning with dedicated LMs |
| Scratchpad (Nye et al., 2021) | [GAP] | Intermediate computation finetuning |
| WebGPT (Nakano et al., 2021) | [COVERED] | Browser-assisted QA |
| BlenderBot (Shuster et al., 2022b) | [GAP] | Conversational agent with retrieval |
| Sparrow (Glaese et al., 2022) | [GAP] | DeepMind dialogue agent with rules |
| SimpleTOD (Hosseini-Asl et al., 2020) | [GAP] | Task-oriented dialogue with API calls |
| SayCan (Ahn et al., 2022) | [GAP] | Grounding LLMs in robotic affordances |
| Inner Monologue (Huang et al., 2022b) | [GAP] | Embodied reasoning through planning with LLMs |
| Abramson et al., 2020 | [GAP] | Imitating interactive intelligence |
| Karamcheti et al., 2021 | [GAP] | Language in interactive decision making |
| Reed et al., 2022 (Gato) | [GAP] | Generalist agent |

**Potentially important gaps for our survey:**
- **Inner Monologue** (Huang et al., 2022b): Direct precursor to ReAct for embodied agent reasoning
- **SayCan** (Ahn et al., 2022): Foundational work on grounding LLMs for robot actions
- **Sparrow** (Glaese et al., 2022): Important alignment-with-rules approach for dialogue agents

---

### Source 2: Toolformer (Schick et al., 2023) — Related Work (Section 6, pages 9-11)

**Research themes identified:**
1. Language model pretraining with augmented information (metadata, HTML tags, retrieval)
2. Tool use via search engines, browsers, calculators, translation, Python interpreters
3. Bootstrapping and self-training techniques
4. Distinction between human-supervised vs. prompt-based tool learning

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| Keskar et al., 2019 (CTRL) | [GAP] | Conditional language model with metadata |
| Aghajanyan et al., 2021 (HTLM) | [GAP] | Hyper-text pre-training and prompting |
| Schick et al., 2022 | [GAP] | Wikipedia markup pretraining |
| Guu et al., 2020 (REALM) | [GAP] | Retrieval-augmented language model pre-training |
| Borgeaud et al., 2021 (RETRO) | [GAP] | Retrieval from trillions of tokens |
| Izacard et al., 2022 (Atlas) | [GAP] | Few-shot learning with retrieval |
| Komeili et al., 2022 | [GAP] | Internet-augmented dialogue generation |
| Thoppilan et al., 2022 (LaMDA) | [GAP] | Dialogue model using tools |
| Lazaridou et al., 2022 | [GAP] | Few-shot prompting for tool use |
| Shuster et al., 2022 | [GAP] | Language models using search tools |
| Cobbe et al., 2021 | [GAP] | Training verifiers to solve math word problems |
| Gao et al., 2022 (PAL) | [GAP] | Program-aided language models |
| TALM (Parisi et al., 2022) | [COVERED] | Tool augmented language models |
| WebGPT (Nakano et al., 2021) | [COVERED] | Browser-assisted QA |
| Yarowsky, 1995 | [GAP] | Classic self-training/bootstrapping |
| Schick and Schutze, 2021a | [GAP] | Few-shot text classification bootstrapping |
| Izacard and Grave, 2021 | [GAP] | Distilling knowledge for retrieval QA |
| Zelikman et al., 2022 (STaR) | [COVERED in Thread 4] | Bootstrapping reasoning |

**Potentially important gaps for our survey:**
- **PAL** (Gao et al., 2022): Program-aided language models -- directly relevant to tool use for code execution
- **LaMDA** (Thoppilan et al., 2022): Google's dialogue model that learns to use tools
- **Cobbe et al., 2021** (verifiers for math): Important precursor to reward-model-based verification
- **REALM/RETRO**: Retrieval-augmented approaches relevant to tool-augmented paradigm

---

### Source 3: Voyager (Wang et al., 2023) — Related Work (Section 5, pages 10-11)

**Research themes identified:**
1. Decision-making agents in Minecraft (low-level controllers, high-level planners)
2. Large language models for agent planning (robot learning, text agents)
3. Code generation with execution feedback
4. Embodied agents and multimodal planning

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| MineDojo (Fan et al., 2022) | [GAP] | Minecraft benchmark with YouTube videos |
| VPT (Baker et al., 2022) | [GAP] | Video Pre-Training for Minecraft |
| DreamerV3 (Hafner et al., 2023) | [GAP] | World model for open-ended exploration |
| Codex (Chen et al., 2021) | [GAP] | Code generation model -- foundational |
| Code as Policies (Liang et al., 2023) | [GAP] | LLMs generate executable robot policies |
| ProgPrompt (Singh et al., 2022) | [GAP] | Programmatic prompting for robots |
| VIMA (Jiang et al., 2022) | [GAP] | Multimodal prompts for robot manipulation |
| PaLM-E (Driess et al., 2023) | [GAP] | Pre-trained LLMs with embodied inputs |
| ReAct (Yao et al., 2022) | [COVERED] | Reasoning + acting |
| Reflexion (Shinn et al., 2023) | [COVERED] | Self-reflection for agents |
| AutoGPT (Richards, 2023) | [GAP] | Popular autonomous agent tool |
| DERA (Nair et al., 2023) | [GAP] | Task as dialogue between GPT-4 agents |
| Generative Agents (Park et al., 2023) | [GAP] | Simulating human behaviors with LLM agents |
| SPRING (Wu et al., 2023) | [GAP] | Game mechanics from manuals via GPT-4 |
| Inner Monologue (Huang et al., 2022) | [GAP] | Embodied reasoning with LLMs |
| SayCan (Ahn et al., 2022) | [GAP] | Robotic affordances with LLMs |
| Chen et al., 2021 (CodeRL related) | [COVERED] | Evaluating code generation |

**Potentially important gaps for our survey:**
- **Generative Agents** (Park et al., 2023): Landmark work on LLM-based agent simulation
- **AutoGPT**: Highly influential open-source autonomous agent
- **Code as Policies** (Liang et al., 2023): LLMs directly generating executable policies
- **Codex** (Chen et al., 2021): Foundational code generation model underpinning many tool-use works
- **PaLM-E** (Driess et al., 2023): Embodied multimodal LLM

---

### Source 4: ToolLLM (Qin et al., 2023) — Introduction & References (pages 1-4, 10-12)

**Research themes identified:**
1. Instruction tuning datasets for tool use (limited APIs, constrained scenarios)
2. Planning and reasoning for tool use (CoT, ReACT, DFSDT)
3. Open-source LLM ecosystem (LLaMA, Alpaca, Vicuna)
4. Automatic evaluation of tool-use capabilities

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| Tool Learning Survey (Qin et al., 2023b) | [COVERED] | Foundation survey |
| LLaMA (Touvron et al., 2023a) | [GAP -- foundational] | Open-source base model |
| LLaMA 2 (Touvron et al., 2023b) | [GAP -- foundational] | Updated open-source model |
| Alpaca (Taori et al., 2023) | [GAP -- foundational] | Stanford instruction-tuned model |
| Vicuna (Chiang et al., 2023) | [GAP] | Open-source chatbot |
| Gorilla (Patil et al., 2023) | [COVERED] | Large API model |
| API-Bank (Li et al., 2023a) | [COVERED] | Tool benchmark |
| ToolAlpaca (Tang et al., 2023) | [COVERED] | Tool instruction tuning |
| Xu et al., 2023b | [GAP] | On tool manipulation capability |
| WizardLM (Xu et al., 2023a) | [COVERED in Thread 7] | Complex instruction following |
| Self-Instruct (Wang et al., 2022) | [COVERED in Thread 7] | Self-generated instructions |
| ChatGPT (OpenAI, 2022) | [GAP -- foundational] | Commercial baseline |
| GPT-4 (OpenAI, 2023) | [GAP -- foundational] | Commercial baseline |
| Bubeck et al., 2023 (Sparks of AGI) | [GAP] | GPT-4 capabilities analysis |
| CoT (Wei et al., 2023) | [COVERED in Thread 4] | Chain of thought |
| Falcon (Penedo et al., 2023) | [GAP] | Open-source base model |
| Sentence-BERT (Reimers & Gurevych, 2019) | [GAP] | Used for API retrieval |
| Tree of Thoughts (Yao et al., 2023) | [GAP] | Deliberate problem solving with LLMs |
| WebCPM (Qin et al., 2023a) | [GAP] | Interactive web search for Chinese QA |
| RestGPT (Song et al., 2023) | [GAP] | Connecting LLMs with REST APIs |
| Creator (Qian et al., 2023) | [GAP] | Tool creation by LLMs |
| AssistGPT (Gao et al., 2023) | [GAP] | General multi-modal assistant |
| GeneGPT (Jin et al., 2023) | [GAP] | Augmenting LLMs with domain tools |
| AlpacaEval (Li et al., 2023b) | [GAP] | Automatic evaluation of instruction following |

**Potentially important gaps for our survey:**
- **Tree of Thoughts** (Yao et al., 2023): Advanced reasoning strategy, extends CoT
- **Creator** (Qian et al., 2023): LLMs creating their own tools -- novel paradigm
- **RestGPT** (Song et al., 2023): REST API integration for LLMs
- **WebCPM** (Qin et al., 2023): Interactive web search agent

---

## Thread 7: Efficiency & Data Engineering

### Source 5: LoRA (Hu et al., 2021) — Related Work (Section 6, pages 8-9)

**Research themes identified:**
1. Transformer language models (GPT, BERT evolution)
2. Prompt engineering and fine-tuning approaches
3. Parameter-efficient adaptation (adapters, prefix tuning, prompt tuning)
4. Low-rank structures in deep learning (intrinsic dimensionality)

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| Transformer (Vaswani et al., 2017) | [GAP -- foundational] | Architecture foundation |
| GPT (Radford et al., 2018) | [GAP -- foundational] | Autoregressive LM |
| BERT (Devlin et al., 2019) | [GAP -- foundational] | Bidirectional pretraining |
| GPT-2 (Radford et al., 2019) | [GAP -- foundational] | Scaling autoregressive LMs |
| GPT-3 (Brown et al., 2020) | [GAP -- foundational] | In-context learning |
| Adapters (Houlsby et al., 2019) | [GAP] | Original adapter layers for NLP |
| Rebuffi et al., 2017 | [GAP] | Residual adapters for visual domains |
| Lin et al., 2020 | [GAP] | Single adapter per block variant |
| COMPACTER (Mahabadi et al., 2021) | [GAP] | Kronecker product adapters |
| Prefix Tuning (Li & Liang, 2021) | [GAP] | Continuous prompt optimization |
| Prompt Tuning (Lester et al., 2021) | [GAP] | Learned soft prompts |
| P-tuning (Liu et al., 2021) | [GAP] | Continuous prompts |
| Hambardzumyan et al., 2020 | [GAP] | Input word embedding optimization |
| Li et al., 2018a (intrinsic dimensionality) | [GAP] | Low-rank structure in ML |
| Aghajanyan et al., 2020 (intrinsic dimensionality) | [GAP] | Models reside on low intrinsic dimension |
| Ruckle et al., 2020 | [GAP] | Multi-task adapters |
| Pfeiffer et al., 2021 | [GAP] | AdapterFusion for multi-task |
| Collobert & Weston, 2008 | [GAP -- historical] | Multi-task learning for NLP |

**Potentially important gaps for our survey:**
- **Prefix Tuning** (Li & Liang, 2021): Major PEFT alternative to LoRA
- **Prompt Tuning** (Lester et al., 2021): Another important PEFT method
- **Adapters** (Houlsby et al., 2019): Original adapter method LoRA builds upon
- **Aghajanyan et al., 2020**: Intrinsic dimensionality -- theoretical motivation for LoRA
- **COMPACTER** (Mahabadi et al., 2021): Advanced adapter with Kronecker products

---

### Source 6: QLoRA (Dettmers et al., 2023) — Introduction & Experiments (pages 1-9)

**Research themes identified:**
1. Quantization techniques for LLMs (NF4, Double Quantization)
2. Memory-efficient finetuning (Paged Optimizers)
3. Instruction tuning dataset comparison (OASST1, FLAN v2, Alpaca, Chip2, etc.)
4. Evaluation methodology (human vs. GPT-4 evaluation, Elo ratings)

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| LoRA (Hu et al., 2021) | [COVERED] | Low-rank adapters |
| LLaMA (Touvron et al., 2023) | [GAP -- foundational] | Base model |
| Vicuna (Chiang et al., 2023) | [GAP] | Open-source chatbot |
| Alpaca (Taori et al., 2023) | [GAP -- foundational] | Instruction-tuned model |
| Self-Instruct (Wang et al., 2022) | [COVERED] | Self-generated instructions |
| HH-RLHF (Bai et al., 2022) | [GAP -- foundational] | Anthropic RLHF dataset |
| FLAN v2 (Chung et al., 2022) | [GAP] | Scaling instruction-finetuned models |
| Chip2 | [GAP] | Hybrid instruction dataset |
| Longform (Koksal et al., 2023) | [GAP] | Long-form instruction dataset |
| Unnatural Instructions (Honovich et al., 2022) | [GAP] | Almost no human labor instructions |
| OASST1 (Kopf et al., 2023) | [GAP] | Open Assistant crowdsourced dataset |
| GPT-4 (OpenAI, 2023) | [GAP -- foundational] | Evaluation baseline |
| MMLU (Hendrycks et al., 2021) | [GAP -- benchmark] | Multi-task language understanding |

**Potentially important gaps for our survey:**
- **OASST1 / Open Assistant** (Kopf et al., 2023): Important open-source RLHF dataset
- **FLAN v2** (Chung et al., 2022): Major instruction tuning collection
- **Unnatural Instructions** (Honovich et al., 2022): Novel synthetic data approach
- **Longform** (Koksal et al., 2023): Long-form instruction generation

---

### Source 7: Zephyr (Tunstall et al., 2023) — Related Work (Section 2, pages 2-3) & Experiments

**Research themes identified:**
1. Open LLM ecosystem (LLaMA, Falcon, Mistral, MPT, RedPajama)
2. Distillation for alignment (dSFT, dDPO, self-instruct method)
3. Benchmarking and evaluation (MT-Bench, AlpacaEval, LMSYS chatbot arena)
4. Direct Preference Optimization (DPO) as alternative to PPO

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| LLaMA (Touvron et al., 2023a) | [GAP -- foundational] | Open-source foundation model |
| LLaMA 2 (Touvron et al., 2023b) | [GAP -- foundational] | Updated foundation model |
| Falcon (Penedo et al., 2023) | [GAP] | Open-source LLM |
| Mistral-7B (Jiang et al., 2023) | [GAP] | Base model for Zephyr |
| MPT (MosaicML, 2023) | [GAP] | Open-source LLM |
| RedPajama-INCITE (Together AI, 2023) | [GAP] | Open-source LLM family |
| DPO (Rafailov et al., 2023) | [GAP -- foundational] | Direct preference optimization |
| PPO (Schulman et al., 2017) | [GAP -- foundational] | Proximal policy optimization |
| InstructGPT (Ouyang et al., 2022) | [GAP -- foundational] | RLHF for instruction following |
| Self-Instruct (Wang et al., 2023) | [COVERED] | Self-generated instructions |
| Alpaca (Taori et al., 2023) | [GAP -- foundational] | Instruction-tuned model |
| Vicuna (Chiang et al., 2023) | [GAP] | Open-source chatbot |
| WizardLM (Xu et al.) | [COVERED] | Complex instructions |
| Xwin-LM (Team, 2023) | [GAP] | PPO-distilled open LLM |
| UltraChat (Ding et al., 2023) | [COVERED] | Dialogue dataset |
| UltraFeedback (Cui et al., 2023) | [COVERED] | AI feedback dataset |
| MT-Bench / LMSYS (Zheng et al., 2023) | [GAP] | Chatbot arena benchmark |
| AlpacaEval (Li et al., 2023) | [GAP] | Automatic evaluation framework |
| AlpacaFarm (Dubois et al., 2023) | [GAP] | Simulation framework for RLHF |
| Chain-of-Thought Hub (Fu et al., 2023) | [GAP] | Reasoning evaluation |
| ChatEval (Sedoc et al., 2019) | [GAP] | Dialogue evaluation |
| FastEval (2023) | [GAP] | Fast evaluation tool |
| The False Promise of Imitating Proprietary LLMs (Gudibande et al., 2023) | [GAP] | Important critique of distillation |
| Harm De Vries "Go Smol or Go Home" (2023) | [GAP] | Compute-optimal small models |
| HH-RLHF (Bai et al., 2022) | [GAP -- foundational] | Safety training dataset |
| FLAN (Chung et al., 2022) | [GAP] | Instruction finetuning at scale |
| FlashAttention-2 (Dao, 2023) | [GAP] | Efficient attention implementation |
| DeepSpeed ZeRO-3 (Rajbhandari et al., 2020) | [GAP] | Distributed training |
| TRL (von Werra et al., 2020) | [GAP] | Transformer Reinforcement Learning library |

**Potentially important gaps for our survey:**
- **DPO** (Rafailov et al., 2023): Absolutely critical -- should be in Thread 2 (Preference) if not already
- **Mistral-7B** (Jiang et al., 2023): Key base model for many efficiency works
- **The False Promise of Imitating Proprietary LLMs** (Gudibande et al., 2023): Important critique of distillation approaches
- **AlpacaFarm** (Dubois et al., 2023): Simulation framework for RLHF methods
- **MT-Bench / LMSYS Chatbot Arena** (Zheng et al., 2023): Major evaluation framework
- **InstructGPT** (Ouyang et al., 2022): Foundational RLHF paper

---

### Source 8: Phi-1 (Gunasekar et al., 2023) — Introduction & Related Work (pages 1-3)

**Research themes identified:**
1. Scaling laws and data quality vs. quantity tradeoff
2. Code generation models (Codex, CodeGen, StarCoder, WizardCoder, etc.)
3. Synthetic data generation for training LLMs
4. Emergent capabilities from high-quality data
5. Recursive training / model collapse concerns

**Referenced papers:**

| Paper | Status | Notes |
|-------|--------|-------|
| Scaling laws (Kaplan et al., 2020) | [GAP -- foundational] | Neural scaling laws |
| Chinchilla (Hoffmann et al., 2022) | [GAP -- foundational] | Compute-optimal training |
| Codex (Chen et al., 2021) | [GAP] | Code generation foundation |
| CodeGen (Nijkamp et al., 2023) | [GAP] | Open code generation models |
| PaLM-Coder (Chowdhery et al., 2022) | [GAP] | PaLM for code |
| CodeGeeX (Zheng et al., 2023) | [GAP] | Multilingual code model |
| SantaCoder (Allal et al., 2023) | [GAP] | 1.1B code model |
| StarCoder (Li et al., 2023) | [GAP] | 15.5B code model |
| Replit (2023) | [GAP] | 2.7B code model |
| CodeGen2 (Nijkamp et al., 2023) | [GAP] | Improved code generation |
| WizardCoder (Luo et al., 2023) | [GAP] | Evol-Instruct for code |
| InstructCodeT5+ (Wang et al., 2023) | [GAP] | Instruction-tuned code model |
| PaLM 2-S (Anil et al., 2023) | [GAP] | Small efficient LLM |
| CodeT5+ (Wang et al., 2023) | [GAP] | Code understanding and generation |
| Eldan & Li, 2023 (TinyStories) | [GAP] | Synthetic data for small LMs |
| The Stack (Kocetkov et al., 2022) | [GAP] | Large-scale code dataset |
| Wang et al., 2022 (synthetic data for LLMs) | [GAP] | Using LLMs to synthesize training data |
| Taori et al., 2023 (Alpaca) | [GAP -- foundational] | Stanford Alpaca |
| Gunasekar et al., 2023 -- Mukherjee (Orca) | [COVERED] | Progressive learning |
| Li et al., 2023 (WizardLM) | [COVERED] | Complex instructions |
| Shumailov et al., 2023 | [GAP] | Model collapse from recursive training |
| Gudibande et al., 2023 | [GAP] | False promise of imitation |
| Bubeck et al., 2023 (Sparks of AGI) | [GAP] | GPT-4 capabilities |

**Potentially important gaps for our survey:**
- **TinyStories** (Eldan & Li, 2023): Direct precursor to phi-1, synthetic data for small models
- **WizardCoder** (Luo et al., 2023): Evol-Instruct applied to code -- bridges Thread 6 and 7
- **StarCoder** (Li et al., 2023): Major open code model
- **Chinchilla** (Hoffmann et al., 2022): Compute-optimal scaling -- foundational for efficiency thread
- **Model Collapse** (Shumailov et al., 2023): Critical concern for synthetic data approaches
- **Codex** (Chen et al., 2021): Foundation of code generation field

---

## Priority Gap Summary

### HIGH PRIORITY -- Papers that appear across multiple sources and are thematically central:

**For Thread 6 (Agentic/Tool Use):**

| Paper | Cited by | Why it matters |
|-------|----------|----------------|
| SayCan (Ahn et al., 2022) | ReAct, Voyager | Grounding LLMs in robotic affordances -- foundational for embodied agents |
| Inner Monologue (Huang et al., 2022) | ReAct, Voyager | Direct precursor to ReAct-style closed-loop reasoning |
| Generative Agents (Park et al., 2023) | Voyager | Landmark agent simulation work |
| AutoGPT (Richards, 2023) | Voyager | Highly influential autonomous agent system |
| PAL (Gao et al., 2022) | Toolformer | Program-aided LMs -- key tool-use paradigm |
| LaMDA (Thoppilan et al., 2022) | Toolformer | Google's tool-using dialogue model |
| Codex (Chen et al., 2021) | Voyager, phi-1 | Foundation of code generation + tool use |
| Code as Policies (Liang et al., 2023) | Voyager | LLMs generating executable robot code |
| Tree of Thoughts (Yao et al., 2023) | ToolLLM | Advanced reasoning for complex planning |
| Creator (Qian et al., 2023) | ToolLLM | LLMs creating their own tools |

**For Thread 7 (Efficiency):**

| Paper | Cited by | Why it matters |
|-------|----------|----------------|
| DPO (Rafailov et al., 2023) | Zephyr | Core algorithm for Zephyr -- should be in Thread 2 |
| Prefix Tuning (Li & Liang, 2021) | LoRA | Major PEFT method, primary LoRA comparator |
| Prompt Tuning (Lester et al., 2021) | LoRA | Another key PEFT alternative |
| Adapters (Houlsby et al., 2019) | LoRA | Original adapter method LoRA improves upon |
| Mistral-7B (Jiang et al., 2023) | Zephyr | Base model for Zephyr and many others |
| The False Promise of Imitating Proprietary LLMs (Gudibande et al., 2023) | Zephyr, phi-1 | Critical analysis of distillation limitations |
| FLAN v2 / Scaling Instruction-Finetuned Models (Chung et al., 2022) | QLoRA, Zephyr | Major instruction tuning collection |
| TinyStories (Eldan & Li, 2023) | phi-1 | Precursor to phi-1's data quality approach |
| AlpacaFarm (Dubois et al., 2023) | Zephyr | Simulation framework for alignment methods |
| OASST1 / Open Assistant (Kopf et al., 2023) | QLoRA | Key open-source RLHF dataset |
| WizardCoder (Luo et al., 2023) | phi-1 | Bridges code generation and instruction tuning |
| MT-Bench / Chatbot Arena (Zheng et al., 2023) | Zephyr | De facto evaluation standard |
| Unnatural Instructions (Honovich et al., 2022) | QLoRA | Novel synthetic instruction approach |
| Model Collapse (Shumailov et al., 2023) | phi-1 | Critical risk in synthetic data paradigm |
| Chinchilla (Hoffmann et al., 2022) | phi-1 | Compute-optimal scaling laws |

### MEDIUM PRIORITY -- Foundational works assumed as context:

These are widely cited but may be considered "pre-post-training era" or out of direct scope:
- LLaMA / LLaMA 2 (Touvron et al., 2023a/b)
- Alpaca (Taori et al., 2023)
- Vicuna (Chiang et al., 2023)
- InstructGPT (Ouyang et al., 2022)
- GPT-3 / GPT-4 / ChatGPT
- PPO (Schulman et al., 2017)
- Scaling Laws (Kaplan et al., 2020)

### LOW PRIORITY -- Domain-specific or older references:
- BlenderBot, SimpleTOD, Sparrow (conversational AI baselines)
- MineDojo, VPT, DreamerV3 (Minecraft-specific RL)
- Classical bootstrapping (Yarowsky 1995, Brin 1999)
- Low-rank theory papers (Li et al., 2016/2018, Cai et al., 2010)
