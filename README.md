#  Scientific Data Extraction from OPV Papers via LLM

> Automated extraction of structured device parameters from Organic Photovoltaic (OPV) scientific literature using the Gemini 2.5 Flash API — reducing extraction time from hours to seconds.

---

##  Project Overview

Manually extracting device performance data from scientific PDFs is a time-consuming, error-prone process. This pipeline automates it entirely using **Google Gemini 2.5 Flash** as a multimodal document reader, with advanced prompt engineering to guarantee structured JSON output.

**Domain**: Organic Photovoltaics (OPV) — next-generation solar cell technology  
**Impact**: Extraction time reduced from **several hours → a few seconds per article**

---

##  Key Features

- **Native PDF multimodal processing** via Gemini File API (reads text layer + layout simultaneously)
- **Advanced prompt engineering**: strict JSON schema with conformity validation
- **Smart device separation**: each D:A ratio variation, thickness, or fabrication condition is treated as a distinct device entry
- **Edge case handling**: missing values, ambiguous units, nested data normalization
- **Structured Excel export** via Pandas — ready for downstream analysis

---

##  Output Structure

Each extracted device entry contains up to **8 parameters per device**, including:

| Field | Example |
|-------|---------|
| Donor material | PM6 |
| Acceptor material | Y6 |
| D:A ratio | 1:1.2 |
| Active layer thickness | 100 nm |
| PCE (%) | 16.5 |
| Voc (V) | 0.85 |
| Jsc (mA/cm²) | 24.3 |
| Fill Factor (%) | 79.8 |

---

##  Prompt Engineering Approach

The extraction prompt instructs Gemini to:

1. **Identify every unique device configuration** in the paper
2. **Treat every variation as a separate entry** (ratio, thickness, additive, etc.)
3. Return **only a strict JSON array** — no preamble, no markdown fences
4. Handle ambiguous or missing values with explicit `null` rather than hallucinating data

```python
# Example of strict schema enforcement
EXTRACTION_PROMPT = """
Role: Expert Material Scientist and Data Curator specializing in OPV.
Task: Extract ALL device configurations as separate JSON objects.
...
Return ONLY valid JSON. No explanations.
"""
```

---

##  Tech Stack

- **Python** — `google-generativeai`, `pandas`, `openpyxl`, `json`, `os`
- **LLM**: Gemini 2.5 Flash (multimodal, native PDF support)
- **Output**: Structured `.xlsx` export

---

##  Project Structure

```
├── OPV.ipynb               # Main extraction notebook
├── papers/                 # PDF articles to process (not included)
├── scanned_pdf_extraction.xlsx  # Output file (generated)
└── README.md
```

---

##  How to Run

```bash
pip install google-generativeai pandas openpyxl
```

1. Add your Gemini API key in the notebook (`API_KEY = "your_key_here"`)
2. Place PDF articles in the `papers/` folder
3. Run all cells — output is saved to `scanned_pdf_extraction.xlsx`

>  **Rate limits**: Gemini API has per-minute request limits. Test with one article first (as noted in the notebook comments).

---

## 📈 Performance

| Metric | Manual | This Pipeline |
|--------|--------|---------------|
| Time per article | 2–4 hours | ~10 seconds |
| Devices per article | Up to 8 | Up to 8 (automated) |
| Output format | Inconsistent | Structured Excel |

---

## 💡 Why This Matters

This pipeline is directly applicable to **systematic literature reviews** and **materials database construction** in academic research — tasks that currently consume significant researcher time and are prone to transcription errors.
