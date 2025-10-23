# Speech Difficulty Detection
startAD AI for Good — Cohort 2 (Ability Challenge)

> Web-based AI tool that analyzes short audio clips to flag potential speech and language difficulties.  
> Built by **Team ADUAA (Team 38)** for the **startAD AI for Good Sandbox — Cohort 2**.

---

## Why this matters

Speech-Language Pathologists (SLPs) need faster, more consistent screening tools—current processes are:
- Slow and manual
- Prone to human variation
- Not robust to multilingual, dialect-rich speech

**Goal:** provide a quick, clinic-ready triage tool to support SLPs and parents—not to replace professional diagnosis.  fileciteturn1file0

---

## What we built

A **Bi-directional GRU (BiGRU)** classifier over **log-Mel spectrograms** that predicts **Normal** vs **Dysarthria / Speech Difficulty** from `.wav` audio.  
The system includes:

- **Model training & evaluation** (PyTorch + Torchaudio)
- **Flask backend API** for inference
- **React frontend** with drag‑and‑drop multi-file upload and per‑file confidence scores  
- **Red highlighting** for non‑normal predictions

**Pitch highlights**  
- Dataset (prototype): *Noise-Reduced UASPEECH Dysarthria* (for early experimentation)  
- Reported per-class metrics from our pitch iteration:  
  - **Normal:** Precision 1.000, Recall 0.992, F1 0.996  
  - **Dysarthria:** Precision 0.993, Recall 1.000, F1 0.997  fileciteturn1file0

> These numbers are from an early proof-of-concept stage and will evolve with better protocol, cross-validation, and a regional pediatric dataset.

---

## Repository structure

```
.
├── FlaskBackend_Team38/             # Flask API (GRU/Transformer backends)
├── ReactFrontend_Team38/            # React app (Vite) with multi-file upload
├── ModelsTrainingAndTesting_Team38/ # Training/testing scripts (BiGRU)
└── README.md
```

> If you only need BiGRU, you may remove Transformer-specific files to keep things lean.

---

## Quick start

### 1) Backend (Flask + PyTorch)

From `FlaskBackend_Team38/`:

```bash
# Windows PowerShell
py -m venv venv
. venv/Scripts/activate

# Core deps
pip install torch torchaudio flask flask-cors soundfile matplotlib
# (Optional) install CUDA builds of torch/torchaudio if your GPU is supported
```

Run the **BiGRU** backend:
```bash
python GRUBackEnd.py
```
API default: `http://127.0.0.1:5000`

### 2) Frontend (React + Vite)
From `ReactFrontend_Team38/`:
```bash
npm install
npm run dev
```
Open the printed URL (usually `http://127.0.0.1:5173`).

### 3) (Optional) Train / Test
From `ModelsTrainingAndTesting_Team38/`:
```bash
# Train
python train_gru_dysarthria.py

# Evaluate / larger test set
python test_gru_dysarthria.py
```

Expected dataset folders:
```
dataset_root/
  train/{Normal,Dysarthria}
  val/{Normal,Dysarthria}
  test/{Normal,Dysarthria}
```

---

## API

**POST** `/predict`  
- `multipart/form-data` with one or more `file` fields (WAV)  
- Returns JSON with predicted label + confidence per file  
- CORS enabled for local React

Example response:
```json
{
  "results": [
    {"file": "sample1.wav", "label": "Normal", "score": 0.87},
    {"file": "sample2.wav", "label": "Dysarthria", "score": 0.93}
  ]
}
```

---

## Frontend UX

- Drag & drop multiple files
- Display per-file confidence
- **Non-normal** results shown **in red**
- Ready for integration into clinic workflows (upload > triage > export result)

---

## Go‑to‑Market (from pitch)

- **Initial pilots**: pediatric therapy centers in the UAE (e.g., Ability Pediatric Rehabilitation)  
- **Business model**: SaaS tiers per volume of patients analyzed  
- **Expansion path**: UAE → GCC region  fileciteturn1file0

---

## Roadmap

1) **Data & MVP (Weeks 1–4)**: collect regional pediatric audio (with consent), fine‑tune models, ship MVP  
2) **Validation**: robust protocols, cross‑site evaluation, bias & fairness checks  
3) **Clinical integration**: reports, consent flows, EMR exports, Arabic/English UI  
4) **Deployment**: Edge/Cloud variants, privacy-preserving pipelines (on‑prem options)  fileciteturn1file0

---

## Tech stack

- **Python**: PyTorch, Torchaudio, Flask
- **React**: Vite, modern hooks-based UI
- **Tooling**: Node.js, npm

---

## Ethics & responsible use

- This is **not** a medical device and does **not** provide diagnosis.  
- Use only with proper consent and privacy controls.  
- Validate on **representative, pediatric, multilingual** datasets before any clinical decisions.

---

## Team & acknowledgments

- **Team ADUAA (Team 38)** — startAD AI for Good Sandbox, **Cohort 2**  
- Built for the **Ability** challenge with mentorship from the program organizers  
- Thanks to coaches and partners for their guidance  fileciteturn1file0

---

## License

Add a license (e.g., MIT/Apache-2.0). Until then, treat this as “source available for research/education.”
