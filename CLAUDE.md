# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This is an academic research project (IMT Mines Alès) studying how harmonic musical structure influences the popularity of romantic piano pieces — a Hit Song Prediction (HSP) task within Music Information Retrieval (MIR). See `README.md` for the full research theme, supervisors, and citations.

The codebase is currently an empty scaffold (`src/` has no files yet) — no pipeline architecture to document until code is added.

## Environment setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Key dependencies: `music21` (symbolic music analysis), `numpy`, `requests` (dataset/API downloads), `python-dotenv`, `ipykernel`, `tqdm`.

There are no configured lint, test, or build commands in this repo yet.

## Git conventions

Never add Claude as a co-author on commits (no `Co-Authored-By: Claude ...` trailer).

## Data layout

`data/` and `romantic_piano_corpus-v2.0/` are gitignored at the repo root — raw/processed datasets are never committed regardless of location.
