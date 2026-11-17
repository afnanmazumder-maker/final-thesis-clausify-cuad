# Final Thesis: AI-Powered Legal Document Analysis System

**Clause Detection, Category-Level Risk Prioritization and Plain-Language Explanation on CUAD**

BRAC University, Department of Computer Science and Engineering
Group ID P25301051

Jerin Aktar, Afnan Mazumder, Shoyeb Hasan Sayem
Supervisor: Utsha Kumar Roy

[![Open in Streamlit](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://clausify-p3.streamlit.app)

**[Try the live app](https://clausify-p3.streamlit.app)** , paste a contract and it returns a risk-prioritized report.

---

## What this is

A staged pipeline that reads a commercial contract, detects which of CUAD's 41 clause
categories it contains, extracts the clause text, attaches a High / Medium / Low risk level,
and presents the result as a risk-prioritized report a non-lawyer can act on.

## Repository layout

| Path | Contents |
|---|---|
| `app/` | **Clausify**, the Streamlit demonstration application |
| `research/` | Thesis code, experiments, results, report and IEEE paper |

## Live demo

The application is deployed on Streamlit Community Cloud:

**https://clausify-p3.streamlit.app**

It runs on CPU and loads its weights from Hugging Face on first boot, so a cold start can
take a few minutes. Open it once before relying on it.

## Running the app locally

```bash
pip install -r requirements.txt
streamlit run app/app.py
```

Model weights are not stored in this repository. They are published at
[`af123Af/clausify-models`](https://huggingface.co/af123Af/clausify-models) and download
automatically on first run. To use local checkpoints instead, set `CUAD_MODELS_DIR`.

## Dataset

CUAD v1, The Atticus Project, 510 commercial contracts annotated across 41 clause
categories, obtained from the Hugging Face hub (`theatticusproject/cuad`). It is downloaded
on first run and is not committed here.

## Headline results

Measured on a contract-level 80/20 split, 102 unseen contracts, 4,182 held-out decisions.

| Task | Result |
|---|---|
| Clause presence | micro-F1 0.776, accuracy 85.5% |
| Span extraction | token-F1 0.763 |
| End-to-end survival | 21.7% |

The thesis reports three negative results alongside these: the windowed transformer does not
beat a TF-IDF baseline, the summarizer emits category templates rather than summarizing, and
the configuration that wins on micro-F1 misses 37.5% of High-risk clauses while a
configuration that finishes last on micro-F1 misses only 13.6%.

Full write-up in `research/project_report/`.
