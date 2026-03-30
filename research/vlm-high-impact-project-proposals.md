# VLM 多模态 High-Impact Project Proposals

这份草稿刻意避开仓库里已有的 VLM proposal 题目，例如 visual commitment、active re-perception、uncertainty decomposition、self-verification、connector bottleneck、difficulty decomposition 等，转而聚焦 5 个新的高影响方向。筛选标准只有三个：

1. 不是只靠 scaling 就能自然解决的问题。
2. 如果做成，会改变 VLM 的能力边界，而不只是刷高 1 个 benchmark。
3. 能从第一性原理往下追问，定位到结构性 root cause。

## 总判断：现在 VLM 真正卡在哪里

如果连续追问 `why`，当前 VLM 的主要瓶颈并不是“模型还不够大”，而是下面 5 类结构性错配：

1. 它把视频当作一堆帧，而不是随时间演化的状态。
2. 它把空间看成 2D pattern matching，而不是可操作的 3D geometry。
3. 它把外部知识当作附加文本，而不是与视觉证据共同裁决答案的证据系统。
4. 它把长上下文 demonstration 当作 prompt stuffing，而不是临时可编译的 task adaptation。
5. 它的 benchmark 仍然允许 shortcut，所以整个领域可能在优化“看起来会看图”，而不是“真的依赖图像推理”。

## 快速优先级

| 方向 | Root cause | 影响力 | 风险 | 适合的人 |
|---|---|---:|---:|---|
| Proposal 1. Stateful Video VLM | 视频被压缩成一次性 token | 很高 | 高 | 想做 long-video / agent / world understanding |
| Proposal 2. Geometry-Grounded VLM | 缺少 metric 级空间表征 | 很高 | 高 | 想做 robotics / 3D / embodied AI |
| Proposal 3. Retrieval-Calibrated Expert VLM | 外部知识与视觉证据缺少仲裁机制 | 很高 | 中 | 想做 medical / science / real-world deployment |
| Proposal 4. Context-to-Adapter VLM | many-shot context 不能转化为临时适配 | 高 | 中 | 想做 efficient adaptation / open ecosystem |
| Proposal 5. Intervention-Centric Multimodal Benchmark | 评测奖励 shortcut 而非真理解 | 很高 | 中 | 想做 benchmark + method + field-shaping work |

---

## Proposal 1. Stateful Video VLM: 从“看很多帧”转向“维护可推理的事件状态”

### 一句话摘要

长视频理解失败，不是因为模型没看够帧，而是因为它没有形成一个可以持续更新、回溯、查询的事件状态系统。

### 第一性原理推导

1. `Why` 长视频难？
   因为 token budget 固定，而视频信息量随时长近似线性增长。
2. `Why` 更长 context 还不够？
   因为注意力会被大量低价值帧稀释，关键因果事件反而更难保留。
3. `Why` 压缩之后仍然不行？
   因为当前压缩大多保留“看到了什么”，却丢掉“什么时候发生、因谁而起、后面会影响什么”。
4. `Why` 这是根因？
   因为现有 video VLM 本质上仍把视频当成 frame bag，而不是 state transition process。

### 核心假设

如果让 VLM 学会显式维护 `event state memory`，并把后续推理建立在“事件状态的演化”而不是“帧级摘要”上，那么它在长视频上的性能会比单纯扩 context 更稳、更便宜，也更可解释。

### 方法草案

1. 设计 `state slots`，每个 slot 表示一个正在演化的事件或实体状态，例如人物目标、物体位置、因果链节点。
2. 引入三类动作：`write`、`update`、`rewind`。模型在观看视频过程中必须决定何时写入新事件、何时更新旧状态、何时回溯验证。
3. 构造 `same frames, different order` 的反事实视频，让模型不能只做静态识别，而必须学习顺序与因果。
4. 对 memory 做 question-conditioned credit assignment，逼迫模型只保留未来问答真正需要的状态。
5. 用小模型先验证 memory 结构本身带来的增益，再迁移到大模型，避免把结论混进“参数更多”的 confound。

### 实验设计

