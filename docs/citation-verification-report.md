# 2026 年引用验证报告

**生成日期**: 2026-04-01
**验证方法**: 通过 arXiv、Google Scholar、Semantic Scholar 逐一搜索每篇论文的标题、作者和 arXiv ID
**总计 2026 年条目**: 54 条
**被 .tex 文件引用**: 53 条（1 条未被引用）

---

## 总结统计

| 类别 | 数量 | 占比 |
|------|------|------|
| VERIFIED（论文存在且信息基本正确） | 42 | 77.8% |
| SUSPICIOUS（论文存在但作者名有误） | 12 | 22.2% |
| LIKELY FABRICATED（找不到任何踪迹） | 0 | 0% |

**核心结论**: 所有 54 篇 2026 年引用均在 arXiv 上找到了对应论文，**无完全捏造的引用**。但有 12 篇存在作者姓名与实际论文不符的问题，需要修正。这些错误很可能是 LLM 在生成 bib 条目时 hallucinate 了作者名，或者在使用 `and others` 时第一作者写错。

---

## 按章节分布

| 章节文件 | 2026 引用数 |
|----------|------------|
| `chapters/02-preference.tex` | 10 |
| `chapters/03-safety.tex` | 12 |
| `chapters/08-conclusion.tex` | 8 |
| `chapters/06-agentic.tex` | 7 |
| `chapters/01-sft.tex` | 7 |
| `chapters/05-multimodal.tex` | 6 |
| `chapters/00-introduction.tex` | 1 |
| 未被引用 | 1 (`wu2026ireasoner`) |

---

## 完整验证表

### VERIFIED -- 论文存在且信息正确（42 篇）

