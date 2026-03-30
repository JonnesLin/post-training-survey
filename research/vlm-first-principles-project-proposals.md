# VLM 多模态 Post-Training Project Proposals

目标不是再列一批“可能能涨点”的题目，而是挑出几条真正值得赌的方向。这里的筛选标准只有四条：

1. 不是单靠更大模型或更多数据就会自然解决的问题。
2. 如果做成，会改变 VLM 的能力边界，而不是只刷高一个 benchmark。
3. 能从第一性原理不断追问 `why`，最终落到结构性 root cause。
4. 第一篇 paper 就能做出一个最小闭环，而不是空泛的大愿景。

## 如果不断问 `why`，当前 VLM post-training 真正卡在哪里

把最近两年的现象压缩到根上，核心不是“模型还不够大”，而是 5 个结构性错配：

1. `Reward is modality-blind`
   最终答案对了，训练目标就给奖励，但它并不在乎模型到底是“看图得出答案”，还是“靠语言先验蒙对答案”。
2. `Reasoning medium is text-dominant`
   VLM 被迫把大量中间推理早早翻译成 text tokens，但很多视觉任务真正需要保留的是几何、布局、局部细节和关系结构。
3. `Video is treated as frame bag`
   长视频理解大多还是“多看几帧 + 做摘要”，而不是维护一个随时间演化的世界状态。
4. `Evidence sources are not arbitrated`
   在医学、科学、工业等高价值场景里，视觉证据、外部知识和任务规范经常冲突，但现有 VLM 缺少显式的证据仲裁机制。
5. `Benchmarks still reward shortcuts`
   如果评测允许 shortcut，post-training 最终优化出来的就会是“看起来会看图”，而不是“真的依赖图像推理”。

## 一页 shortlist

| 方向 | 核心 question | 影响力 | 风险 | 第一篇 paper 形态 |
|---|---|---:|---:|---|
| Proposal 1. Visual Commitment RL | 怎样避免 VLM 在 RL 后越来越不看图？ | 很高 | 中高 | 方法 + 诊断 benchmark |
| Proposal 2. Latent Visual Workspace | 视觉推理一定要通过文字中间链吗？ | 很高 | 高 | 新训练范式 + reasoning interface |
| Proposal 3. Stateful Video World Model | 长视频理解为什么扩 context 仍然不稳？ | 很高 | 中高 | 方法 + memory analysis |
| Proposal 4. Evidence-Arbitrating Expert VLM | 图像、检索知识、规范冲突时谁说了算？ | 很高 | 中 | 系统性方法 + 高价值应用 |
| Proposal 5. Counterfactual Intervention Training | 怎样逼模型学到“因视觉变化而变”的能力？ | 很高 | 中 | benchmark + 训练框架 |

---

## Proposal 1. Visual Commitment RL

### 一句话摘要

VLM 在 post-training 阶段最大的风险不是“不会推理”，而是“越训练越不看图”。核心问题是 reward 只看答案，不看证据来源。这个方向的目标，是让模型在推理前先做可审计的视觉承诺，再对承诺后的推理给奖励。

### 第一性原理链条

1. `Why` 很多 VLM 一旦进入 RL 或 preference optimization，就会更依赖语言先验？
   因为最终 reward 只评价答对没有，不评价答案是不是建立在视觉证据上。
2. `Why` 这在 VLM 中比 text-only 更严重？
   因为语言 backbone 的先验远强于视觉分支，很多问题哪怕不看图也能靠常识、模板或数据偏置猜到一部分。
3. `Why` 只做更好的 PRM/ORM 还不够？
   因为大多数 verifier 仍然是在审查“文字推理看起来是否合理”，不是审查“文字推理是否被视觉事实支撑”。
4. `Why` 这是根因？
   因为当前训练目标把图像当作可选上下文，而不是必须提交的证据。

### 核心假设

如果把 VLM 的回答过程拆成两段：

