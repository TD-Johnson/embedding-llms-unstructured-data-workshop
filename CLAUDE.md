# CLAUDE.md — Embedding LLMs Workshop

## Project identity
This is a workshop repository for a workshop titled
"Programmatically using LLMs for qualitative research methods". It is developed by Dr Kyle Hemming (Centre for eResearch, University of Auckland) for researchers with no prior coding or ML background.
Target audience: researchers who use qualitative methods and want to understand how 
LLMs can be embedded into their workflows.
The priority is for researchers to learn by doing. They need relevant topics they can
relate the underlying concepts to.
Workshop length: 3 hours.

## Learning outcomes
- Build an LLM-assisted research workflow to aid analyses of qualitative data.
- Practice effective prompting techniques to prepare, label, and analyse unstructured text and images.
- Identify and mitigate key ethical risks, such as biases and hallucinations.
- Critically validate the outputs of LLM-assisted analysis methods.

## Repository structure
- notebooks/       → all workshop content. One .ipynb file per notebook.
- instructors/     → highly detailed lesson notes, step by step
- learners/        → participant setup instructions
- profiles/        → learner personas

### First cell
Notebooks have no Colab badge (they run locally). The first markdown cell holds
the title, duration and overview.

### Execution environment
All code is Python, in Jupyter notebooks that participants run locally in VS Code.
Participants download the repo as a ZIP from GitHub, then run `uv sync` to create
a `.venv` from `pyproject.toml` (Python version pinned in `.python-version`).
Setup steps are in learners/00-setup.md and are done before the workshop.
- No `!pip install` in notebooks — add packages with `uv add <package>`.
- Notebooks run with the `notebooks/` folder as the working directory, so local
  data is read from `../data/`.

### LLM API: vLLM on university hardware
- Models run on Dell Pro Max (GB10) machines serving vLLM inside the university
  network. Participants must be on the university VPN.
- Accessed with the `openai` Python library (OpenAI-compatible API), not `groq`.
- Each participant gets a base URL + API key on the day, stored in a `.env` file
  at the repo root (`LLM_BASE_URL`, `LLM_API_KEY`); `.env.example` is the template.
  `.env` is gitignored.
- Each machine serves one model. The setup cell reads its name from the server
  (`client.models.list()`), so model names are not hardcoded.
- `TEXT_MODEL` and `VISION_MODEL` are defined once in the setup cell.

### Standard notebook setup cell (every notebook)
Every notebook begins with this cell:
    # ============================================================
    # SETUP CELL — Run this once at the start of every notebook
    # ============================================================

    import os, json, base64, requests, io
    from dotenv import load_dotenv
    from openai import OpenAI
    from lxml import etree
    from PIL import Image
    from IPython.display import Image as IPImage, display

    # Load your base URL and API key from the .env file (set up in notebook 01)
    load_dotenv(override=True)
    if not os.getenv("LLM_BASE_URL") or not os.getenv("LLM_API_KEY"):
        raise RuntimeError("Could not find LLM_BASE_URL or LLM_API_KEY. Check your .env file (see notebook 01).")

    client = OpenAI(base_url=os.environ["LLM_BASE_URL"], api_key=os.environ["LLM_API_KEY"])

    # Each Dell Pro Max machine serves one model. Ask the machine for its name.
    try:
        TEXT_MODEL = client.models.list().data[0].id
    except Exception as error:
        raise RuntimeError(
            "Could not connect to the LLM. Check that you are on the university VPN "
            "and that the values in your .env file are correct."
        ) from error
    VISION_MODEL = TEXT_MODEL   # the same model reads both text and images

    print(f"Connected to {TEXT_MODEL}.")
    print("Setup complete.")

### Data source 1: NZ Legislation XML
No API key required. Direct XML access via URL pattern.
URL pattern: https://legislation.govt.nz/{type}/{category}/{year}/{number}/en/latest.xml
Key XML elements:
- <prov>  → individual sections
- <label> → section numbers
- <text>  → section content

Primary corpus (2 Acts):
- Privacy Act 2020:           /act/public/2020/31/en/latest.xml
- Runner up: Impounding Act 1955: /act/public/1955/108/en/latest.xml

### Data source 2: NZ archival images (visual feature extraction notebook)
Source: Archives New Zealand collection via Wikimedia Commons
Category: "Category:Images from Archives New Zealand" (~9,000 images)
Licence: No known copyright restrictions (NZ Crown copyright, NZGOAL framework)
Requires: User-Agent header identifying the workshop


### Key data, reference, and inspiration for thematic analysis notebook
New Zealand's Mental Health Act as a Case Study.
Information (MDPI), February 2026.
https://www.mdpi.com/2078-2489/17/2/161
Demonstrates LLM-assisted topic modelling on NZ legislation specifically.
For images, use url = "https://commons.wikimedia.org/w/api.php" and resize to width = 400 before sending to VISION_MODEL

## Notebook Map (locked structure v1)

### Format and delivery
- 00 and 06: Powerpoint slides with instructor notes in instructors/
- 01-05: Jupyter notebooks, one per notebook
- Notebooks live in notebooks/ folder in this repo
- GitHub Pages site links to the ZIP download, setup guide, and notebook previews
- Participants run notebooks locally in VS Code, on the university VPN

## Writing style
- Content lives in notebook markdown cells, not standalone .md files.
- Plain English. Write for a researcher who has never seen a terminal.
- Active voice. Short sentences.
- Define every technical term on first use.
- No unexplained acronyms.
- When introducing a concept, use a concrete research workflow example
  (e.g. "imagine you have 500 interview transcripts...").
- Use standard markdown in notebook cells. Do not use callout block syntax
  (:::objectives, :::challenge, :::solution, :::keypoints blocks).

## Consistency rules
- "LLM" not "large language model" after first use"
- Workshop tone: collegial and low-stakes. Mistakes are expected.

## What I will ask you to do
- Create notebooks from descriptions
- Revise existing notebooks based on feedback notes I provide
- Ensure consistency in terminology and difficulty progression across notebooks
- Summarise external readings into workshop-appropriate explanations

## What you must NOT do
- Do not add R code blocks — all code is Python
- Do not invent citations or tool names
- Do not use callout block syntax in notebooks (e.g. :::objectives, :::challenge, etc.)
