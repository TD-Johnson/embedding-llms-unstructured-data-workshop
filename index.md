---
layout: default
title: Programmatically Using LLMs for Qualitative Research Methods
---

This hands-on workshop introduces researchers to using large language models (LLMs) programmatically to support qualitative research workflows. Using real New Zealand legislative data, you will practice prompting techniques to prepare, label, and analyse unstructured text and images — then learn how to critically evaluate and validate what the model produces. No prior coding or machine learning experience is required.

## How to use this workshop

You run the workshop notebooks on your own laptop, in VS Code. Before the workshop, follow the [setup guide](learners/00-setup.md). In short:

1. Install VS Code and uv.
2. [Download the workshop as a ZIP file](https://github.com/TD-Johnson/embedding-llms-unstructured-data-workshop/archive/refs/heads/main.zip) and unzip it.
3. Open the unzipped folder in VS Code and run `uv sync` in the terminal.

On the day, connect to the university VPN, open each notebook from the `notebooks` folder, and run the cells from top to bottom.

## No VS Code? Use Google Colab

If you could not install VS Code, you can run the notebooks in Google Colab instead. Colab is a free Google service that runs notebooks in your web browser, so there is nothing to install and you do not need the VPN.

1. Click **Open in Colab** next to a notebook in the table below. Sign in with a Google account if asked.
2. Colab may warn that the notebook was not written by Google. Click **Run anyway**.
3. Your instructor will give you a base URL and API key for Colab. These are different from the ones for VS Code.
4. Run the setup cell. Two boxes appear, one after the other. Paste the base URL into the first and press Enter. Then paste the API key into the second and press Enter.
5. In notebook 01, skip the steps about VS Code and the `.env` file. Everything else works the same.

Colab does not save your changes for you. To keep your work, choose **File → Save a copy in Drive**.

## Workshop notebooks

| Episode | File | Preview on GitHub | Run in Google Colab |
|---------|------|-------------------|---------------------|
| 01 — Environment setup | `notebooks/01-environment-setup.ipynb` | [View](https://github.com/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/01-environment-setup.ipynb) | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/01-environment-setup.ipynb) |
| 02 — LLMs as research instruments | `notebooks/02-llms-as-instruments.ipynb` | [View](https://github.com/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/02-llms-as-instruments.ipynb) | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/02-llms-as-instruments.ipynb) |
| 03 — Exploratory analysis | `notebooks/03-prompt-engineering.ipynb` | [View](https://github.com/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/03-prompt-engineering.ipynb) | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/03-prompt-engineering.ipynb) |
| 04 — Visual feature extraction | `notebooks/04-visual-extraction.ipynb` | [View](https://github.com/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/04-visual-extraction.ipynb) | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/04-visual-extraction.ipynb) |
| 05 — Looping an LLM over many documents | `notebooks/05-looping-over-documents.ipynb` | [View](https://github.com/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/05-looping-over-documents.ipynb) | [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TD-Johnson/embedding-llms-unstructured-data-workshop/blob/main/notebooks/05-looping-over-documents.ipynb) |

---

Developed by Dr Toby Johnson, Centre for eResearch, University of Auckland
