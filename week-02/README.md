# Bitext Customer Support FAQ Semantic Search Engine

An end-to-end semantic retrieval system built on [ChromaDB](https://www.trychroma.com/) and [Sentence Transformers](https://www.sbert.net/) (`all-MiniLM-L6-v2`) using the Bitext 27K Customer Support dataset.

---

## 📌 Overview

This project builds a vector-search index across customer service interaction data. It handles raw ingestion, HTML and whitespace normalization, sliding-window word chunking, embedding generation, in-memory cosine indexing, and top-$k$ semantic query resolution.

### Pipeline Architecture

```
Raw CSV (Hugging Face)
       │
       ▼
Text Cleaning (HTML stripping, whitespace normalization, deduplication)
       │
       ▼
Sliding-Window Chunking (100 words, 20 overlap)
       │
       ▼
Dense Embedding (all-MiniLM-L6-v2, 384-dim, normalized)
       │
       ▼
Vector Storage & Indexing (ChromaDB HNSW with Cosine Distance)
       │
       ▼
Top-3 Nearest Neighbor Retrieval
```

---

## ⚙️ Requirements & Installation

### Prerequisites
- Python 3.9 – 3.11
- PyTorch (CUDA optional but recommended for faster batch indexing)

### Setup

```bash
# Clone or place files in your working directory
git clone <repo-url>
cd <repo-folder>

# Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate       # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

---

## 🚀 Pipeline Step-by-Step

### 1. Ingestion & Data Preparation
- Loads the Bitext 27K dataset directly from Hugging Face (`Bitext_Sample_Customer_Support_Training_Dataset_27K_responses-v11.csv`).
- Extracts `instruction` (user utterance) and `response` (agent answer), mapping them to `question` and `answer`.
- Normalizes escaped HTML entities (`unescape`), strips potential HTML tags (`BeautifulSoup`), collapses whitespace, and eliminates nulls and duplicate pairs.

### 2. Document Chunking
- Documents are structured as:
  ```text
  Question: <cleaned_question>
  Answer: <cleaned_answer>
  ```
- Long answers are split using a sliding-window strategy:
  - **Chunk Size**: 100 words
  - **Overlap**: 20 words
- Generates 44,132 individual chunk records with tracked `source_id` metadata.

### 3. Embedding & Vector Indexing
- **Model**: `sentence-transformers/all-MiniLM-L6-v2` (384-dimensional dense vectors).
- Vectors are $L_2$-normalized (`normalize_embeddings=True`).
- Ingested in batches of 256 into an in-memory ChromaDB collection configured with Cosine space (`metadata={"hnsw:space": "cosine"}`).

---

## 🔍 Example Queries & Sample Outputs

Below are example queries spanning distinct support categories along with what the top matches return.

### Example 1: Order Cancellation (Intent: `cancel_order`)
```python
query = "How do I cancel an order?"
```
**Expected Matches**:
- **Rank 1** (Distance ~0.146): Steps to navigate to account order history, select the purchase, and trigger the "Cancel Order" prompt.
- **Rank 2** (Distance ~0.148): Instructions for locating the specific order ID and contacting live support if self-cancellation fails.

---

### Example 2: Payment Issue (Intent: `payment_issue`)
```python
query = "My credit card was charged twice for the same purchase."
```
**Expected Matches**:
- Steps to report duplicate card charges, relevant transaction lookup guidance, and customer billing support hours.

---

### Example 3: Password / Account Recovery (Intent: `recover_password`)
```python
query = "I cannot log in because I lost my password."
```
**Expected Matches**:
- Directions to the login screen "Forgot Password" self-service reset flow and email token verification steps.

---

### Example 4: Shipping Address Change (Intent: `change_shipping_address`)
```python
query = "Can I change where my package is being delivered after placing it?"
```
**Expected Matches**:
- Cut-off limits before order dispatch, online address modification instructions, and support agent escalation.

---

## 💻 Running Query Searches in Python

To run queries programmatically after populating the index:

```python
def search_faq(query_text: str, top_k: int = 3):
    query_embedding = model.encode([query_text], normalize_embeddings=True).tolist()
    results = collection.query(
        query_embeddings=query_embedding,
        n_results=top_k,
        include=["documents", "distances", "metadatas"]
    )
    
    print(f"\n=== Query: {query_text} ===")
    for rank, (doc, dist) in enumerate(zip(results["documents"][0], results["distances"][0]), start=1):
        print(f"\n[Rank {rank}] Cosine Distance: {dist:.4f}")
        print(doc)

# Test search
search_faq("Where can I download my invoice receipt?")
```

---

## ⚡ Performance Guidelines

- **GPU (NVIDIA T4 / Colab)**: Indexing 44,132 chunks takes approximately **35–50 seconds**.
- **CPU (4–8 cores)**: Indexing takes approximately **4–8 minutes**.
- **Query Latency**: Top-3 retrieval per query runs in **10–25 milliseconds**.