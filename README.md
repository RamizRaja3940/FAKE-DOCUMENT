# Scholarship Document Fraud Detection — Starter POC

A minimal FastAPI-based starter to ingest documents, extract basic forensic/consistency features (stubs), and compute a transparent risk score for human review.

This is an MVP scaffold: extractors return placeholder values now, with clear TODOs to wire real checks (OCR, PDF structure, image forensics, QR decode, dedup).

## Quick Start

- Prereqs: Python 3.10+
- Optional: create and activate a virtualenv
- Install deps: see `requirements.txt` (you can start the API without extras)
- Run API: `uvicorn app.main:app --reload`

## Endpoints

- `GET /health` — health check
- `POST /upload` — multipart upload of a single file
  - Returns: case metadata, extracted feature placeholders, risk score, reasons

Example curl:

```
curl -F "file=@/path/to/your/document.pdf" http://localhost:8000/upload
```

## Project Structure

- `app/main.py` — FastAPI app, `/upload` endpoint
- `app/processing/pipeline.py` — Orchestrates feature extraction + scoring
- `app/processing/risk_engine.py` — Weighted, explainable risk scoring
- `app/processing/rules.py` — Weights and thresholds
- `app/processing/extractors/*` — Feature extractors (stubs + TODOs)
- `app/storage/files.py` — Saves uploaded files to `storage/uploads/`
- `config/settings.py` — Basic settings (paths, thresholds)

## Roadmap (suggested)

- Implement real checks in extractors:
  - PDF: incremental updates, metadata, object edits near stamps/signatures
  - Image: ELA, copy-move, DPI/PRNU inconsistency, font/edge artifacts
  - OCR: template checks, field consistency, numbers/date logic
  - QR: decode + payload vs visible text
  - Dedup: perceptual hash/text MinHash store & match
- Add persistence (SQLite/Postgres) for features and outcomes
- Introduce an async worker for background processing
- Build a reviewer UI for triage and audit trail

## Dependencies

Base (to run API and stubs):
- fastapi
- uvicorn
- pydantic
- python-multipart
- pillow (PIL)

Extras (implementations often need system libs — add as you go):
- opencv-python-headless (image forensics)
- pytesseract (OCR) + Tesseract binary
- pikepdf (PDF structure)
- imagehash (perceptual hashes)
- pyzbar (QR/barcode; needs zbar)

Tip: Start with base; add extras per feature as you implement.

## Safety & Policy

- This system flags risk for human review; do not auto-reject solely on score.
- Handle PII securely; delete raw uploads per policy after review.
- Log features and decisions for audit and continuous improvement.

