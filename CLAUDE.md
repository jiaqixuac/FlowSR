# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is the **project page for the FlowSR paper** (ICCV 2025: *Fast Image Super-Resolution via Consistency Rectified Flow*), **not a software codebase**. It contains only:

- `README.md` — the rendered project page (paper links, method summary, benchmark tables, qualitative results)
- `assets/` — figures referenced by the README (`teaser.jpg`, `overview.jpg`, `consistency.jpg`, `qualitative.jpg`, `results_*.jpg`)
- `LICENSE` — CC BY-NC 4.0 (non-commercial academic use)

There is **no build, lint, test, or run workflow** here, and there are no source files. Do not invent commands or scaffold a project unless explicitly asked. The official training/inference code is *not* released (company open-source policy); the README points to a community-maintained, inference-only reproduction at `github.com/springXIACJ/FlowSR` and weights at `huggingface.co/chunjie-spring/FlowSR`. Those live in separate repositories — they are not part of this one.

This directory is also not a git repository.

## Working here

Nearly all edits will be to `README.md` (or its embedded benchmark tables and asset references) and to the images in `assets/`. When editing:

- The README is GitHub-flavored Markdown with inline HTML (`<p align="center">`, `<img>`, `<details>`, `<sub>`, `<ins>`). Preserve this HTML styling — the page relies on it for centered figures and collapsible sections.
- Image paths are relative (`assets/...`). If you add or rename a figure, update both the file and every `<img src>` referencing it.
- Benchmark tables use a fixed convention: **bold** = best, <ins>underline</ins> = second best, per column. Keep that convention consistent when adding rows/methods, and keep the arrow direction markers (↑ better-high, ↓ better-low) in the headers.
- Author affiliations, the BibTeX block, and the resource link table must stay in sync with the paper if numbers or links change.

## Method context (for understanding README edits)

FlowSR is a one-step real-world super-resolution model on a Stable Diffusion 2.1 backbone. Key terms that recur in the README and should be used consistently:

- **Rectified SR flow** — linear interpolation `X_t = (1−t)·X_HR + t·X_LR` with a learned velocity field; inference is a single Euler step from the LR image: `X̂_HR = X_LR − v_θ(X_LR, 1)`.
- **HR-regularized consistency** — distillation term anchoring the one-step prediction to the true HR target rather than only the teacher.
- **Fast–slow time scheduling** — adjacent timesteps drawn from two schedulers (few-step "fast", 1000-step "slow").
- Two training stages: (1) SR-flow pre-training (ℓ₂ + LPIPS), (2) consistency distillation (+ GAN + CLIP-IQA losses).
