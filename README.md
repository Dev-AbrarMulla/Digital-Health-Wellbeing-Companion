# Digital Health & Wellbeing Companion (Voice-Based)

The system listens to a user, understands their emotional state, responds with empathy, speaks the response back, and generates a daily reflective journal — all wrapped in a Responsible-AI evaluation layer (privacy, fairness, hallucination, safety).

## Overview

**Pipeline:**

```
User speech (mic)
   → Whisper STT (transcript)
   → Emotion/stress classifier
   → LLM (empathetic response, safety-constrained, crisis escalation)
   → TTS (spoken response)
   → Daily journal generator (reflection + mood score)
```

**Deliverables:** working prototype, technical report, Responsible-AI evaluation report.

## Team & Ownership

| Module | Owner | Path |
|---|---|---|
| STT + accent/language eval | TBD | `src/stt/` |
| Emotion classifier + LLM dialogue + safety prompts | TBD | `src/dialogue/` |
| TTS + crisis detection | TBD | `src/tts_safety/` |
| Journal generator + redaction + RAI eval report | TBD | `src/journal/`, `eval/` |

## Repo Structure

```
wellbeing-companion/
├── src/
│   ├── stt/            # transcription, accent WER eval
│   ├── dialogue/        # emotion classifier, LLM response, safety prompts
│   ├── tts_safety/      # text-to-speech, crisis detection
│   ├── journal/          # daily summary generator, PII redaction
│   └── pipeline.py        # end-to-end integration
├── eval/
│   ├── test_sets/          # crisis utterances, hallucination prompts, accent samples
│   ├── run_eval.py
│   └── metrics.py
├── notebooks/
│   └── demo.ipynb          # thin demo notebook, imports from src/
├── report/                  # technical + Responsible-AI report
├── tests/
├── requirements.txt
├── .env.example
└── .gitignore
```

## Setup

```bash
git clone <repo-url>
cd wellbeing-companion
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env       # fill in your API keys, never commit .env
```

### Required API keys (add to `.env`)

```
GROQ_API_KEY=
ELEVENLABS_API_KEY=
```

## Workflow

- `main` stays demo-ready at all times.
- Work on a feature branch per module (e.g. `stt-whisper`, `dialogue-safety`), PR into `main`.
- Each module exposes one clean function in, one function out — see `src/pipeline.py` for the integration contract.
- Every new feature should come with at least one adversarial test case in `eval/test_sets/` — the Responsible-AI report is built from accumulated evidence, not written last-minute.

## Responsible-AI Focus Areas

- **Privacy** — no raw audio retention; transcripts redacted before storage.
- **Hallucination** — the bot must never fabricate medical claims; tested against `eval/test_sets/medical_hallucination_prompts.json`.
- **Fairness** — emotion-classification and STT accuracy measured per accent group.
- **Safety** — crisis/self-harm signals must reliably trigger escalation, not in-model counseling.

## Status

🚧 Early setup — pipeline skeleton in progress.
