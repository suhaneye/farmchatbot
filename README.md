# 🌾 FarmChatBot — Multilingual Agricultural Advisory System (RAG + Evaluation Framework)

<p align="center">
Production-style AI system demonstrating grounded LLM responses using retrieval instead of hallucination.
</p>

---

## 📋 Overview

FarmChatBot is a **Retrieval Augmented Generation (RAG)** based advisory system designed to deliver reliable agricultural recommendations in low-resource language settings.

Instead of allowing a language model to guess answers, the system retrieves verified agricultural information first and forces the model to respond only using that evidence.

The project emphasizes:

* grounded AI responses
* measurable output quality
* real-world deployment thinking

It supports **multilingual interaction (Hindi & English)** via text or speech while maintaining English-based reasoning for model reliability.

---

## 🚀 Quick Start

### 1. Clone the repository

git clone https://github.com/suhaneye/farmchatbot.git
cd farmchatbot

### 2. Install dependencies

pip install -r requirements.txt

### 3. Run the main pipeline

Open the notebook:

farmBot_Final.ipynb

Run all cells sequentially to:

* load embeddings
* retrieve knowledge context
* generate grounded answer

### 4. Run evaluation (optional)

RAG_test(web+qa).ipynb

This computes BLEU, ROUGE, F1 and semantic similarity.

---

## 🏗️ System Architecture

### Knowledge Sources

**Static Knowledge Base**

* ~174,000 agricultural Q/A pairs
* Embeddings: all-MiniLM-L6-v2
* Vector Database: ChromaDB (qa_context)

**Dynamic Web Context**

* Automated retrieval using n8n workflow
* Content extraction via Trafilatura
* Embedded and stored as web_context

---

### Retrieval Pipeline (RAG Core)

For each user query:

1. Convert query to embedding
2. Retrieve Top-K relevant passages from vector database
3. Optionally fetch real-time web context
4. Combine and truncate context
5. Send to LLM

The model is instructed to **not answer without retrieved context**.

---

### Grounded Generation

Model: Llama-4-Scout-17B-Instruct (Groq API)

Behavior constraint:

* respond only using provided evidence
* return uncertainty when context is insufficient

Goal → reduce hallucinated recommendations.

---

## 🌐 Multilingual Pipeline

**Input**

* Hindi speech → Speech-to-Text
* Hindi text → translated to English

**Processing**

* Retrieval and reasoning in English

**Output**

* English response → Hindi translation
* Optional Hindi Text-to-Speech

---

## 📏 Evaluation Framework

The chatbot is treated as a **measurable system**, not just a demo.

| Metric                  | Purpose             |
| ----------------------- | ------------------- |
| BLEU                    | lexical overlap     |
| ROUGE-L                 | sequence similarity |
| Precision / Recall / F1 | keyword coverage    |
| Cosine Similarity       | semantic similarity |

Outputs are stored in:

evaluated_out_ref2.csv

---

## 📁 Repository Contents

farmchatbot/

* farmBot_Final.ipynb        → main RAG pipeline
* RAG_test(web+qa).ipynb     → evaluation pipeline
* My_workflow_3.json         → n8n automation workflow
* questionsv4.csv            → dataset
* evaluated_out_ref2.csv     → evaluation outputs
* README.md

---

## 🧠 Key Ideas Demonstrated

* Retrieval Augmented Generation (RAG)
* Vector search using embeddings
* Grounded LLM responses (hallucination reduction)
* Multilingual interface design
* Automatic evaluation of AI outputs
* Applied AI for real-world decision support

---

## 🛠️ Skills Demonstrated

* Python
* NLP pipelines
* Vector databases (ChromaDB)
* Prompt grounding
* LLM evaluation metrics
* System design thinking
* Applied AI in low-resource environments
* Data pipeline structuring

---

## 🔮 Future Improvements

* Confidence scoring before answering
* Conversation memory
* WhatsApp / Telegram interface
* Additional Indian language support

---

## Takeaway
This project is a **retrieval-first advisory pipeline** where evidence precedes generation and responses are evaluated for reliability.
