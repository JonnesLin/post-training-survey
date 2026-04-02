# LLM/VLM Post-Training：从演化视角审视大语言模型与视觉语言模型的后训练

[**English Version**](README.md)

本书从演化视角系统地梳理了大语言模型（LLM）和视觉语言模型（VLM）post-training 领域的发展脉络（2017--2026）。

不同于传统 survey 按方法分类的组织方式，本书将该领域划分为**七条问题驱动的演化线程（thread）**，在每条线程内追踪 _"问题 → 解决方案 → 新问题 → 改进方案"_ 的发展链条，并通过跨线程的交叉引用揭示不同研究方向之间的思想流动。

## 七条演化线程

| # | 线程 | 核心问题 |
|---|------|----------|
| T1 | Instruction Following & SFT | 如何教会模型遵循人类指令？ |
| T2 | Reward Modeling & Preference Optimization | 如何超越监督模仿，优化人类偏好？ |
| T3 | Safety & Alignment | 如何在不牺牲能力的前提下约束模型行为？ |
| T4 | Reasoning & Verification | 如何训练模型可靠地推理并验证自身输出？ |
| T5 | Multimodal Post-training | 如何将 post-training 技术从语言模型扩展到视觉语言模型？ |
| T6 | Tool Use & Agentic Capabilities | 如何训练模型使用工具并作为自主智能体行动？ |
| T7 | Efficiency & Data Engineering | 如何让 post-training 在规模化场景下切实可行？ |

## 主要特色

- **演化视角**：追踪每个子领域如何从具体问题出发、经过迭代改进而演化，而非呈现静态分类。
- **跨线程引用**：通过彩色箭头和显式章节引用连接不同线程之间的思想流动（例如 reward modeling 如何影响 reasoning verification，SFT pipeline 如何被 multimodal 训练采纳）。
- **全景演化图**：使用 TikZ 生成的演化图在 2017--2026 的共享时间轴上可视化七条线程及跨线程的思想迁移。
- **460+ 篇参考文献**：从 InstructGPT 和 RLHF 到 DeepSeek-R1、o3 及更前沿的工作。
- **中英双语**：完整的中文版和英文版，共享同一份参考文献库。

## 项目结构

```
.
├── main.tex              # 中文版入口
├── main-en.tex           # 英文版入口
├── preamble.tex          # 中文 LaTeX 导言区
├── preamble-en.tex       # 英文 LaTeX 导言区
├── references.bib        # 共享参考文献（460+ 条目）
├── chapters/             # 中文章节
│   ├── 00-preface.tex
│   ├── 00-introduction.tex
│   ├── 01-sft.tex
│   ├── 02-preference.tex
│   ├── 03-safety.tex
│   ├── 04-reasoning.tex
│   ├── 05-multimodal.tex
│   ├── 06-agentic.tex
│   ├── 07-efficiency.tex
│   └── 08-conclusion.tex
├── chapters-en/          # 英文章节
│   └── (同上结构)
├── figures/
│   └── overview.tex      # 全景演化图（TikZ）
└── research/             # 背景研究笔记
```

## 编译

需要支持 `xelatex` 和 `bibtex` 的 TeX 发行版（如 TeX Live 2024+）。

**中文版：**

```bash
xelatex main && bibtex main && xelatex main && xelatex main
```

**英文版：**

```bash
xelatex main-en && bibtex main-en && xelatex main-en && xelatex main-en
```

或使用 `latexmk`：

```bash
latexmk -xelatex main.tex      # 中文版
latexmk -xelatex main-en.tex   # 英文版
```

## 引用

如果本书对您的研究有所帮助，请引用：

```bibtex
@misc{lin2026posttraining,
  title   = {LLM/VLM Post-Training: A Survey Through the Lens of Evolution},
  author  = {Jinhong Lin},
  year    = {2026},
  note    = {\url{https://github.com/JonesLin/post-training-survey}}
}
```

## 许可

本作品仅供学术和教育用途。