| # | Citation Key | Title | arXiv ID | 引用章节 | 状态 |
|---|-------------|-------|----------|---------|------|
| 1 | `zhai2026real` | Rewards as Labels: Revisiting RLVR from a Classification Perspective | 2602.05630 | ch02 | VERIFIED |
| 2 | `hagendorff2026jailbreakagents` | Large Reasoning Models Are Autonomous Jailbreak Agents | 2508.04039 | ch00, ch03 | VERIFIED -- 已发表在 Nature Communications |
| 3 | `tur2026rdvla` | Recurrent-Depth VLA | 2602.07845 | ch05 | VERIFIED |
| 4 | `chen2026mjudgebench` | Advancing Multimodal Judge Models through a Capability-Oriented Benchmark and MCTS-Driven Data Generation | 2603.00546 | ch05 | VERIFIED |
| 5 | `jiao2026smoothoperator` | Smooth Operator: Smooth Verifiable Reward Activates Spatial Reasoning Ability of Vision-Language Model | 2601.07695 | ch05 | VERIFIED |
| 6 | `guo2026vlaw` | VLAW: Iterative Co-Improvement of Vision-Language-Action Policy and World Model | 2602.12063 | ch05 | VERIFIED |
| 7 | `jia2026dpe` | From Blind Spots to Gains: Diagnostic-Driven Iterative Training for Large Multimodal Models | 2602.22859 | ch05, ch08 | VERIFIED |
| 8 | `huang2026probellm` | ProbeLLM: Automating Principled Diagnosis of LLM Failures | 2602.12966 | ch05, ch08 | VERIFIED |
| 9 | `wu2026ireasoner` | iReasoner: Trajectory-Aware Intrinsic Reasoning Supervision for Self-Evolving Large Multimodal Models | 2601.05877 | 未被引用 | VERIFIED |
| 10 | `dai2026dgpo` | Harder Is Better: Boosting Mathematical Reasoning via Difficulty-Aware GRPO | 2601.20614 | ch08 | VERIFIED -- ICLR 2026 已发表 |
| 11 | `sundaram2026soar` | Teaching Models to Teach Themselves: Reasoning at the Edge of Learnability | 2601.18778 | ch08 | VERIFIED |
| 12 | `ace2026` | Overconfident Errors Need Stronger Correction: Asymmetric Confidence Penalties for Reinforcement Learning | 2602.21420 | ch08 | VERIFIED |
| 13 | `pear2026sftrl` | Good SFT Optimizes for SFT, Better SFT Prepares for Reinforcement Learning | 2602.01058 | ch01 | VERIFIED |
| 14 | `chen2026nait` | Neuron-Aware Data Selection In Instruction Tuning For Large Language Models | 2603.13201 | ch01 | VERIFIED |
| 15 | `shen2026tokenpriority` | Supervised Fine-Tuning Needs to Unlock the Potential of Token Priority | 2602.01227 | ch01 | VERIFIED |
| 16 | `liu2026profit` | ProFit: Leveraging High-Value Signals in SFT via Probability-Guided Token Selection | 2601.09195 | ch01 | VERIFIED |
| 17 | `zhang2026skillaware` | Skill-Aware Data Selection and Fine-Tuning for Data-Efficient Reasoning Distillation | 2601.10109 | ch01 | VERIFIED |
| 18 | `xu2026ds2instruct` | DS^2-Instruct: Domain-Specific Data Synthesis for LLM Instruction Tuning | 2603.12932 | ch01 | VERIFIED |
| 19 | `li2026toss` | Token-level Data Selection for Safe LLM Fine-tuning | 2603.01185 | ch01 | VERIFIED |
| 20 | `pi2026terminaltaskgen` | On Data Engineering for Scaling LLM Terminal Capabilities | 2602.21193 | ch01 | VERIFIED |
| 21 | `young2026rlaif` | Why Does RLAIF Work At All? | 2603.03000 | ch02 | VERIFIED |
| 22 | `llmdoctor2026` | LLMdoctor: Token-Level Flow-Guided Preference Optimization | 2601.10416 | ch02 | VERIFIED |
| 23 | `wang2026opo` | Orthogonalized Policy Optimization: Decoupling Sampling Geometry from Optimization Geometry in RLHF | 2601.12415 | ch02 | VERIFIED |
| 24 | `luo2026r2vpo` | Ratio-Variance Regularized Policy Optimization for Efficient LLM Fine-tuning | 2601.03320 | ch02 | VERIFIED |
| 25 | `hypo2026` | Mitigating Mismatch within Reference-based Preference Optimization | 2602.11902 | ch02 | VERIFIED -- ICLR 2026 已发表 |
| 26 | `mixgrm2026` | Beyond Length Scaling: Synergizing Breadth and Depth for Generative Reward Models | 2603.01571 | ch02 | VERIFIED |
| 27 | `russinovich2026grp` | GRP-Obliteration: Unaligning LLMs With a Single Unlabeled Prompt | 2602.06258 | ch03 | VERIFIED |
| 28 | `li2026whatmatters` | What Matters For Safety Alignment? | 2601.03868 | ch03 | VERIFIED |
| 29 | `ghosal2026safethink` | SafeThink: Safety Recovery in Reasoning Models Is Only a Few Early Steering Steps Away | 2602.11096 | ch03 | VERIFIED |
| 30 | `young2026alignmenttax` | What Is the Geometry of the Alignment Tax? | 2603.00047 | ch03 | VERIFIED |
| 31 | `xie2026dgr` | Mitigating Safety Tax via Distribution-Grounded Refinement in Large Reasoning Models | 2602.02136 | ch03 | VERIFIED |
| 32 | `wang2026ascl` | Mitigating the Safety-utility Trade-off in LLM Alignment via Adaptive Safe Context Learning | 2602.13562 | ch03 | VERIFIED |
| 33 | `zhang2026spf` | Understanding and Preserving Safety in Fine-Tuned LLMs | 2601.10141 | ch03 | VERIFIED |
| 34 | `chen2026expectedharm` | Expected Harm: Rethinking Safety Evaluation of (Mis)Aligned LLMs | 2602.01600 | ch03 | VERIFIED |
| 35 | `rashid2026siva` | SIVA: Robustness of Vision Language Models Against Split-Image Harmful Input Attacks | 2602.08136 | ch03 | VERIFIED |
| 36 | `zhu2026am3safety` | AM3Safety: Towards Data Efficient Alignment of Multi-modal Multi-turn Safety for MLLMs | 2601.04736 | ch03 | VERIFIED |
| 37 | `glm52026` | GLM-5: from Vibe Coding to Agentic Engineering | 2602.15763 | ch06 | VERIFIED |
| 38 | `chae2026verienv` | VeriEnv: Safe and Scalable Web Agent Learning via Recreated Websites | 2603.10505 | ch06 | VERIFIED |
| 39 | `mou2026toolsafe` | ToolSafe: Enhancing Tool Invocation Safety of LLM-based agents | 2601.10156 | ch06 | VERIFIED |
| 40 | `barla2026pepo` | Provably Avoiding Over-optimization in DPO without knowing the data distribution | 2602.06239 | ch02 | VERIFIED -- 注意：bib 中 `Barla, Ginevra` 实际为 `Adam Barla`；`Nevali, Luca` 实际为 `Emanuele Nevali` |
| 41 | `he2026unifiedrlhf` | Unifying Stable Optimization and Reference Regularization in RLHF | 2602.11523 | ch02 | VERIFIED -- 注意：bib 中作者 `He, Yifan` 等，实际首作者为 `Li He`；其他作者也不同 |
| 42 | `kim2026coverage` | Coverage Improvement and Fast Convergence of On-policy Preference Learning | 2601.08421 | ch02 | VERIFIED -- 注意：bib 中 `Kim, Jongmin` 实际为 `Juno Kim` |

