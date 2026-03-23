<div align="center">
    
# RAG Chatbot for Communicable Disease Query Answering in Childcare Settings

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![HuggingFace](https://img.shields.io/badge/HuggingFace-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![Falcon](https://img.shields.io/badge/Falcon--7B--Instruct-6A0DAD?style=for-the-badge&logo=falconai&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-0078D4?style=for-the-badge&logo=meta&logoColor=white)
![Gradio](https://img.shields.io/badge/Gradio-F97316?style=for-the-badge&logo=gradio&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black)

</div>

---

## Overview

This project implements a **Retrieval-Augmented Generation (RAG) chatbot** designed to assist childcare providers in answering questions about communicable diseases. The system is grounded in the **NH Disease Handbook for Childcare Providers** published by the New Hampshire Department of Health and Human Services (DHHS), covering over 40 communicable diseases commonly encountered in childcare settings.

Rather than relying on a standalone LLM that may hallucinate medical facts, this system retrieves relevant passages from the handbook before generating a response — ensuring every answer is traceable and handbook-grounded.

---

## Authors

| Name | Email |
|---|---|
| Aaron Kent M. Celles | akmcelles@mymail.mapua.edu.ph |
| Lee Ryan A. Leviste | lraleviste@mymail.mapua.edu.ph |
| Enrique S. Santeco | essanteco@mymail.mapua.edu.ph |

> School of Information Technology, Mapua University, Makati, Philippines

---

## Features

- **RAG Pipeline** — retrieves relevant disease context before generating answers, minimizing hallucination
- **Falcon-7B-Instruct** — 4-bit NF4 quantized for efficient inference on Google Colab T4 GPU (~5GB VRAM)
- **PubMedBERT Embeddings** — domain-specific biomedical embeddings (`pritamdeka/S-PubMedBert-MS-MARCO`) for accurate semantic retrieval
- **FAISS Vector Index** — fast similarity search over 482+ sentence-level chunks from the handbook
- **Scope Control** — the system refuses to answer questions outside the handbook's coverage
- **Gradio Web UI** — accessible chatbot interface deployable from any device
- **53-Question Evaluation Dataset** — spanning easy, medium, hard, out-of-scope, and natural language tiers

---

## Architecture

```
User Query
    │
    ▼
PubMedBERT Encoder
    │
    ▼
FAISS Vector Search  ←──── NH Disease Handbook (482 chunks)
    │
    ▼
Relevance Guard (cosine ≥ 0.70)
    │
    ▼
Falcon-7B-Instruct (4-bit)
    │
    ▼
Grounded Answer
```

---

## Tech Stack

| Component | Tool |
|---|---|
| Language Model | `tiiuae/Falcon3-7B-Instruct` (4-bit NF4) |
| Embedding Model | `pritamdeka/S-PubMedBert-MS-MARCO` |
| Vector Store | FAISS (IndexFlatIP) |
| PDF Extraction | PyPDF2 |
| Sentence Tokenization | NLTK |
| Web UI | Gradio |
| Runtime | Google Colab T4 GPU |

---

## Getting Started

### Prerequisites

- Python 3.8+
- Google Colab with T4 GPU (recommended) or local GPU with ≥6GB VRAM

### Installation

```bash
pip install transformers==4.45.0 accelerate bitsandbytes sentence-transformers faiss-cpu PyPDF2 gradio nltk torch numpy pandas scikit-learn
```

### Usage

1. Open the notebook in Google Colab
2. Run all cells in order
3. Upload the NH Disease Handbook PDF when prompted
4. Interact with the chatbot via the Gradio interface

---

## Dataset

| Property | Value |
|---|---|
| Source | NH Disease Handbook for Childcare Providers |
| File Type | PDF |
| Pages | ~180 |
| Diseases Covered | 40+ communicable diseases |
| Text Size (post-cleaning) | ~120,000 characters |
| Chunks Produced | 482 (5 sentences, 1-sentence overlap) |

---

## Evaluation

The system was evaluated on a **53-question benchmark dataset** across five difficulty tiers:

| Tier | Count | Description |
|---|---|---|
| Easy | 11 | Single-fact, single-disease questions |
| Medium | 12 | Multi-step or procedural questions |
| Hard | 10 | Cross-disease comparisons, policy questions |
| Out-of-scope | 10 | Questions beyond the handbook's scope |
| Natural | 10 | Informal, colloquial queries from parents/caregivers |

### Metrics

**Retrieval-Level:** Recall@K, Precision@K, Context Relevancy (cosine similarity)

**Generation-Level:** Faithfulness, Answer Relevancy, Groundedness (keyword overlap)

---

## Related Paper

> Celles, A. K. M., Leviste, L. R. A., & Santeco, E. S. — *Retrieval-Augmented Generation for Communicable Disease Query Answering in Childcare Settings* — CSS181-3, Mapua University

**Abstract:** A RAG system powered by the Qwen3-8B large language model and PubMedBERT biomedical embeddings was developed to address disease-related queries based on the NH Disease Handbook for Childcare Providers. Qwen3-8B achieved the highest Correctness score of 0.686 among models evaluated. Results demonstrate the effectiveness of domain-specific retrieval grounding and biomedical embeddings in minimizing hallucination risk.

**Keywords:** Retrieval-Augmented Generation, LLM, PubMedBERT, FAISS, NLP, public health, medical question answering

---

## Disclaimer

This chatbot is intended as a **reference tool for non-clinical childcare providers only**. It does not replace professional medical advice. The system is strictly scoped to the NH Disease Handbook and will decline questions outside its coverage.
