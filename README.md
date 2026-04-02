# LLM/VLM Post-Training: A Survey Through the Lens of Evolution

[**Chinese Version (中文版)**](README-zh.md)

A comprehensive survey that systematically traces the evolution of post-training for Large Language Models (LLMs) and Vision-Language Models (VLMs) from 2017 to 2026.

Unlike traditional surveys that organize by method categories, this work structures the field into **seven problem-driven evolutionary threads**, tracing the _"problem → solution → new problem → improved solution"_ chain within each thread and revealing cross-thread idea flow through explicit references.

## Threads

| # | Thread | Core Question |
|---|--------|---------------|
| T1 | Instruction Following & SFT | How do we teach models to follow human instructions? |
| T2 | Reward Modeling & Preference Optimization | How do we optimize for human preferences beyond supervised imitation? |
| T3 | Safety & Alignment | How do we constrain model behavior without sacrificing capability? |
| T4 | Reasoning & Verification | How do we train models to reason reliably and verify their own outputs? |
| T5 | Multimodal Post-training | How do we extend post-training techniques from language to vision-language models? |
| T6 | Tool Use & Agentic Capabilities | How do we train models to use tools and act as autonomous agents? |
| T7 | Efficiency & Data Engineering | How do we make post-training practical at scale? |

## Key Features

- **Evolutionary perspective**: Tracks how each subfield emerged from concrete problems and evolved through iterative refinement, rather than presenting a static taxonomy.
- **Cross-thread references**: Color-coded arrows and explicit section references connect ideas across threads (e.g., how reward modeling shaped reasoning verification, how SFT pipelines were adopted for multimodal training).
- **Panoramic overview figure**: A TikZ-generated evolution graph visualizes all seven threads on a shared 2017--2026 timeline with cross-thread idea flow.
- **460+ references**: Covers landmark papers from InstructGPT and RLHF to DeepSeek-R1, o3, and beyond.
- **Bilingual**: Full Chinese and English editions compiled from the same bibliography.

## Project Structure

```
.
├── main.tex              # Chinese edition entry point
├── main-en.tex           # English edition entry point
├── preamble.tex          # Chinese LaTeX preamble
├── preamble-en.tex       # English LaTeX preamble
├── references.bib        # Shared bibliography (460+ entries)
├── chapters/             # Chinese chapters
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
├── chapters-en/          # English chapters
│   └── (same structure)
├── figures/
│   └── overview.tex      # Panoramic evolution graph (TikZ)
└── research/             # Background research notes
```

## Build

Requires a TeX distribution with `xelatex` and `bibtex` (e.g., TeX Live 2024+).

**Chinese edition:**

```bash
xelatex main && bibtex main && xelatex main && xelatex main
```

**English edition:**

```bash
xelatex main-en && bibtex main-en && xelatex main-en && xelatex main-en
```

Or use `latexmk`:

```bash
latexmk -xelatex main.tex      # Chinese
latexmk -xelatex main-en.tex   # English
```

## Citation

If you find this survey useful, please cite:

```bibtex
@misc{lin2026posttraining,
  title   = {LLM/VLM Post-Training: A Survey Through the Lens of Evolution},
  author  = {Jinhong Lin},
  year    = {2026},
  note    = {\url{https://github.com/JonesLin/post-training-survey}}
}
```

## License

This work is intended for academic and educational purposes.
