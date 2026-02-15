# FarmChatBot — End-to-End Retrieval-Augmented Decision Support System

FarmChatBot is a retrieval-augmented decision-support system that combines semantic search, data pipelines, and controlled LLM reasoning to generate grounded responses.

Instead of allowing a language model to answer directly, the system first retrieves verified information from structured and real-time sources, constructs evidence-backed context, and then constrains the model to reason only over that context.

The objective is to simulate how enterprise knowledge systems operate in environments where correctness, traceability, and evaluation are required.

The project is intentionally built as a data + AI pipeline rather than a standalone chatbot interface.

Key idea:
generation happens only after retrieval.

This repository demonstrates how large language models can be integrated into a measurable, auditable system instead of being used as a black-box API.

---

## Tech Stack

**Languages:** Python  
**Libraries & Frameworks:** HuggingFace Transformers, SentenceTransformers, NLTK, Scikit-learn  
**Vector Database:** ChromaDB  
**LLM Inference:** Groq API (Llama-4-Scout-17B-Instruct)  
**Workflow Automation:** n8n  
**Web Processing:** Trafilatura  
**Evaluation:** BLEU, ROUGE, Precision, Recall, F1, Cosine Similarity  
**Data Processing:** Pandas, NumPy  
**Interface:** Jupyter Notebook pipeline

---

## How to Run

1. Clone the repository
git clone https://github.com/suhaneye/farmchatbot.git
cd farmchatbot

2. Install dependencies
pip install -r requirements.txt

3. Run main pipeline
Open:
farmBot_Final.ipynb

Run all cells sequentially.

The pipeline will:
- embed the query
- retrieve relevant context
- generate grounded response
- optionally translate to Hindi

4. Evaluation (optional)
Open:
RAG_test(web+qa).ipynb

This notebook computes BLEU, ROUGE, F1, and semantic similarity scores.

---

## Repository Structure

farmchatbot/
│
├── farmBot_Final.ipynb        # Main RAG pipeline
├── RAG_test(web+qa).ipynb     # Evaluation pipeline
├── My_workflow_3.json         # n8n automation workflow
├── questionsv4.csv            # QA dataset
├── evaluated_out_ref2.csv     # Evaluation results
├── requirements.txt           # Dependencies
└── README.md

---

## What Problem This Project Addresses

Large language models often produce confident but incorrect answers. In advisory or operational environments this is unsafe.

This project explores a system design approach where:
- the model is not treated as the source of truth
- retrieved evidence becomes the source of truth
- responses must be grounded in retrieved context
- output quality is quantitatively evaluated

The focus is system reliability and measurability, not just text generation.

---

## System Overview

User Query → Embedding → Vector Retrieval → Context Assembly → LLM Reasoning → Evaluation

Components:
• Python pipeline orchestration  
• Vector search using embeddings (ChromaDB)  
• Real-time web ingestion workflow  
• Controlled LLM reasoning via Groq API  
• Automatic evaluation using NLP similarity metrics  

The conversational interface is only an access layer — the core of the project is the retrieval, data processing, and evaluation pipeline.

Primary Contribution:
Implemented the semantic retrieval pipeline (embedding + vector search), designed the context-grounding prompt logic, built the evaluation framework (BLEU, ROUGE, F1, cosine similarity), and integrated real-time web ingestion via an automated workflow.

---

## Example Query

User Query:
"What should I do if wheat leaves turn yellow?"

System Process:
- Retrieve similar cases from knowledge base
- Fetch recent agricultural advisory data
- Construct grounded context
- Generate response using constrained LLM

Output:
The system returns an evidence-based explanation (e.g., nitrogen deficiency or possible fungal disease) along with recommended corrective actions, while indicating uncertainty if context is insufficient.

---

## System Architecture

The system is designed as a modular pipeline separating storage, retrieval, reasoning, and evaluation.

High-level flow:

1. Query processing and normalization
2. Embedding generation
3. Similarity search in vector database
4. Optional live web retrieval
5. Context assembly
6. Constrained LLM reasoning
7. Response evaluation

---

### Architecture Components

**Query Processing**
- Hindi & English input support
- Speech-to-text conversion
- Hindi-to-English translation for consistent retrieval

**Embedding & Indexing**
- Sentence embeddings via `all-MiniLM-L6-v2`
- Stored in ChromaDB vector store

**Knowledge Retrieval**
Static dataset:
~174,000 agricultural Q/A pairs

Dynamic dataset:
Real-time web content via automated workflow

Top-K passages retrieved using cosine similarity.

**Context Assembly**
- Ranking and truncation
- Structured prompt construction

**Controlled Generation**
Model: Llama-4-Scout-17B-Instruct (Groq API)

The model is instructed to:
- answer only using retrieved evidence
- avoid unsupported claims
- return uncertainty when context is insufficient

**Multilingual Output**
- English output translated to Hindi
- Optional Hindi text-to-speech

---

## Data Pipeline

### Data Ingestion
- Dataset cleaned and standardized
- Embedded into vector representations
- Stored in ChromaDB (`qa_context`)

### Real-Time Pipeline
Search → Scrape → Clean → Extract → Embed → Store

### Retrieval
For each query:
- Generate embedding
- Perform similarity search
- Merge static and live context

### Prompt Construction
Context + User Question + Instruction (answer only from context)

---

## Evaluation & Reliability

The system output is quantitatively evaluated using:

- BLEU (lexical overlap)
- ROUGE-L (sequence similarity)
- Precision / Recall / F1 (keyword coverage)
- Cosine similarity (semantic similarity)

Evaluation results are stored in `evaluated_out_ref2.csv`.

Purpose:
Treat the chatbot as a measurable system that can be benchmarked and improved.

---

## Design Decisions & Tradeoffs

**RAG vs Fine-Tuning**
Chosen for traceability, easy updates, and inspectable reasoning.

**Vector Search vs Keyword Search**
Provides semantic matching for varied farmer queries.

**English Reasoning + Multilingual Interface**
Improves model accuracy while supporting local language interaction.

**Grounded Generation**
Prefers uncertainty over incorrect answers.

**Real-Time Web Context**
Improves relevance but introduces noise handling challenges.

**Evaluation Metrics**
Added for reliability monitoring and regression testing.

---

## Future Improvements

- Confidence scoring
- Conversation memory
- FastAPI deployment
- Embedding caching
- Retrieval ranking optimization
- Messaging platform integration

---

## Takeaway

The goal is not to demonstrate a chatbot UI, but to show how LLMs can be integrated into a reliable information retrieval system where:

retrieval provides knowledge  
the model performs reasoning  
evaluation measures reliability
