# CASEset — Claude Code Instructions

## Project Overview

**CASEset** is a gaze estimation dataset and training infrastructure for **CASE (Context-Aware Screen-based Estimation of Gaze)**. The core problem: existing gaze datasets capture webcam footage but not the screen content users were viewing, preventing models from learning visual interface context.

The solution is a synchronized 3-stream data collection pipeline (webcam + screenshots + Tobii ground truth) paired with a FAZE-CCT hybrid model that uses on-screen context to improve webcam-based gaze estimation to near-hardware-level accuracy.

**Team:** Max Tran, Pepper (you), Lisa (advisor)
**Funding:** CSE Multidisciplinary Research Program
**Target completion:** Fall 2026

---

## Current Focus

Building the data collection infrastructure:
- Timestamp alignment across three concurrent streams (<50ms precision)
- Tobii Pro Fusion SDK integration
- OpenCV video capture with Python threading
- Pilot dataset collection and validation

---

## Architecture

### Data Collection Pipeline
Three concurrent streams, sub-50ms synchronized:
1. **Webcam frames** — face/eye appearance via OpenCV
2. **Desktop screenshots** — visual context (what the user sees)
3. **Gaze labels** — ground truth from Tobii Pro Fusion eye tracker

### FAZE-CCT Hybrid Model
- **Stage 1 — FAZE DT-ED:** Processes webcam frames → normalized gaze vectors
- **Stage 2 — Coordinate Translator:** Maps gaze vectors → tentative screen coordinates
- **Stage 3 — CCT (Compact Convolutional Transformer):** Refines predictions using a 400×400 screenshot patch centered on the tentative location + optional recent click history

Knowledge distillation: Tobii (expensive hardware) teaches the webcam-only model.

---

## Project Structure

```
CASEset/
├── collect/        # Data collection modules (webcam, screen, Tobii)
├── configs/        # YAML configs for collection & training
├── data/           # Raw and processed datasets
├── docs/           # Documentation
├── evaluation/     # Evaluation scripts and metrics
├── models/         # Model architectures and weights
├── notebooks/      # Jupyter notebooks for analysis
├── outputs/
│   ├── checkpoints/
│   ├── logs/
│   └── results/
├── scripts/        # Utility and automation scripts
├── tests/          # Unit and integration tests
└── training/       # Training pipelines and utilities
```

---

## Tech Stack

- **Language:** Python
- **CV/Capture:** OpenCV, Python `threading`
- **Eye Tracker:** Tobii Pro Fusion (Tobii SDK)
- **ML Framework:** PyTorch
- **Model Bases:** FAZE DT-ED, Compact Convolutional Transformer (CCT)
- **Notebooks:** Jupyter

---

## Key Constraints

- **Synchronization:** All three streams must be captured within <50ms of each other. Timestamp alignment is critical — log hardware timestamps from the Tobii SDK, not system time alone.
- **Privacy:** On-device inference is a hard requirement for defense applicability. No cloud processing of gaze or screen data.
- **Hardware dependency:** Tobii Pro Fusion required for ground truth collection. Webcam-only inference is the deployment target.

---

## Development Notes

- The team is learning PyTorch and transformer architectures alongside development — keep model code well-commented and readable.
- Pilot datasets are for pipeline validation, not final training. Don't over-engineer data schemas before the pipeline is stable.
- Tests live in `tests/`. Write tests for synchronization logic and timestamp alignment — these are the most failure-prone components.

---

## Git Rules

**Never commit or push.** Only the user manages all git commits and pushes. Do not run `git commit`, `git push`, or any command that writes to the repository history — even if asked to "save" or "finalize" changes.
