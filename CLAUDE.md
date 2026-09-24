# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This is an academic research project (IMT Mines Alès) studying how harmonic musical structure influences the popularity of romantic piano pieces — a Hit Song Prediction (HSP) task within Music Information Retrieval (MIR). See `README.md` for the full research theme, supervisors, and citations.

The codebase is a small, early-stage data pipeline (recently converted from notebooks to plain Python scripts — see commit "from notebook to python"), not an application with a stable architecture yet.

## Environment setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Key dependencies: `music21` (symbolic music analysis), `numpy`, `requests` (dataset/API downloads), `python-dotenv`, `ipykernel`, `tqdm`.

There are no configured lint, test, or build commands in this repo yet.

## Pipeline architecture

`src/` scripts are named with letter prefixes indicating pipeline order:

- **`src/A_extract_music_dataset.py`** — Downloads and prepares the primary dataset. `get_music_dataset()` downloads the PianoCoRe MIDI dataset (+ `metadata.csv` / `composers.csv`) from Zenodo into `../data/` (sibling to the repo root, gitignored), extracts the zip, copies the `raw` MIDI tree into `data/pieces/`, strips `.mid` files (keeping `.mxl`), and removes now-empty folders. Force-download/extract/copy and delete-after-extract behavior are controlled by module-level flags (`force_download`, `force_extract`, `force_copy`, `delete_zip`, `delete_raw`) at the top of the file — flip these when re-running steps instead of adding CLI args.
- **`src/B_popularity_dataset.py`** — Popularity dataset construction (currently a stub).
- **`src/main.py`** — Pipeline entry point (currently a stub).
- **`src/utile.py`** — Shared utilities used across the pipeline:
  - Filesystem helpers: `download_file`, `extract_zip`, `copy_directory`, `get_file_size`, `get_folder_size`, `count_file`, `remove_files`, `remove_folders`. Each of these has a `force_*`/idempotency guard — they skip work if the destination already exists, matching the flag-driven pattern in `A_extract_music_dataset.py`.
  - `fetch_musicbrainz_works(query, limit, max_results)` — paginated MusicBrainz Work API client using Lucene-style queries (e.g. `tag:piano AND (tag:romantic OR tag:classical)`). Respects MusicBrainz's 1 req/sec rate limit (`time.sleep(1.1)` between pages) and requires a descriptive `User-Agent`; preserve both when modifying this function.

## Git conventions

Never add Claude as a co-author on commits (no `Co-Authored-By: Claude ...` trailer).

## Data layout

Datasets are downloaded to `../data/` relative to the script location (i.e. outside the repo, alongside it) — not `./data/`. `data/` is also gitignored at the repo root via `.gitignore`, so raw/processed data is never committed regardless of location. When adding new dataset steps, follow the existing convention: raw archives → extracted → filtered/copied into a clean working subfolder, with size/count logging at each step (see `download_and_extract_dataset` / `clean_dataset` in `A_extract_music_dataset.py`).