1. 先生成 `visual commitments`
   例如对象、属性、空间关系、数量、局部区域引用。
2. 再基于这些 commitments 做语言推理。

那么模型就更难绕开视觉分支，RL 也会更像“强化 grounded reasoning”，而不是“强化更会编的语言输出”。

### 方法草案

1. 引入 `commitment tokens`
   回答前必须先输出若干视觉承诺，每条承诺绑定 region / object / relation。
2. 设计三类 reward
   `answer reward` 评价最终答案；
   `commitment fidelity reward` 评价承诺是否和图像一致；
   `consistency reward` 评价最终推理是否真正使用了这些承诺。
3. 构造语言先验与视觉证据冲突的数据
   比如品牌 logo、计数、局部编辑、时序反转，让“只靠常识猜”必然吃亏。
4. 在训练中加入 `re-perception check`
   模型做出承诺后，对相关区域再次编码，检查承诺是否稳定。

### 最小闭环实验

1. 用 counting / logo / spatial relation 三类明显受语言偏置影响的任务做第一版。
2. 比较四个版本：
   `SFT`、`answer-only RL`、`PRM-style RL`、`visual commitment RL`。
3. 除了 accuracy，还看：
   grounding error、commitment error、answer-right-but-for-wrong-reason 的比例。

### 为什么 high impact

这是 VLM post-training 最底层的训练目标问题。只要 reward 仍然是模态盲的，后面很多 fancy 方法都只是在更高层修补。这个方向如果成立，会直接影响 multimodal RL、reasoning VLM、agent VLM 甚至 medical VLM 的训练范式。

### 最大风险

模型可能学会模板化地“假装先看图”，把 commitments 变成新的 verbosity。

### 降风险方案

把 reward 重点放在反事实设定上：
同一问题、同一语言上下文，只改图像局部。如果 commitments 不跟着变，就直接判错。

---

## Proposal 2. Latent Visual Workspace

### 一句话摘要

很多 VLM 的失败，不是 reasoning depth 不够，而是 reasoning medium 错了。视觉问题过早翻译成文字，会把真正关键的空间、布局和局部结构信息丢掉。这个方向要做的是一个“潜在视觉工作区”，让推理的一部分继续在视觉表征里发生。

### 第一性原理链条

1. `Why` VLM 在空间、图表、几何、计数、局部关系任务上经常不稳？
   因为这些任务不是单纯的语义识别，而是需要保留结构化的中间状态。
2. `Why` 语言 chain-of-thought 不足以补救？
   因为 text token 擅长离散符号推理，但不擅长保真地承载几何、相对位置和细粒度视觉布局。
3. `Why` 更长的 CoT 反而可能更差？
   因为一旦早期文字总结错了，后面所有推理都在错误抽象上展开，无法回到原始视觉状态。
4. `Why` 这是根因？
   因为模型被迫把“看”和“想”过早地投影到了语言空间里。

### 核心假设

真正强的 VLM 不应该只有 text CoT，还应该有一个 `latent visual workspace`：
模型可以在其中做 zoom、mark、compare、measure、rewrite 等中间操作，再把最后结论翻译成自然语言。

### 方法草案

1. 在 connector 和 LLM 之间加入一层可迭代更新的 `workspace tokens`。
2. 允许模型执行一小组显式操作：
   `zoom(region)`、`compare(a,b)`、`trace(line)`、`count(set)`、`measure(relative distance)`。
3. 训练信号来自两类来源：
   工具生成的可验证中间步骤；
   人工或自动标注的 step-level correctness。
4. 最终输出可以仍然是文本，但中间计算不必完全语言化。

### 最小闭环实验

1. 只挑三类任务先做：
   spatial reasoning、diagram/chart reasoning、counting。
2. 与纯 text CoT、tool-augmented prompting、program distillation 做对比。
3. 重点不只看 final answer，还看中间 workspace 是否能预测最终错误类型。

