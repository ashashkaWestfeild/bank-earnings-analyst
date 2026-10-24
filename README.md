# bank-earnings-analyst

Ask questions about Indian banks' quarterly results and get answers with citations, covering both **what management said** on the earnings call and **the numbers behind it**.

> **Status: just started (Oct 2026).** Nothing is built or measured yet. This repo grows week by week; see the roadmap below.

## Goal

- **Questions it should answer**
  - *What did management say about deposit growth in Q1 FY27?*
  - *How did GNPA move for HDFC Bank and ICICI Bank over the last four quarters?*
  - *What guidance did SBI give on credit costs, and did the numbers back it up?*
- **Data** (public exchange filings only): earnings-call transcripts and investor presentations of 6 listed Indian banks (HDFC Bank, ICICI Bank, State Bank of India, Axis Bank, Kotak Mahindra Bank, Bank of Baroda) for Q2 FY26 – Q1 FY27.
- **Two paths, one system**
  - Transcript text → chunks → hybrid retrieval (BM25 + dense) → cited answers.
  - Key metrics (NIM, GNPA, NNPA, CASA, PCR, CRAR, advances and deposit growth) → a Postgres table → exact lookups.
  - An agent decides which path a question needs. Every number in an answer must come from the metrics table or a cited passage.

## Rules

1. **Built from scratch first.** Chunking, embeddings, cosine search, BM25, rank fusion and evaluation are written by hand. No LangChain, LlamaIndex or ChromaDB until the core works and has been measured; a framework is used later only to compare against it.
2. **Measure before tuning.** Every change is judged on a hand-written gold set and reported as counts (e.g. *recall@5 = 31/40*), never as adjectives.
3. **Public data only.** Raw PDFs are not committed; `data/sources.csv` lists where each file came from so anyone can download them again.

## Results

None yet. Numbers will appear here only after they are measured on the gold set, with a version history that keeps the bad runs too.

## Roadmap

- [ ] Ingest transcripts; first LLM calls (local model via Ollama); tests
- [ ] Chunking + embeddings + naive RAG with citations
- [ ] Gold set (40 questions) + retrieval and answer evaluation
- [ ] Hybrid search (BM25 + dense, rank fusion) on Postgres + pgvector
- [ ] Reranking, bank/quarter filters, latency per stage
- [ ] FastAPI service, async, streaming; tests
- [ ] Docker, CI with an evaluation gate, live demo
- [ ] Agent: metrics tool + transcript search, routing, retry when the answer scores low
- [ ] Cost, latency and tracing