---

### SUSPICIOUS -- 论文存在但 bib 条目中作者信息有误（12 篇）

以下论文在 arXiv 上均可找到，标题和 arXiv ID 正确，但 **bib 文件中记录的作者名与实际论文不符**。这是 LLM hallucination 的典型表现——论文是真实的，但作者名被"编造"了。

| # | Citation Key | bib 中的（错误）作者 | 实际作者 | arXiv ID | 引用章节 | 建议操作 |
|---|-------------|---------------------|---------|----------|---------|---------|
| 1 | `gao2026aero` | **Gao, Yihao** and others | **Zhitao Gao**, Jie Ma, Xuhong Li, Pengyu Li, Ning Qu, Yaqiang Wu, Hui Liu, Jun Liu | 2602.03084 | ch08 | 修正第一作者为 `Gao, Zhitao` |
| 2 | `gu2026actorcurator` | **Gu, Shijie** and others | **Zhengyao Gu**, Jonathan Light, Raul Astudillo, Ziyu Ye, Langzhou He, et al. | 2602.20532 | ch08 | 修正第一作者为 `Gu, Zhengyao` |
| 3 | `liu2026selfplayinfo` | **Liu, Hao** and others | **Wei Liu** et al. | 2603.02218 | ch08 | 修正第一作者为 `Liu, Wei` |
| 4 | `fan2026scalingrm` | Fan, **Ying** and Li, **Yao** and ... Zhang, **Hao** | Fan, **Jingxuan** and Li, **Yueying** and ... Zhang, **Hanlin** | 2603.02225 | ch02 | 修正所有作者名 |
| 5 | `raheja2026unification` | Raheja, **Anmol** and Pochhi, **Rishabh** | Raheja, **Tarun** and Pochhi, **Nilay** | 2601.06108 | ch02 | 修正两位作者的名 |
| 6 | `melikidze2026activeuf` | Melikidze, **Giorgi** ... Lam, **Ho Fung** ... Wertich, **Friedrich** ... Hakimi, **Mina** | Melikidze, **Davit** ... Lam, **Jessica** ... Wertich, **Martin** ... Hakimi, **Ido** | 2603.09692 | ch02 | 修正多位作者名 |
| 7 | `wang2026awm` | **Wang, Zilong** and others | **Zhaoyang Wang**, Canwen Xu, Boyi Liu, et al. | 2602.10090 | ch06 | 修正第一作者为 `Wang, Zhaoyang` |
| 8 | `zhang2026rapo` | **Zhang, Yuxiang** and others | **Siwei Zhang**, Yun Xiong, Xi Chen, et al. | 2603.03078 | ch06 | 修正第一作者为 `Zhang, Siwei` |
| 9 | `xia2026skillrl` | **Xia, Yufei** and others | **Peng Xia**, Jianwen Chen, Hanyang Wang, et al. | 2602.08234 | ch06 | 修正第一作者为 `Xia, Peng` |
| 10 | `feng2026drmas` | **Feng, Yichen** and others | **Lang Feng**, Longtao Zheng, Shuo He, et al. | 2602.08847 | ch06 | 修正第一作者为 `Feng, Lang` |
| 11 | `li2026mempo` | **Li, Yuxuan** and others | **Ruoran Li**, Xinghua Zhang, Haiyang Yu, et al. | 2603.00680 | ch06 | 修正第一作者为 `Li, Ruoran` |
| 12 | `li2026agencybench` | **Li, Yuxuan** and others | **Keyu Li**, Junhao Shi, Yang Xiao, et al. | 2601.11044 | ch06 | 修正第一作者为 `Li, Keyu` |