### 为什么 high impact

这是在问一个更本质的问题：
VLM 的“思考载体”到底该是什么。
如果这条线成立，它不是一个局部 trick，而是可能打开下一代 multimodal reasoning architecture。

### 最大风险

latent workspace 可能带来训练复杂度，却没有学到真正可迁移的视觉计算。

### 降风险方案

先从最容易验证的任务入手，只做可以被外部工具明确判定中间步骤是否正确的场景，比如图表、几何图、相对空间关系。

---

## Proposal 3. Stateful Video World Model

### 一句话摘要

长视频理解真正难的地方，不是帧不够多，而是模型没有维护一个会随时间演化的世界状态。它现在更像是在读“很多视觉快照”，而不是在跟踪一个动态世界。

### 第一性原理链条

1. `Why` 长视频任务在扩 context、抽帧优化后还是明显掉性能？
   因为视频信息量随着时间线性增长，而真正需要保留的是少量但关键的事件状态。
2. `Why` 压缩摘要也不够？
   因为摘要通常保留的是“看到了什么”，不是“什么状态被谁改变了、接下来会影响什么”。
3. `Why` 图像模型迁移到视频后总差一口气？
   因为 frame-level 表征天然适合识别，不天然适合建模状态转移和事件依赖。
4. `Why` 这是根因？
   因为现有 video post-training 仍在优化“更会读帧”，而不是“更会维护状态”。

### 核心假设

如果显式训练 VLM 去维护 `entity-event state memory`，并允许它在时间上写入、更新、回溯状态，那么长视频理解的收益会比单纯扩 context 更稳、更省、更可解释。

### 方法草案

1. 定义 `state slots`
   每个 slot 绑定一个实体或事件，如人物目标、物体位置、任务阶段、因果节点。
2. 训练三类动作：
   `write`、`update`、`rewind`。
3. 构造强顺序敏感数据：
   同样片段，不同顺序；
   去掉关键过渡帧；
   保持外观相似但因果不同。
4. 用 question-conditioned credit assignment
   只奖励那些对未来问答真正有用的 state updates。

### 最小闭环实验

1. 先在可控视频环境里做 proof-of-concept，让 ground-truth state 可得。
2. 再上长视频 benchmark，看随视频长度增长的性能衰减曲线。
3. 额外分析：
   错答时到底是 perception 错、memory 错，还是 reasoning 错。

### 为什么 high impact

视频、agent、机器人、监控、长时操作理解，底层都依赖同一个能力：
持续维护世界状态。这个方向如果做成，影响的不只是 video QA，而是所有“时间维度上的 VLM”。

### 最大风险

所谓的 state memory 可能最后只是另一种摘要缓存。

### 降风险方案

必须加入 order-sensitive 和 counterfactual video 评测。只要模型还能在时序被打乱时维持高分，就说明它没有真的学会 state transition。

---

## Proposal 4. Evidence-Arbitrating Expert VLM

### 一句话摘要

在高价值场景里，真正困难的不是“图像 + 文本一起输入”，而是视觉证据、外部知识和任务规范往往并不一致。当前 VLM 缺的不是更多 retrieval，而是一个显式的证据仲裁器。

### 第一性原理链条

1. `Why` 通用 VLM 到医疗、科学、工业、法律图文场景往往掉得厉害？
   因为这些任务通常不能只靠图像直接回答，必须引入外部知识或规则。
2. `Why` multimodal RAG 也还不够可靠？
   因为检索回来的文本常常和图像所见不完全一致，模型会把两者混成一个流畅但不可信的答案。
3. `Why` 继续做 end-to-end imitation 没法根治？
   因为 imitation 目标奖励的是“像专家回答”，不是“区分可见事实、外部证据和不确定推断”。
4. `Why` 这是根因？
   因为当前模型没有把回答看成一个多源证据共同裁决的过程。

### 核心假设

如果把回答分成三层证据：

