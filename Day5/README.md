# Day 5 — Conversation Memory & RAG (Retrieval-Augmented Generation)

**Drive Ready – AI Agents | One Person Company (OPC) Initiative**  
Department of Technical Training, Aditya University

## Overview

Day 5 upgrades the **Level 2 Single Agent** with two core capabilities:
1. **Thread-Isolated Conversation Memory** using LangGraph's `InMemorySaver` checkpointer (`thread_id`).
2. **Retrieval-Augmented Generation (RAG)** using vector embeddings (`ChromaDB` and `FAISS`) so the agent answers strictly from verified documents and store policies without hallucinating.

## What Was Built

### Part 1 — Sweet Crumbs Home Bakery Agent (`InMemorySaver` + `ChromaDB`)
- Stored **12 bakery policy FAQs** (delivery, UPI advance, cancellation, allergens, photo cakes, pickup hours, etc.) inside a `ChromaDB` collection.
- Built `@tool def search_faq(question: str)` to retrieve the top-3 semantically relevant policies at runtime.
- Added multi-turn memory isolation across separate customer threads (`thread_id="ravi"` vs `thread_id="sravani"`).
- Evaluated against `tests.csv` using isolated threads per test case (**86% pass rate**, 100% semantic accuracy).

### Part 2 — PDF RAG Pipeline (`PyMuPDF` + `SentenceTransformer` + `FAISS`)
- Extracted text from `The_Little_Lantern.pdf` using `PyMuPDF` (with automatic `Tesseract OCR` fallback for scanned pages).
- Split text into overlapping chunks (`chunk_size=600`, `overlap=150`) and generated 384-dimensional embeddings via `all-MiniLM-L6-v2`.
- Indexed vectors in `FAISS` (`IndexFlatL2`) and enforced strict context-grounded Q&A (refusing out-of-PDF queries like *"Who is Virat Kohli?"*).

### Part 3 — Self-Practice Domain: BrightPage Stationery
- Created a custom store assistant with **16 stationery policies** in `ChromaDB` (`search_stationery_faq`), live pricing (`get_stationery_price`), and multi-thread memory (`ananya` vs `karthik`).
- Evaluated on `stationery_tests.csv` (**83% pass rate**, 100% factual accuracy).
