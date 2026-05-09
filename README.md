[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/_SA52CFJ)
# AI-CA2-2026

This repo contains my CA2 notebook for the Artificial Intelligence for Vision and NLP module.

The notebook is trying to build a basic multi-modal document understanding app. It takes in scanned or photographed documents, runs OCR on them, does some NLP on the text that was found, does some computer vision work on the image, and then pulls the results together into a final report.

Some of the documents are intentionally poor, like photographed labels, a scrunched utility bill and a handwritten page. The notebook shows where the approach works well and where it struggles. I took this approach so could see the difference between what was straightforward for the system to understand about the document and where it had trouble.

## What the notebook does

The main notebook is `CA2_Template.ipynb`.

Rough overview:

- reads in the configured documents from `data/originals`
- converts PDFs to images where needed
- creates different image versions, such as grayscale, denoised and thresholded
- runs Tesseract OCR on each image version
- picks the best OCR version using a simple confidence and word count score
- cleans the OCR text and runs NLP steps like tokenisation, stopword removal, stemming and lemmatisation
- extracts useful text features using regex, NER, TF-IDF and spaCy phrase matching
- uses OpenCV to find visual regions in the document
- tries to label regions, such as account details, charges, usage blocks, table-like rows or instruction sections
- links OCR text lines back to detected image regions
- creates a final single-document report
- runs the same pipeline across all configured documents for comparison

## Setup

The notebook expects Python/Jupyter and the packages listed in `requirements.txt`.

The rough setup is:

```bash
pip install -r requirements.txt
```

The notebook also uses Tesseract OCR, so Tesseract needs to be installed on the machine separately. The notebook tries to find it automatically, but if Tesseract is not installed then the OCR cells will not work.

The spaCy English model is included in the requirements file, and the notebook also has a cell to download it if needed.

## Folder overview

- `data/originals` contains the source documents/images used by the notebook
- `data/output` contains generated files, such as converted PDF pages and saved visual evidence images
- `CA2_Template.ipynb` contains the main assignment code and outputs
- `PRESENTATION_SCRIPT.md` contains a rough 10 minute presentation script
- `requirements.txt` contains the Python packages used

## How to run it

Open `CA2_Template.ipynb` and run it from top to bottom.

There is a single active document walkthrough first. This shows each step in detail for one selected document, so it is easier to understand what is happening.

Near the end there is a `Run All Documents` section. This repeats the same general pipeline for every document in `documents_to_process`, so the outputs can be compared.

**NB**: the run-all section currently depends on helper functions that are defined earlier in the notebook. So if the kernel has been restarted, run the notebook from the top rather than jumping straight to the run-all cells.

## What to expect from the outputs

The notebook produces a few different types of output:

- OCR comparison tables showing which image version worked best
- raw and cleaned OCR text samples
- extracted fields, such as dates, money values, kWh usage or percentages
- NLP outputs, such as top words, lemmas, named entities, key phrase matches and TF-IDF values
- processed images showing OCR boxes and detected regions
- multi-modal tables linking OCR lines to visual regions
- a final report for the active document
- run-all comparison tables for all configured documents

Some outputs are better than others depending on the input document. Scanned and flat page-like documents generally work better. Curved product labels, handwritten text and noisy photographed images are harder, so those outputs are included more as examples of the limitations.

## Notes
There is some repeated logic because I wanted to keep the notebook readable rather than turning the whole thing into a fully abstracted pipeline, which to me is actually more of a programming assignment then image processing.