1. `visual findings`
2. `retrieved knowledge`
3. `task constraints / norms`

并显式训练模型判断它们之间的支持、冲突和缺失，那么系统会更可靠，也更容易学会 `abstain` 或 `ask for more context`。

### 方法草案

1. 第一步只提取 `what is visually supported`，禁止混入外部知识。
2. 第二步基于 findings 去做 targeted retrieval，而不是让 retriever 盲目接整张图。
3. 第三步构建 `evidence graph`
   节点是发现、文献、规则、先验；
   边是支持、冲突、缺失。
4. 最后不只输出 answer，还允许：
   `answer / abstain / request-more-evidence`。

### 最小闭环实验

1. 先不直接上最难的医学 3D 报告，先在 context-augmented multimodal QA 上验证。
2. 指标不只看 accuracy，还看：
   citation faithfulness、calibration、abstention quality、evidence attribution。
3. 然后再迁移到更高价值的小规模专家场景。

### 为什么 high impact

这是最接近真实 deployment value 的方向之一。医疗、科研助手、工业检修、专利和法规图文分析，本质上都不是“看图答题”，而是“视觉证据参与知识密集型判断”。

### 最大风险

最后可能退化成“更复杂的 pipeline engineering”。

### 降风险方案

把创新点放在训练目标和 benchmark 上，而不是只搭系统。重点证明：模型何时该信图、何时该信检索、何时必须拒答。

---

## Proposal 5. Counterfactual Intervention Training

### 一句话摘要

如果 benchmark 奖励 shortcut，post-training 就一定会学 shortcut。要逼 VLM 学会真正的视觉因果依赖，就必须用“干预前后应该怎么变、哪些不该变”的数据和目标来训练它。

### 第一性原理链条

1. `Why` 很多 VLM 在标准 benchmark 上看起来很强，但一做局部编辑、空间扰动、顺序反转就崩？
   因为它学到的是相关性，不是“哪个视觉因素决定了答案”。
2. `Why` 静态分布上的 imitation 学不到这个能力？
   因为模型只看见世界是什么样，很少看见“当某个因素被单独改动时，答案应如何变化”。
3. `Why` 这对 post-training 特别关键？
   因为 post-training 决定模型最后学会的是 robust policy，还是 benchmark gaming policy。
4. `Why` 这是根因？
   因为当前数据和评测大多没有把 intervention 作为基本训练单位。

### 核心假设

如果训练数据从单样本变成 `intervention tuple`：

1. 原图 / 原视频
2. 只改一个关键视觉因素后的版本
3. 对应应该变化或不该变化的问题集合

那么模型就更容易学到视觉上的因果依赖，而不是语言模板和统计捷径。

### 方法草案

1. 构造 paired / triplet 数据：
   只改变一个对象、属性、空间关系、计数、遮挡或时序因素。
2. 训练两个目标：
   `directional consistency`
   变化该变的答案；
   `invariance`
   不该变的答案保持不变。
3. 加入 explanation target
   模型不仅回答变了什么，还回答“为什么这个变化应该影响 / 不影响答案”。
4. 用 intervention-centric benchmark 做统一评测。

### 最小闭环实验

1. 从最容易自动生成的计数、空间关系、局部编辑开始。
2. 再扩展到长视频顺序交换和关键帧删除。
3. 最后测是否能迁移到真实世界的 hallucination、compositional generalization 和 robustness benchmark。

### 为什么 high impact

这是 field-shaping 型方向。因为它改的不是单个模型，而是整个领域对“真的会看图”这件事的定义和测量方式。

### 最大风险

合成干预可能过于干净，模型在真实场景里不一定收益同样明显。

### 降风险方案

三种数据一起做：
真实图像编辑、渲染合成场景、人工构造 hard set。只要三者都提升，transfer 才站得住。

---

## 我会优先押注的顺序

如果目标是“最有机会做成 high-impact 主线”，我的排序是：

