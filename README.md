# 🎗️ AI Cancer Research Assistant (RAG System)

## 📘 Overview
This project demonstrates how **Retrieval-Augmented Generation (RAG)** can support oncology research by providing evidence-based answers from clinical trial data.  
It is a prototype that combines a lightweight retrieval layer with a generative model to produce concise, context-aware summaries for researcher queries.

## ⚙️ Methodology
- Built a simple RAG pipeline using **TF-IDF vectorization** for retrieval.  
- A small curated corpus of 6 trial abstracts (immunotherapy, targeted therapy, CAR-T, radiation) is used as the knowledge base.  
- The top retrieved documents are passed as context to a generative model (LLM) which composes an evidence-based answer.

## 📊 Results (Prototype)
- Average relevance: **36%** (prototype-level metric for small corpus)  
- Peak accuracy on close-match queries: **83%**  
- Response time: **~15 ms** per query (retrieval only, on local TF-IDF)

> ⚠️ These metrics are for demonstration only (small corpus). For production, metrics must be re-measured on larger, labeled datasets.

## 💡 Clinical Value
- Rapid literature triage for oncologists and trial designers.  
- Reduces time spent searching PubMed / ClinicalTrials.gov from hours to seconds.  
- Helps design smarter trial cohorts, endpoints, and feasibility assessments based on historical outcomes.

## 🧠 Tools (Prototype)
- Python, Pandas  
- Scikit-learn (TF-IDF & similarity)  
- LangChain (optional orchestration)  
- Any public LLM endpoint or local LLM for generation


## 🧭 Usage
Open and run the notebook in Google Colab:  
[Colab Link](https://colab.research.google.com/drive/1guG711I_83rq_UMKv9_a_q9emNjhS0yg?usp=drive_link)

---

## ▶️ From Prototype to Production — Practical Roadmap

Below is a concise, technical roadmap you can present to show you understand how to build a real, production-grade Cancer RAG system. Use these points to explain design choices, required engineering, and evaluation steps.

### 1) Data & Corpus
- **Scale up corpus**: collect full-text trial reports, PubMed abstracts, ClinicalTrials.gov records, regulatory documents, and meta-analyses.  
- **Metadata**: store trial metadata (disease, phase, sample size, endpoints, treatment, outcomes, dates).  
- **Provenance**: keep source links and extraction timestamps to enable auditability.

### 2) Retrieval Layer (Better than TF-IDF)
- **Embeddings**: move from TF-IDF → semantic embeddings using domain models (Bio/Clinical embeddings). Example open-source options: `BioBERT`, `PubMedBERT`, or sentence-transformers fine-tuned on biomedical text.  
- **Vector DB**: store embeddings in a production vector database like **Weaviate**, **Pinecone**, **Milvus**, or **FAISS** (self-hosted) for fast similarity search and filtering by metadata.

### 3) Generative Model (LLM)
- **Open-source model options** (self-host or cloud):
  - Llama 2 / Llama 3 (if fine-tuned and hosted appropriately)
  - Mistral, Falcon (depending on licensing & size)
  - Bio-focused models if available (or adapt a general LLM with biomedical fine-tuning)
- **Approach**:
  - Use retrieved documents as context prompt (RAG pattern).  
  - Limit token window by summarizing long docs (chunk + re-rank).  
  - Optionally fine-tune or instruction-tune LLM on biomedical Q&A pairs for better fidelity.

### 4) Safety, Hallucination Control & Evidence Grounding
- **Evidence citation**: always return short answer + supporting excerpts with direct source links (document ID, sentence).  
- **Conservatism**: tune system to say “I don’t know” if confidence is low — implement retrieval confidence thresholds.  
- **Calibration**: measure hallucination rate by comparing generated claims to cited passages; reduce by stricter prompt engineering and retrieval quality.

### 5) Evaluation Strategy
- **IR metrics**: Precision@K, Recall, MRR for retrieval.  
- **Generation metrics**: ROUGE / BLEU (limited), human expert evaluation, fact-checking against sources.  
- **Clinical utility**: expert-labeled tasks — does the system surface correct trial arms, endpoints, outcomes?  
- **A/B tests**: measure time-saving for oncologists (user study).

### 6) Explainability & UI
- **Answer + Evidence Panel**: show short answer, then supporting snippets and source links.  
- **Filters**: allow filtering by phase, tumor type, date, or intervention.  
- **Audit log**: save queries and model responses for traceability.


---

## ✅ Minimal Production Stack (Open-Source Friendly)
- Vector DB: **Milvus** or **FAISS** (self-host) / **Weaviate** (open-source)  
- Embeddings: **sentence-transformers** with `biomed/` model or `PubMedBERT` derivatives  
- LLM: self-host Llama-family model or use an enterprise API if allowed  
- Orchestration: **LangChain** / **LlamaIndex** for RAG pipelines  
- API: **FastAPI** + Gunicorn / Uvicorn  
- Deployment: Docker + Kubernetes for scale

---

## 🔬 Example Implementation Steps (concise)
1. Crawl & collect trial texts → clean & store raw docs + metadata.  
2. Chunk long docs → compute embeddings → push to vector DB with metadata.  
3. Build retrieval API that returns top-K docs + similarity scores.  
4. Compose prompt: system + top-K contexts + user question → call LLM.  
5. Post-process LLM output: extract claims, attach source snippets & links.  
6. Run human-in-the-loop evaluation and iterate.

---

## ⚠️ Common Pitfalls & How to Avoid Them
- **Hallucinations**: avoid long context windows without citation; use stricter retrieval + answer grounding.  
- **Outdated info**: include publication dates and prefer recent trials for time-sensitive queries.  
- **Overfitting to small corpus**: prototype metrics won’t generalize — expand corpus before production claims.

---

## 📚 References & Next Steps
1. Replace TF-IDF with `sentence-transformers` embeddings and integrate a small FAISS index inside the Colab.  
2. Add evidence citation to each generated answer.  
3. Create a short `dockerized` API example to show how this can be served.


---

## 🪪 License
MIT License