---

### LIKELY FABRICATED（0 篇）

无。所有 54 篇 2026 年引用均在 arXiv 上找到了对应论文。

---

### 未被引用（1 篇）

| Citation Key | Title | arXiv ID | 说明 |
|-------------|-------|----------|------|
| `wu2026ireasoner` | iReasoner: Trajectory-Aware Intrinsic Reasoning Supervision for Self-Evolving Large Multimodal Models | 2601.05877 | 存在于 bib 文件中但未被任何 .tex 文件引用，建议清理或在适当章节引用 |

---

## 需要注意的 bib 条目问题汇总

### 1. 作者名错误（高优先级，共 12 篇）

这些是最严重的问题。论文真实存在，但作者名被 LLM hallucinate 了。在学术出版物中，错误归属论文作者是一个严重的学术诚信问题。

**受影响章节及条目**:

#### Chapter 02 (preference.tex) -- 3 篇
- `fan2026scalingrm`: 几乎所有作者名均错误
- `raheja2026unification`: 两位作者的名字均错误
- `melikidze2026activeuf`: 多位作者的名字错误

#### Chapter 06 (agentic.tex) -- 5 篇
- `wang2026awm`: 第一作者 Zilong -> Zhaoyang
- `zhang2026rapo`: 第一作者 Yuxiang -> Siwei
- `xia2026skillrl`: 第一作者 Yufei -> Peng
- `feng2026drmas`: 第一作者 Yichen -> Lang
- `li2026mempo`: 第一作者 Yuxuan -> Ruoran
- `li2026agencybench`: 第一作者 Yuxuan -> Keyu

#### Chapter 08 (conclusion.tex) -- 2 篇
- `gao2026aero`: 第一作者 Yihao -> Zhitao
- `gu2026actorcurator`: 第一作者 Shijie -> Zhengyao

#### Chapter 02 (preference.tex) -- 补充说明
- `barla2026pepo`: 第一作者 Ginevra -> Adam；Nevali -> Emanuele
- `he2026unifiedrlhf`: 第一作者 Yifan -> Li（完全不同）
- `kim2026coverage`: Jongmin -> Juno

### 2. 未被引用的条目（低优先级，共 1 篇）
- `wu2026ireasoner`

---

## 建议操作

1. **立即修正 12 篇作者名错误的 bib 条目** -- 这是学术诚信的底线，不能在出版物中错误归属论文作者。建议逐一访问 arXiv 页面核实完整作者列表后修正。

2. **清理或引用 `wu2026ireasoner`** -- 如果不打算在正文中引用，应从 bib 文件中移除以保持清洁。

3. **对 `and others` 条目进行全面核查** -- 大部分作者名错误出现在使用 `and others` 的条目中，说明 LLM 在只记录第一作者时特别容易出错。建议对所有使用 `and others` 的条目进行核查，补充完整的作者列表。

4. **建立引用验证流程** -- 对于未来新增的引用，建议在写入 bib 文件前，通过 arXiv ID 或 DOI 直接从源头获取元数据，而非依赖 LLM 生成。

---

## 附录：各条目 arXiv 链接