1. 主 benchmark 用 [Video-MME](https://arxiv.org/abs/2405.21075) 和 [MLVU](https://arxiv.org/abs/2406.04264)。
2. 重点看长视频分桶，不只看 overall score。
3. 增加 order-sensitive 和 causal-sensitive stress test，例如同样片段、不同时序、删去关键过渡镜头。
4. 额外评估 memory trace 是否可解释：错误答案时，memory 中是否早已丢掉关键事件。

### 为什么 high impact

长视频是 agent、监控、手术录像、教育录像、会议分析、机器人长期任务理解的共同底层能力。如果这个方向做出来，它不会只影响一个 benchmark，而会影响所有“需要在时间上持续建模世界”的 VLM 系统。

### 主要风险

风险在于 memory 机制可能只是在架构上增加复杂度，却没有真的学到 state abstraction。

### 降风险方案

先做两个最小闭环：

1. 用合成视频验证模型是否真的学会“顺序敏感”而不是更强 OCR。
2. 用可控小游戏环境生成视频，让 ground-truth state 可直接监督。

---

## Proposal 2. Geometry-Grounded VLM: 让 VLM 从“看起来懂空间”变成“真的会算空间”

### 一句话摘要

很多 VLM 在图像问答里像是会空间推理，但它们真正缺的是 metric-aware 的几何表征，所以一旦任务需要可执行的 3D 推理，性能会快速坍塌。

### 第一性原理推导

1. `Why` 当前 VLM 在很多视觉任务上看起来已经很强？
   因为大量 benchmark 只要求语义识别，不要求可执行的空间量化。
2. `Why` 一到机器人、AR、导航、工业场景就不够？
   因为这些任务需要的是“距离、深度、遮挡、可达性、相对尺寸”等可计算关系。
3. `Why` 语言 CoT 不能直接补上？
   因为空间错误主要不是 reasoning format 问题，而是输入表征里根本没有稳定几何量。
4. `Why` 这是根因？
   因为当前 VLM 学到的是 image-text correlation，不是 geometry-faithful world representation。

### 核心假设

如果把 3D geometry 明确提升为中间表征，并让语言推理建立在几何对象与关系图之上，VLM 才可能真正获得可转移的 spatial reasoning 能力。

### 方法草案

1. 先从图像或多视角输入中构建轻量级 `scene graph / point tokens / object-centric geometry tokens`。
2. 所有空间问题都先转换成对几何对象的引用，再进行语言推理，避免模型直接用表面纹理 shortcut。
3. 构造几何反事实数据，例如保持语义基本不变，只改距离、朝向、遮挡或大小关系，迫使模型学习真正的空间不变量。
4. 对输出施加 `geometry consistency loss`，要求答案与中间几何表征一致。

### 实验设计

1. 基础评估可参考 [SpatialVLM](https://arxiv.org/abs/2401.12168) 一类的 3D spatial reasoning 设定。
2. 在合成场景中测量定量误差，例如距离估计误差、相对大小排序错误率、遮挡判断准确率。
3. 在下游场景验证，例如机器人 pick-and-place、室内导航、AR 指令理解。

### 为什么 high impact

一旦空间推理从“描述层”走到“执行层”，VLM 才真正连接到 embodied AI、机器人操作、工业检测和真实世界代理。这是从 demo intelligence 走向 operational intelligence 的关键一步。

### 主要风险

几何中间层可能带来额外噪声，尤其是单图 3D 恢复不稳定时。

### 降风险方案

先做 object-centric 相对几何，再逐步过渡到绝对 metric geometry；先证明“关系正确”再追求“尺度也准”。

---

## Proposal 3. Retrieval-Calibrated Expert VLM: 面向医学/科学的证据仲裁型多模态系统

### 一句话摘要

专家场景下，真正难的不是“图像 + 文本一起输入”，而是视觉证据、外部知识、任务规范三者经常冲突，现有 VLM 缺少一个显式的证据仲裁机制。

### 第一性原理推导

1. `Why` 通用 VLM 到医疗、科研、工业文档里经常掉得很厉害？
   因为这些任务的答案通常不能只从图像里直接读出。
2. `Why` 只做 domain fine-tuning 也不够？
   因为专家知识会变化、会版本化，而且很多关键规则本来就应该留在参数外部。
3. `Why` multimodal RAG 比 text RAG 更难？
   因为视觉所见与检索到的文本证据可能冲突，而现有模型通常没有明确地区分“我看到了什么”和“外部资料说了什么”。
4. `Why` 这是根因？
   因为当前 VLM 没有把答案看作一个由多源证据共同裁决的过程，而是在做 end-to-end next-token imitation。

### 核心假设

如果把视觉发现、检索证据、任务规范拆成三个显式证据层，并训练模型进行跨证据一致性判断与 selective answering，那么专家型多模态系统会显著更可靠。

### 方法草案

1. 模型先生成 `visual findings`，明确区分可见事实和不可见推断。
2. Retriever 只根据 findings 和 query 去抓取规范、论文、病例知识库，而不是把整张图扔给 retriever。
3. 构建 `evidence graph`，边表示支持、冲突、缺失、需要补充证据。
4. 引入 `answer / abstain / request-more-context` 三路决策，而不是强迫模型一定回答。
5. 在训练中加入“冲突证据对”，让模型学会在视觉证据与文本先验矛盾时优先报告不确定性。

### 实验设计

1. 用 [SK-VQA](https://arxiv.org/abs/2406.19593) 这类 context-augmented multimodal setting 做基础评估。
2. 医疗场景可参考 [Med-Gemini](https://arxiv.org/abs/2405.03162) 暴露出来的问题，尤其是 3D CT 报告和临床可接受性。
3. 除了最终准确率，还要评估 evidence attribution、citation faithfulness、校准误差和 abstention quality。

### 为什么 high impact

这是最接近真实落地价值的 VLM 方向之一。医疗、科学助手、专利检索、工业检修、法律图文材料审阅，本质上都不是“识图”，而是“视觉证据参与知识密集型判断”。

### 主要风险

风险在于系统最后退化为“更复杂的 RAG pipeline”，但不产生新的学习机制。

### 降风险方案

关键不是只搭系统，而是把“证据冲突仲裁”做成一个新的训练目标和 benchmark，逼迫模型学习何时相信图像、何时相信检索、何时必须 abstain。

---

## Proposal 4. Context-to-Adapter VLM: 把 many-shot multimodal ICL 变成临时适配能力

### 一句话摘要

many-shot multimodal ICL 说明长上下文里藏着巨大的适配潜力，但现在的用法太粗糙了。真正的机会不是塞更多示例，而是把示例“编译”为临时任务适配器。

### 第一性原理推导

1. `Why` many-shot ICL 值得重视？
   因为它证明模型在不更新参数的情况下，仍然能从大量示例里获得显著增益。
2. `Why` 现在的方法仍然低效？
   因为 demonstration 只是被串接进 prompt，没有被压缩成持久、可复用、任务相关的内部状态。
3. `Why` 这会成为瓶颈？
   因为长 prompt 成本高、延迟高，而且示例一换，模型就得重新“临时读一遍教材”。
4. `Why` 这是根因？
   因为当前 ICL 仍是 retrieval-and-read，而不是 retrieval-and-compile。

### 核心假设

如果把 many-shot multimodal context 显式蒸馏成一个临时 adapter、latent task sketch 或 routing state，模型就能更便宜、更稳定地适应新 domain，同时保留 ICL 的免微调优势。

### 方法草案

1. 先检索大量 demonstration，但不直接把它们完整送入主模型。
2. 用一个 `context compiler` 把这些示例压缩成临时 task adapter，例如 prefix state、LoRA-like ephemeral weights、或 latent task program。
3. 主模型只消费“编译结果 + 少量代表性样例”，而不是完整原始示例。
4. 训练目标直接优化“给定大量示例后，新 query 的 adaptation quality”，而不是普通 next-token loss。
5. 加入 uncertainty-aware example selection，减少无关示例对临时适配器的污染。

### 实验设计

1. 以 [Many-Shot In-Context Learning in Multimodal Foundation Models](https://arxiv.org/abs/2405.09798) 为出发点，关注从 few-shot 到近 2000 例的 scaling 区间。
2. 比较三种成本曲线：原始 many-shot ICL、full fine-tuning、context-to-adapter。
3. 任务覆盖分类、VQA、定位、医疗影像、遥感、分子图像等跨域设置。
4. 重点汇报 accuracy-latency-cost 三维 trade-off，而不是只报一个精度数。

### 为什么 high impact

这条线最有机会改变多模态模型的部署范式。它指向一个现实命题：能否让 VLM 在不重新训练大模型的前提下，快速适配一个新任务、新医院、新工厂、新卫星数据源。

### 主要风险

临时 adapter 可能学到的只是 prompt compression，而不是 task adaptation。

### 降风险方案

做强分布外测试：训练时给的是某类任务 demonstration，测试时要求迁移到结构相近但表面形式不同的新任务，验证其是否真的学到了可迁移 task program。

---

## Proposal 5. Intervention-Centric Multimodal Benchmark and Training: 重新定义“真的会看图”

### 一句话摘要

如果 benchmark 本身奖励 shortcut，那么整个领域都会被带偏。高影响的一步不是再堆一个更大的模型，而是发明一套让“真假多模态能力”彻底分家的评测与训练框架。

### 第一性原理推导

1. `Why` 现在很多 VLM 分数看起来很高，但一到真实场景就脆？
   因为 benchmark 往往允许语言先验、OCR shortcut、选项猜测、数据泄漏等旁门左道。
2. `Why` 这比单个模型失败更严重？
   因为 benchmark 决定整个社区在优化什么。
3. `Why` 修修数据集还不够？
   因为只要没有 intervention，模型仍可能通过统计相关性而不是因果依赖来做题。
4. `Why` 这是根因？
   因为当前领域对“模型到底因为什么答对”缺乏强识别力。

### 核心假设

只有当 benchmark 包含系统性的图像干预、文本干预、检索干预、选项干预，并能明确测出“答案是否随真正因果因素而改变”时，我们才能开始真正训练 grounded VLM。

### 方法草案

1. 以 [MMMU](https://arxiv.org/abs/2311.16502) 为 base setting，但进一步采用 [MMMU-Pro](https://arxiv.org/abs/2409.02813) 那种更严格的思路，系统消除 text-only shortcut。
2. 为每个样本构造最少一组 intervention pair：
   图像改了但文本不改；
   文本改了但图像不改；
   检索上下文改了但核心视觉事实不改；
   选项扰动使猜测失效。
3. 指标不只看 accuracy，还看 `intervention consistency`、`causal sensitivity`、`spurious invariance`。
4. 在 benchmark 之上再做训练方法：对同一问题的 intervention pair 施加对比式 grounded training，让模型学会只对真正因果证据敏感。

### 实验设计

1. 先在 MMMU / MMMU-Pro 上复现实证，确认现有模型的 shortcut 类型。
2. 再构建新的 intervention split，并测不同模型在因果敏感性上的排名是否发生重排。
3. 用 [mDPO](https://arxiv.org/abs/2406.11839) 暴露出的 `unconditional preference problem` 作为训练侧动机，测试新训练目标能否减少“忽略图像条件”的现象。

### 为什么 high impact

这类工作有 field-shaping potential。它不仅能发 benchmark paper，还能影响后续 alignment、data curation、RL、RAG、reasoning 的评价标准。高影响论文很多时候不是“最强模型”，而是“改写了别人怎么证明自己强”。

### 主要风险

如果只停留在 benchmark，容易被看成“评测工程”。

### 降风险方案

必须把 benchmark 和训练方法绑定起来，形成一个闭环：先用 intervention 暴露 shortcut，再用 intervention-aware training 消掉 shortcut。

---

## 我最推荐的 3 个切入点

如果你要的是“高影响 + 能讲出非常强的第一性原理故事”，我建议优先考虑下面三个：

1. `Proposal 1`：适合打 long-video / agent / world understanding，大问题、大空间，但工程难度高。
2. `Proposal 3`：最接近现实价值，最容易讲 high-impact application story，尤其适合医疗、科学、工业知识系统。
3. `Proposal 5`：最有可能做出 field-shaping 工作，尤其适合想做 benchmark + method + theory positioning 的路线。

## 如果要进一步收敛成真正可投的 proposal

下一步可以按三种风格继续往下写：

1. `PhD application version`：更强调 big question、novelty、3-year roadmap。
2. `Grant / fellowship version`：更强调 societal impact、deliverables、risk mitigation。
3. `Paper proposal version`：更强调具体方法、实验、benchmark、ablation。

## 参考依据

这些 proposal 主要基于近年的第一手文献和 benchmark 信号，而不是泛泛的趋势判断：

1. [MMMU: A Massive Multi-discipline Multimodal Understanding and Reasoning Benchmark](https://arxiv.org/abs/2311.16502)
2. [MMMU-Pro: A More Robust Multi-discipline Multimodal Understanding Benchmark](https://arxiv.org/abs/2409.02813)
3. [Video-MME: The First-Ever Comprehensive Evaluation Benchmark of Multi-modal LLMs in Video Analysis](https://arxiv.org/abs/2405.21075)
4. [MLVU: Benchmarking Multi-task Long Video Understanding](https://arxiv.org/abs/2406.04264)
5. [SpatialVLM: Endowing Vision-Language Models with Spatial Reasoning Capabilities](https://arxiv.org/abs/2401.12168)
6. [Many-Shot In-Context Learning in Multimodal Foundation Models](https://arxiv.org/abs/2405.09798)
7. [SK-VQA: Synthetic Knowledge Generation at Scale for Training Context-Augmented Multimodal LLMs](https://arxiv.org/abs/2406.19593)
8. [Advancing Multimodal Medical Capabilities of Gemini](https://arxiv.org/abs/2405.03162)
9. [mDPO: Conditional Preference Optimization for Multimodal Large Language Models](https://arxiv.org/abs/2406.11839)
