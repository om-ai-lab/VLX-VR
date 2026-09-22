<p align="center">
  <img src="assets/logo-small.png" alt="VLX-VR logo" width="180">
</p>

<h1 align="center">VLX-VR</h1>

<h3 align="center">Agentic-Aware Video Reasoning: Think–Memory–Observation</h3>

<p align="center">
  English | <a href="README_zh.md">中文</a>
</p>

<p align="center">
  <a href="https://x.com/OmAI_lab">
    <img alt="X" src="https://img.shields.io/badge/%F0%9F%93%A3%20X-Follow%20%40OmAI_lab-000000">
  </a>
  <a href="https://www.youtube.com/@OmAILab_global">
    <img alt="YouTube" src="https://img.shields.io/badge/%F0%9F%93%A3%20YouTube-Subscribe%20%40OmAI%20lab-FF0000">
  </a>
  <a href="https://discord.gg/SEVNjyXPef">
    <img alt="Discord" src="https://img.shields.io/badge/%F0%9F%93%A3%20Discord-Join%20%40OmAI%20lab-5865F2">
  </a>
  <br>
  <a href="https://arxiv.org/abs/2609.09985">
    <img alt="arXiv" src="https://img.shields.io/badge/arXiv-2609.09985-b31b1b">
  </a>
  <a href="https://www.youtube.com/watch?v=paqyRLzPcbw">
    <img alt="Overview video" src="https://img.shields.io/badge/%F0%9F%93%BA%20Video-Watch%20Overview-FF0000">
  </a>
  <a href="https://om-agent.com/">
    <img alt="Try VLX" src="https://img.shields.io/badge/%F0%9F%9A%80%20Demo-Try%20Now-16a34a">
  </a>
</p>

<p align="center"><sub>Overview video: VLX-VR for agentic-aware video reasoning</sub></p>

<p align="center">
  <a href="https://www.youtube.com/watch?v=paqyRLzPcbw">
    <img src="https://img.youtube.com/vi/paqyRLzPcbw/maxresdefault.jpg" alt="VLX-VR overview video" width="720">
  </a>
</p>

<p align="center">📺 HD version: <a href="https://www.youtube.com/watch?v=paqyRLzPcbw">Watch on YouTube</a></p>

**VLX-VR** is an agentic-aware video reasoning model trained inside a video reasoning framework defined by a **Think–Memory–Observation** loop. It targets real-world video analysis where visual, audio, textual, and temporal evidence is scattered across short and long videos, and where fixed-context, single-pass VideoQA is not enough.

Instead of locking the video context before reasoning begins, VLX-VR learns to decide what evidence is needed, invoke `read_memory` or `write_memory`, incorporate the returned Observation, and decide whether to continue or produce the task output.

