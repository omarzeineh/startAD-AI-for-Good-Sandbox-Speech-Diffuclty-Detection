# Speech Difficulty Detection — startAD AI for Good (Cohort 2)

> Web-based AI tool to triage short audio clips for potential speech and language difficulties.  
> Built by **Team ADUAA (Team 38)** for the **startAD AI for Good Sandbox — Cohort 2** (Ability Challenge).

---

## Overview

This repository delivers a reproducible prototype for **Speech Difficulty Detection** using a **Bi-directional GRU (BiGRU)** over **log‑Mel spectrograms**. It includes:

- **Flask backend API** for on-device or server inference
- **React frontend** with drag‑and‑drop multi-file upload and per-file confidence scores
- **Training & testing scripts** (PyTorch + Torchaudio) for the BiGRU pipeline

Non‑normal predictions are highlighted **in red** on the frontend to aid quick triage. This project supports research and early clinical exploration only—**not** a medical diagnosis tool.

---

## Repository Structure

```
.
├── FlaskBackend_Team38/              # Flask API (BiGRU backend; legacy Transformer available)
├── ReactFrontend_Team38/             # React (Vite) app
├── ModelsTrainingAndTesting_Team38/  # BiGRU training & evaluation scripts
└── README.md
```

> Tip: If you only use BiGRU, you may safely remove Transformer-specific files to keep the repo lean.

---

## Quick Start

### 1) Backend (Flask + PyTorch)

From `FlaskBackend_Team38/` (Windows PowerShell):

```bash
py -m venv venv
. venv/Scripts/activate

# Core deps
pip install flask flask-cors torch torchaudio soundfile matplotlib
```

Start the BiGRU backend:

```bash
python GRUBackEnd.py
```

The API listens on `http://127.0.0.1:5000` by default.

> **CUDA (optional):** if you have a supported NVIDIA GPU, install CUDA wheels. For CUDA 11.8 specifically (as used in our environment):  
> `pip install torch==2.7.1+cu118 torchaudio==2.7.1+cu118 --extra-index-url https://download.pytorch.org/whl/cu118`

### 2) Frontend (React + Vite)

From `ReactFrontend_Team38/`:

```bash
npm install
npm run dev
```

Open the URL printed by Vite (typically `http://127.0.0.1:5173`). Ensure the Flask backend is running.

### 3) (Optional) Train / Test Models

From `ModelsTrainingAndTesting_Team38/`:

```bash
# Train BiGRU
python train_gru_dysarthria.py

# Evaluate on a bigger test set
python test_gru_dysarthria.py
```

Expected dataset layout:

```
dataset_root/
  train/{Normal,Dysarthria}
  val/{Normal,Dysarthria}
  test/{Normal,Dysarthria}
```

---

## API

**POST** `/predict`  
- Content-Type: `multipart/form-data` with one or more `file` fields (`.wav`)  
- Returns: JSON with `{file, label, score}` per input file  
- CORS is enabled for local development

Example:
```json
{
  "results": [
    {"file": "child_01.wav", "label": "Normal", "score": 0.87},
    {"file": "child_02.wav", "label": "Dysarthria", "score": 0.93}
  ]
}
```

---

## Frontend UX

- Drag & drop multiple files
- Per‑file confidence scores
- **Non‑normal** results appear **in red**
- Built for quick clinic triage and parent‑facing demonstrations

---

## Model Notes (BiGRU)

- Features: **log‑Mel spectrograms** from raw audio
- Sequence model: **BiGRU** with a lightweight classification head
- Variable length handling via **packed sequences**
- Metrics & plots (in evaluation): Accuracy, Confusion Matrix, ROC, PR curves

> The model and protocol are intended for research/education. For clinical use, validate on representative pediatric data under professional oversight.

---

## Roadmap

1. **Data & MVP** — collect regional pediatric audio (consented), refine preprocessing, ship a stable MVP  
2. **Validation** — robust protocols, cross‑site evaluation, bias/fairness checks  
3. **Integration** — Arabic/English UI, privacy/consent flows, exportable reports  
4. **Deployment** — cloud and on‑prem options; privacy‑preserving pipelines

---

## Acknowledgments

- Built for the **Ability Pediatric Rehabilitation** challenge under the **startAD AI for Good Sandbox — Cohort 2**  
- Thanks to organizers, mentors, and partners for guidance and feedback

---

## License (MIT)

```
MIT License

Copyright (c) 2025 ADUAA/Team-38

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

### Maintainers

**Team ADUAA (Team 38)** — startAD AI for Good Sandbox, Cohort 2  
Open an issue in this repo for questions or suggestions.
