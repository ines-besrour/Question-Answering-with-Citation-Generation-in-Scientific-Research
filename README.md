# Question-Answering-with-Citation-Generation-in-Scientific-Research

This project is a research and development initiative at ScadsAI (TU Dresden) that aims to build an advanced question answering (QA) system with integrated citation generation. The system is designed to answer questions based on scientific literature while providing precise source references, ensuring transparency and reliability.

---

## Overview

The core idea is to combine state-of-the-art natural language models with a structured dataset of academic papers to enable users to query complex scientific content. The system leverages two primary models:

- **E5 Model**: A robust text representation model that efficiently encodes textual content from scientific documents.
- **LLMA3**: An advanced language model used for generating detailed and contextually aware answers.

These models work in tandem to retrieve relevant sections from academic papers, generate coherent responses, and accurately cite the source material.

---

## Data Format

The system works with academic papers stored in a JSONL (JSON Lines) format, where each line represents a single paper in JSON format. Below is a detailed explanation of the paper object structure and its components:

### Paper Object

Each paper is represented as a JSON object with the following keys:

- **`paper_id`**: The arXiv identifier of the paper (e.g., `"2105.05862"`).
- **`_pdf_hash`**: Always set to `None`.
- **`_source_hash`**: SHA1 hash of the arXiv source file.
- **`_source_name`**: Name of the arXiv source file (e.g., `"2105.05862.gz"`).
- **`metadata`**: Contains additional paper metadata sourced from [Kaggle's arXiv dataset](https://www.kaggle.com/datasets/Cornell-University/arxiv).
- **`discipline`**: Scientific discipline of the paper (e.g., "Physics").
- **`abstract`**: The abstract text extracted from the metadata.
- **`body_text`**: A list representing the main content of the paper, divided into sections. Each element in the list contains:
  - **`section`**: Name of the section.
  - **`sec_number`**: The section number.
  - **`sec_type`**: Type of the section (e.g., section, subsection).
  - **`content_type`**: The type of content (e.g., paragraph, listing).
  - **`text`**: The actual textual content.
  - **`cite_spans`**: A list of citation markers within the text. Each marker contains:
    - `start`: Starting character offset.
    - `end`: Ending character offset.
    - `text`: The text displayed as the citation.
    - `ref_id`: A key linking to the corresponding bibliographic entry in `bib_entries`.
  - **`ref_spans`**: A list of referenced non-textual content (e.g., formulas, figures). Each reference contains:
    - `start`: Starting character offset.
    - `end`: Ending character offset.
    - `text`: The surface text.
    - `ref_id`: A key linking to the corresponding entry in `ref_entries`.

---

## System Architecture

1. **Query Processing**
   When a user poses a query, the system performs the following steps:

   - **Text Representation**:  
     The query and the textual contents from the papers are encoded using the E5 model. This representation allows for efficient similarity search and retrieval of relevant sections from the corpus.

   - **Contextual Answer Generation**:  
     The LLMA3 model generates detailed answers based on the retrieved content. The model ensures that the generated text is both coherent and contextually relevant.

2. **Citation Generation**
   To maintain scientific rigor, the system integrates a citation mechanism:

   - **Identification of Sources**:  
     As the LLMA3 model produces the answer, it identifies key parts of the response that correspond to information from the source documents.

   - **Mapping References**:  
     When generating an answer, the system maps embedded citations and formula references to the detailed information in the paper object, ensuring users can trace back to the original sources.