> [!TIP]
>
> **🚀 Try [VLX](https://om-agent.com/)** and explore how Om AI models understand, reason over, and interact with the multimodal world.

## Community

Join the VLX community to connect with developers, explore applications, share feedback, and shape the future of multimodal AI.

<table align="center">
  <thead>
    <tr>
      <th><div align="center">Official WeChat</div></th>
      <th><div align="center">Discord Community</div></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">
        <img src="assets/WeChat.png" alt="VLX official WeChat QR code" width="220">
      </td>
      <td align="center">
        <a href="https://discord.gg/c3BNhbcyd">
          <img src="assets/Discord.png" alt="VLX Discord community QR code" width="220">
        </a>
      </td>
    </tr>
  </tbody>
</table>

For technical support, partnerships, and community inquiries, contact us at **[marketing@hzlh.com](mailto:marketing@hzlh.com)**.

## Updates

- **[2026-09-22]** 🔥 Added three case demos ([assets/demos/](assets/demos/)).
- **[2026-09-20]** 🔥🔥🔥 GitHub package drafted: README (EN/ZH), overview assets, and a MINERVA result notebook.
- **[2026-09]** Paper released: [VLX-VR: An Agentic-Aware Video Reasoning Model](https://arxiv.org/abs/2609.09985) (`arXiv:2609.09985`).
- **[2026-09]** Overview video published: [Watch on YouTube](https://www.youtube.com/watch?v=paqyRLzPcbw).

## Overview

<p align="center">
  <img src="assets/method_framework.png" alt="Think–Memory–Observation loop of VLX-VR" width="100%">
</p>

<p align="center"><em>Figure 1. Framework-defined Think–Memory–Observation loop used to train and run VLX-VR.</em></p>

Modern video large language models are strong at describing clips and answering Visual QA from a fixed sampled context. Real-world event analysis is harder:

- **Adaptive evidence acquisition:** revisit earlier moments, seek missing cues, or resolve conflicts after an initial observation.
- **Multimodal grounding:** combine frames, audio, OCR/text overlays, and timestamps rather than relying on a single modality.
- **State maintenance:** keep intermediate hypotheses and observations across multi-step reasoning.
- **Termination control:** stop when evidence is sufficient, instead of looping forever or answering too early.

Many pipelines still follow:

```text
video + question -> fixed sampled context -> single-pass answer
```

VLX-VR changes the task to:

```text
video + instruction -> Think -> Memory(read/write) -> Observation -> continue or answer
```

This makes evidence acquisition, memory use, and termination part of the model's learned reasoning policy, not only an external prompt wrapper.

## Highlights

- **Agentic-aware video reasoning:** trained to operate inside a Think–Memory–Observation loop rather than only sitting behind an external agent controller.
- **Direct multimodal memory access:** Memory exposes `read_memory` / `write_memory`; VLX-VR selects the operation from the current reasoning state.
- **Strong MINERVA result:** **78.79%** accuracy among models in our comparison.
- **Stable across duration bins:** **76.70% / 78.73% / 80.92%** under MINERVA's original three duration groups; CDAV = **2.97 pp²**.
- **Checkable process:** on correctly answered samples, **96.20%** of reasoning traces agree with MINERVA reference traces and the evidence they describe; about **75.80%** of all samples satisfy both answer correctness and this evidence-grounded criterion.

## Problem Setting

People understand videos by locating relevant moments, comparing states before and after an event, reading text in a scene, and relating spoken content to visual actions. Supporting such reasoning requires more than recognizing objects or isolated frames.

In many standard VideoQA pipelines, models such as Video-LLaVA, Qwen3-VL, and VideoLLaMA 3 receive sampled frames or clips as a fixed input and produce an output in one pass. Because evidence is determined before reasoning begins, the model cannot adaptively acquire new evidence after detecting missing, ambiguous, or conflicting information.

Existing video agents move closer through iterative gathering, temporal localization, and memory queries. However, placing a general-purpose VLM inside an external loop without training it for that role can still fail: the model may request redundant evidence, forget intermediate states, or terminate too early.

VLX-VR focuses on a core question:

> Can a video reasoning model be trained not merely to participate in an agentic pipeline, but to learn evidence acquisition, memory use, and termination as integral parts of its reasoning process?

VLX-VR's answer is:

> Train an agentic-aware model inside a Think–Memory–Observation loop with direct multimodal-memory access.

## Think–Memory–Observation Loop

At reasoning step *t*, VLX-VR maintains a state *s<sub>t</sub>* that includes the task instruction, observed evidence, multimodal content stored in memory, current output hypotheses, and unresolved uncertainty.

The loop has three stages:

1. **Think:** interpret the task, propose the next evidence need, and estimate whether current evidence is sufficient.
2. **Memory:** invoke `read_memory` or `write_memory` to retrieve multimodal evidence or retain intermediate state.
3. **Observation:** return the result of the Memory operation to VLX-VR for the next decision.

When evidence is sufficient, VLX-VR produces the final output with supporting evidence. When evidence is insufficient or conflicting, it returns to Think and starts another Memory step.

```text
s_0 = initialize(video, instruction)
while not stop(s_t):
    think_t, call_t = VLX-VR(s_t)
    obs_t = execute(call_t)          # call_t ∈ {read_memory, write_memory}
    s_{t+1} = update(s_t, obs_t)
output = VLX-VR(s_t)
```

Multimodal memory is an external and inspectable reasoning state rather than only a cache of textual summaries. Video, audio, supporting evidence, Observations, and intermediate states can be read, written, recorded, and reused across steps.

## Evaluation: MINERVA

We evaluate primarily on [MINERVA](https://arxiv.org/abs/2505.00681) because it combines broad temporal coverage with human-annotated reasoning traces. Each question includes a video, a question, five choices, and a reference reasoning trace, making it suitable for scoring both answer accuracy and evidence-grounded process quality.

### Overall Accuracy

| Model | Accuracy (%) | Δ vs. VLX-VR (pp) | Source |
| --- | ---: | ---: | --- |
| **VLX-VR** | **78.79** | — | [This work](https://arxiv.org/abs/2609.09985) |
| Seed2.1 Pro | 70.70 | -8.09 | [Seed2.1](https://seed.bytedance.com/en/seed2_1) |
| Gemini 3.5 Flash | 68.60 | -10.19 | [Seed2.1](https://seed.bytedance.com/en/seed2_1) |
| Gemini 2.5 Pro Thinking | 66.20 | -12.59 | [MINERVA](https://arxiv.org/abs/2505.00681) |
| Seed2.1 Turbo | 65.90 | -12.89 | [Seed2.1](https://seed.bytedance.com/en/seed2_1) |
| Gemini 3.1 Pro | 63.50 | -15.29 | [Seed2.1](https://seed.bytedance.com/en/seed2_1) |
| GPT-4.1 | 53.99 | -24.80 | [MINERVA](https://arxiv.org/abs/2505.00681) |
| GPT-4o | 45.54 | -33.25 | [MINERVA](https://arxiv.org/abs/2505.00681) |
| OpenAI o1 | 43.48 | -35.31 | [MINERVA](https://arxiv.org/abs/2505.00681) |
| Claude 3.5 Sonnet v2 | 31.28 | -47.51 | [MINERVA](https://arxiv.org/abs/2505.00681) |
| Human | 92.54 | +13.75 | [MINERVA](https://arxiv.org/abs/2505.00681) |
| Random | 20.00 | -58.79 | [MINERVA](https://arxiv.org/abs/2505.00681) |

### Accuracy across Video Durations

<p align="center">
  <img src="assets/minerva_length_vlxvr.png" alt="MINERVA accuracy by duration" width="90%">
</p>

<p align="center"><em>Figure 2. Accuracy across MINERVA duration groups. VLX-VR stays strong as duration increases under the original three-bin grouping.</em></p>

| Model | Below 5 min | 5–15 min | Above 15 min |
| --- | ---: | ---: | ---: |
| **VLX-VR** | **76.70** | **78.73** | **80.92** |
| Gemini 2.5 Pro Thinking | 68.87 | 66.84 | 57.97 |
| GPT-4.1 | 58.84 | 54.79 | 47.25 |
| OpenAI o1 | 48.28 | 41.45 | 40.38 |
| Claude 3.5 Sonnet v2 | 40.90 | 33.68 | 28.30 |

Under the original MINERVA grouping, VLX-VR has mean accuracy **78.78%**, CDAV **2.97 pp²**, std **1.72 pp**, and range **4.22 pp**. Public baselines decline with duration; VLX-VR does not under this grouping.

A refined split of the long-video bin shows the Above 15 min result is driven mainly by **15–30 min (83.40%)**, while **Above 30 min** is **74.75%**. Performance remains broadly stable rather than collapsing on longer videos.

### Skill Profile

| Skill | Accuracy (%) |
| --- | ---: |
| Reading | 90.76 |
| Situational awareness | 90.32 |
| Temporal reasoning | 85.95 |
| Numerical reasoning | 85.71 |
| Event occurrence | 82.81 |
| Object recognition | 82.01 |
| Goal reasoning | 76.92 |
| Listening | 75.19 |
| Counterfactual reasoning | 74.19 |
| Spatial perception | 73.47 |
| Cause and effect | 72.73 |
| State changes | 69.23 |
| Counting | 66.67 |

VLX-VR is strongest on reading, situational awareness, temporal reasoning, and numerical reasoning. Counting, state changes, causal reasoning, and spatial perception remain challenging.

### Reference-trace Agreement

On correctly answered samples, **96.20%** of VLX-VR reasoning traces are consistent with MINERVA reference traces and the evidence they describe. Across the full set:

**78.79% × 96.20% ≈ 75.80%**

samples satisfy both answer correctness and this evidence-grounded trace criterion. This joint rate is still higher than Seed2.1 Pro's answer-only accuracy (70.70%), though the criteria differ.

## Case Demos

Three real side-by-side comparisons of **VLX-VR** and **Gemini 3.1 Pro** on the same user prompts and public web footage, focusing on how each model grounds evidence, timestamps, and final answers.

<table>
  <tr>
    <td width="33%" align="center" valign="top">
      <a href="assets/demos/case1-hockey-score.mp4">
        <img src="assets/demos/case1-hockey-score.jpg" alt="Case 1: hockey counterfactual score" width="100%">
      </a>
      <br>
      <b>Case 1 · Counterfactual score</b><br>
      <sub>Hockey: if the green player had scored at 02:22, what would the score be?</sub><br>
      <a href="assets/demos/case1-hockey-score.mp4">▶️ Watch MP4</a>
    </td>
    <td width="33%" align="center" valign="top">
      <a href="assets/demos/case2-chess-check-turns.mp4">
        <img src="assets/demos/case2-chess-check-turns.jpg" alt="Case 2: chess temporal counting" width="100%">
      </a>
      <br>
      <b>Case 2 · Temporal counting</b><br>
      <sub>Chess: how many white turns between the first check and the end of the game?</sub><br>
      <a href="assets/demos/case2-chess-check-turns.mp4">▶️ Watch MP4</a>
    </td>
    <td width="33%" align="center" valign="top">
      <a href="assets/demos/case3-basketball-anomaly.mp4">
        <img src="assets/demos/case3-basketball-anomaly.jpg" alt="Case 3: basketball anomaly timestamps" width="100%">
      </a>
      <br>
      <b>Case 3 · Anomaly + timestamps</b><br>
      <sub>Gym: find out-of-place actions on a basketball court and list second-level timestamps.</sub><br>
      <a href="assets/demos/case3-basketball-anomaly.mp4">▶️ Watch MP4</a>
    </td>
  </tr>
</table>

<details>
<summary><b>What each case highlights</b></summary>

- **Case 1 (hockey / counterfactual):** both models can reach the correct option, but VLX-VR keeps an evidence trail (scoreboard OCR → jersey–team binding → shot at 02:22 → hypothetical 2-1). Gemini 3.1 Pro often collapses this into a short correct sentence without showing how team names and colors were verified.
- **Case 2 (chess / temporal):** VLX-VR counts white’s two post-check moves with aligned timestamps (~215.7s first check → ~223.6s / ~228.1s white turns → ~229.7s mate). Missing a single intervening move flips the MCQ answer; Gemini 3.1 Pro is prone to drop one turn.
- **Case 3 (basketball court / anomaly):** the hard part is listing soccer-like actions *on a basketball court* person-by-person with second-level times, not inventing a global story (e.g. “video played in reverse”). VLX-VR reports grounded intervals; Gemini 3.1 Pro may over-commit to a false global hypothesis.

</details>

Files live under [`assets/demos/`](assets/demos/). Test clips are public-web footage.

## Notebook

See [`notebooks/vlx_vr_minerva_overview.ipynb`](notebooks/vlx_vr_minerva_overview.ipynb) — **Coming soon**.

## Open-Source Status

| Item | Status |
| --- | --- |
| Paper | Released ([arXiv:2609.09985](https://arxiv.org/abs/2609.09985)) |
| Overview video | Released ([YouTube](https://www.youtube.com/watch?v=paqyRLzPcbw)) |
| Case demos | Released ([assets/demos/](assets/demos/)) |
| README + notebook package | This repository draft |

## Why VLX-VR

- Compared with a fixed-context VideoQA model, VLX-VR can adaptively acquire and reassess evidence during reasoning.
- Compared with an untrained VLM inside an external agent loop, VLX-VR learns evidence acquisition, memory use, and termination as policy behaviors.
- Compared with answer-only VideoQA evaluation, MINERVA-style traces let us check whether correct answers are also evidence-grounded.
- Compared with public long-video curves that drop with duration, VLX-VR remains strong under MINERVA's original three duration groups.

## Technology Lineage

Our team has spent years building multimodal perception and reasoning systems, with open-source projects such as [OmDet](https://github.com/om-ai-lab/OmDet), [VLM-R1](https://github.com/om-ai-lab/VLM-R1), [VLX-Seek](https://github.com/om-ai-lab/VLX-Seek), [VLX-Flow](https://github.com/om-ai-lab/VLX-Flow), and [VLX-Go](https://github.com/om-ai-lab/VLX-Go). VLX-VR extends this line from perception and streaming understanding toward agentic-aware video reasoning with explicit multimodal memory control.

## Citation

```bibtex
@article{vlxvr2026,
  title   = {VLX-VR: An Agentic-Aware Video Reasoning Model},
  author  = {{Om AI Lab}},
  journal = {arXiv preprint arXiv:2609.09985},
  year    = {2026}
}
```

## License

See the repository license file for terms covering code and model weights.