1. `Visual Commitment RL`
   这是最接近当前 frontier、也最容易和已有 RL / reasoning VLM 工作形成直接对话的一条线。
2. `Latent Visual Workspace`
   风险最大，但如果成了，叙事最强，可能是架构层面的新范式。
3. `Counterfactual Intervention Training`
   最容易做成 field-shaping work，因为它天然兼具 benchmark 和 training 两端的价值。
4. `Stateful Video World Model`
   对 video / agent / robotics 很强，但需要更小心地控制 scope。
5. `Evidence-Arbitrating Expert VLM`
   最贴近落地价值，适合如果你想让 proposal 同时显得学术上强且应用上很硬。

## 如果你想把它们压缩成 2 个最值得投的 proposal

### 学术野心最大的双子星

1. `Visual Commitment RL`
   讲的是 multimodal RL 的根问题。
2. `Latent Visual Workspace`
   讲的是 multimodal reasoning 的载体问题。

### 最稳、最容易讲清价值的双子星

1. `Visual Commitment RL`
2. `Evidence-Arbitrating Expert VLM`

---

## 用来定位 proposal 的近期论文线索

这些不是要重复做，而是用来说明“问题已经暴露出来，但还没有被彻底解决”：

1. [`Vision Language Models are Biased`](https://arxiv.org/abs/2505.23941)
   说明 VLM 会强烈依赖 language/context bias，而不是严格依赖视觉证据。
2. [`Unveiling the Compositional Ability Gap in Vision-Language Reasoning Model`](https://arxiv.org/abs/2505.19406)
   说明即使 RL 有帮助，跨模态组合泛化仍然存在明显 gap，且 caption-before-thinking 能带来提升。
3. [`VisualPRM`](https://arxiv.org/abs/2503.10291)
   说明 step-level multimodal reward 确实有价值，但还远没有触到“视觉承诺”这一层。
4. [`SpatialVLM`](https://arxiv.org/abs/2401.12168)
   说明几何与空间 reasoning 不是自然从通用 VLM 里长出来的，而是需要专门表征和数据。
5. [`LLaVA-OneVision`](https://arxiv.org/abs/2408.03326) 与 [`LLaVA-Video`](https://arxiv.org/abs/2410.02713)
   说明 image-to-video transfer 很强，但视频世界状态建模仍远未解决。
6. [`SK-VQA`](https://arxiv.org/abs/2406.19593)
   说明 context-augmented multimodal QA 很重要，但还主要停留在“把上下文喂进去”的阶段。
7. [`HallusionBench`](https://arxiv.org/abs/2310.14566)、[`POPE`](https://arxiv.org/abs/2305.10355)、[`VCD`](https://arxiv.org/abs/2311.16922)
   说明 hallucination 与 shortcut 问题真实存在，但主流解法仍偏 patch 而非 root-cause fix。
8. [`Visual Program Distillation`](https://arxiv.org/abs/2312.03052)
   说明 tool / program supervision 能把复杂视觉推理蒸馏进 VLM，但还没有真正改写推理介质。
9. [`Many-Shot In-Context Learning in Multimodal Foundation Models`](https://arxiv.org/abs/2405.09798)
   说明多模态模型存在很强的 task adaptation 潜力，也意味着未来可以继续往“context compiler / temporary adapter”发展。

## 最后一句判断

如果从第一性原理出发，未来 1-2 年 VLM post-training 最值得做的，不是“再造一个 multimodal DPO 变体”，而是回答这五个更硬的问题：

1. 怎样确保模型真的在看图？
2. 视觉推理到底该在哪种表征里发生？
3. 视频理解如何从看帧升级到维护状态？
4. 多源证据冲突时，模型如何裁决并拒答？
5. 我们怎样训练和评测模型去学习视觉因果，而不是 shortcut？

这五个问题里，只要你真正打穿其中一个，就已经是 high-impact proposal。