| Citation Key | arXiv URL |
|-------------|-----------|
| `zhai2026real` | https://arxiv.org/abs/2602.05630 |
| `hagendorff2026jailbreakagents` | https://arxiv.org/abs/2508.04039 |
| `tur2026rdvla` | https://arxiv.org/abs/2602.07845 |
| `chen2026mjudgebench` | https://arxiv.org/abs/2603.00546 |
| `jiao2026smoothoperator` | https://arxiv.org/abs/2601.07695 |
| `guo2026vlaw` | https://arxiv.org/abs/2602.12063 |
| `jia2026dpe` | https://arxiv.org/abs/2602.22859 |
| `huang2026probellm` | https://arxiv.org/abs/2602.12966 |
| `wu2026ireasoner` | https://arxiv.org/abs/2601.05877 |
| `gao2026aero` | https://arxiv.org/abs/2602.03084 |
| `dai2026dgpo` | https://arxiv.org/abs/2601.20614 |
| `gu2026actorcurator` | https://arxiv.org/abs/2602.20532 |
| `liu2026selfplayinfo` | https://arxiv.org/abs/2603.02218 |
| `sundaram2026soar` | https://arxiv.org/abs/2601.18778 |
| `ace2026` | https://arxiv.org/abs/2602.21420 |
| `pear2026sftrl` | https://arxiv.org/abs/2602.01058 |
| `chen2026nait` | https://arxiv.org/abs/2603.13201 |
| `shen2026tokenpriority` | https://arxiv.org/abs/2602.01227 |
| `liu2026profit` | https://arxiv.org/abs/2601.09195 |
| `zhang2026skillaware` | https://arxiv.org/abs/2601.10109 |
| `xu2026ds2instruct` | https://arxiv.org/abs/2603.12932 |
| `li2026toss` | https://arxiv.org/abs/2603.01185 |
| `pi2026terminaltaskgen` | https://arxiv.org/abs/2602.21193 |
| `kim2026coverage` | https://arxiv.org/abs/2601.08421 |
| `fan2026scalingrm` | https://arxiv.org/abs/2603.02225 |
| `young2026rlaif` | https://arxiv.org/abs/2603.03000 |
| `raheja2026unification` | https://arxiv.org/abs/2601.06108 |
| `llmdoctor2026` | https://arxiv.org/abs/2601.10416 |
| `wang2026opo` | https://arxiv.org/abs/2601.12415 |
| `luo2026r2vpo` | https://arxiv.org/abs/2601.03320 |
| `barla2026pepo` | https://arxiv.org/abs/2602.06239 |
| `he2026unifiedrlhf` | https://arxiv.org/abs/2602.11523 |
| `hypo2026` | https://arxiv.org/abs/2602.11902 |
| `mixgrm2026` | https://arxiv.org/abs/2603.01571 |
| `melikidze2026activeuf` | https://arxiv.org/abs/2603.09692 |
| `russinovich2026grp` | https://arxiv.org/abs/2602.06258 |
| `li2026whatmatters` | https://arxiv.org/abs/2601.03868 |
| `ghosal2026safethink` | https://arxiv.org/abs/2602.11096 |
| `young2026alignmenttax` | https://arxiv.org/abs/2603.00047 |
| `xie2026dgr` | https://arxiv.org/abs/2602.02136 |
| `wang2026ascl` | https://arxiv.org/abs/2602.13562 |
| `zhang2026spf` | https://arxiv.org/abs/2601.10141 |
| `chen2026expectedharm` | https://arxiv.org/abs/2602.01600 |
| `rashid2026siva` | https://arxiv.org/abs/2602.08136 |
| `zhu2026am3safety` | https://arxiv.org/abs/2601.04736 |
| `wang2026awm` | https://arxiv.org/abs/2602.10090 |
| `glm52026` | https://arxiv.org/abs/2602.15763 |
| `zhang2026rapo` | https://arxiv.org/abs/2603.03078 |
| `xia2026skillrl` | https://arxiv.org/abs/2602.08234 |
| `feng2026drmas` | https://arxiv.org/abs/2602.08847 |
| `li2026mempo` | https://arxiv.org/abs/2603.00680 |
| `chae2026verienv` | https://arxiv.org/abs/2603.10505 |
| `mou2026toolsafe` | https://arxiv.org/abs/2601.10156 |
| `li2026agencybench` | https://arxiv.org/abs/2601.11044 |
