<h1 align="center">CASEset</h1>

<p align="center"><img src=".gitbook/assets/CASEset_Ezekiel-Mitchell_2025.png" alt="CASEset"></p>

<h2 align="center">Overview</h2>

<strong>CASEset</strong> is the dataset and training infrastructure for <strong>CASE (Context-Aware Screen-based Estimation of Gaze)</strong>. CASEset captures synchronized webcam frames, desktop screenshots, and high-precision gaze labels to enable models that reason about what users are looking at on screen.

<h3 align="center">The Problem</h3>

High-accuracy gaze tracking currently requires expensive specialized hardware. Webcam-based approaches are cheaper but lack access to on-screen context, which limits accuracy for screen-targeted tasks.

<h3 align="center">The Key Insight</h3>

Existing large-scale gaze datasets capture appearance but not the screen content users view. CASEset pairs synchronized screen content with gaze to enable context-aware models that use visual saliency and UI structure.

<h2 align="center">Technical Architecture</h2>

<h3 align="center">Dataset Pipeline</h3>

The collection infrastructure synchronizes three streams:

- Webcam frames (face/eye appearance)
- Desktop screenshots (visual context)
- High-precision gaze (Tobii Pro Fusion)

Key requirements:
- Sub-50ms synchronization across modalities
- Temporal interaction sequences during natural tasks
- Diverse interface contexts (browsing, documents, apps)

<h3 align="center">FAZE-CCT Hybrid Model</h3>

A high-level pipeline:

- Stage 1: FAZE DT-ED processes webcam frames to extract normalized gaze vectors.
- Stage 2: Coordinate Translator maps gaze vectors to tentative screen coordinates.
- Stage 3: CCT (Compact Convolutional Transformer) refines predictions using a 400×400 screenshot patch centered on the tentative location and optional recent click history.

<h2 align="center">Knowledge Distillation Approach</h2>

CASEset enables distillation where expensive hardware (Tobii) teaches webcam-only models to improve accuracy while supporting on-device, privacy-preserving inference.

<h2 align="center">Roadmap</h2>

Target milestones through Fall 2026; high-level phases include infrastructure & pilot data, full data collection & initial model, model refinement & optimization, and final evaluation & thesis completion.

<h2 align="center">Project Structure</h2>

<pre>
CASEset/
├── README.md              # This file
├── SUMMARY.md             # GitBook table of contents
├── references.md          # Bibliography and citations
├── collect/               # Data collection modules (webcam, screen, Tobii)
├── configs/               # YAML configs for collection & training
├── data/                  # Raw and processed datasets
├── docs/                  # Additional documentation
├── evaluation/            # Model evaluation scripts and metrics
├── models/                # Model architectures and weights
├── notebooks/             # Jupyter notebooks for analysis
├── outputs/               # Generated outputs
│   ├── checkpoints/       # Model checkpoints
│   ├── logs/              # Training and experiment logs
│   └── results/           # Evaluation results
├── scripts/               # Utility and automation scripts
├── tests/                 # Unit and integration tests
└── training/              # Training pipelines and utilities
</pre>

<h2 align="center">Citation</h2>

If you use CASEset in your research, please cite:

```bibtex
@inproceedings{tran2024case,
  title={CASE: Context Aware Screen-Based Estimation of Gaze},
  author={Tran, M. and Milkowski, L.},
  booktitle={Eighth IEEE International Conference on Robotic Computing (IRC)},
  pages={112--113},
  year={2024},
  organization={IEEE}
}
```

<h2 align="center">License</h2>

[License information to be added]

<h2 align="center">Acknowledgments</h2>

S.U. Fall 2025 Undergraduate Research Showcase
