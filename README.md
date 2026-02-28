# ⚖️ LexAgent

> *"I can't lose."*  — Mike Ross, probably, after reading a 180-page SPA in 4 minutes.

**LexAgent** is an AI-powered contract analysis and legal QA system built for Mergers & Acquisitions, corporate transactions, and constitutional law research. Think of it as having **Mike Ross** on your deal team — minus the Harvard fraud, plus the photographic memory — available 24/7, never billing by the hour.

Upload a contract. Ask anything. Get the answer a brilliant legal mind would give you, grounded in the actual document, the US Constitution, and the statutes that govern the deal.

---

## What It Does

| Capability | Description |
|---|---|
| 📄 **Contract Analysis** | Ingests NDAs, SPAs, APAs, LOIs, merger agreements and flags risks by severity |
| ⚠️ **Risk Scoring** | Five-tier system: CRITICAL → HIGH → MEDIUM → LOW → INFO |
| ⚖️ **Constitutional Grounding** | Cross-references clauses against the US Constitution and federal statutes |
| 🤖 **Legal QA Bot** | Ask natural-language questions about any uploaded document |
| 🏛️ **M&A Intelligence** | Benchmarks terms against ABA Deal Points Study market standards |
| 📋 **Redline Export** | Generates analysis reports as PDF or DOCX with tracked changes |

---

## The Mike Ross Standard

Mike Ross could read a contract once and recall every clause verbatim three weeks later. He could cross-reference an obscure 1987 case against a current indemnification clause without breaking a sweat. He'd tell you exactly where you were exposed, what leverage you had, and how to phrase the pushback — in plain English, under pressure, on a deadline.

That's the bar LexAgent is built to meet.

The difference: LexAgent **actually went to Harvard**. Well, it was trained on the US Constitution, the Sherman Act, the ABA Model Purchase Agreement, and about 70 million tokens of legal text. Close enough.

> ⚠️ *LexAgent is a legal intelligence tool, not a licensed attorney. It will not help you commit fraud. Unlike certain associates at Pearson Hardman, it knows where the line is.*

---

## Architecture

LexAgent uses a **Retrieval-Augmented Generation (RAG)** architecture — no expensive fine-tuning, no training from scratch. The reasoning engine is already brilliant. We just make sure it reads the right documents at the right time.

```
┌─────────────────────────────────────────────────┐
│                   USER QUERY                    │
└────────────────────┬────────────────────────────┘
                     │
          ┌──────────▼──────────┐
          │   Embed the Query   │  ← sentence-transformers
          └──────────┬──────────┘
                     │
          ┌──────────▼──────────┐
          │  Vector Store (RAG) │  ← Pinecone / ChromaDB
          │  · Contract chunks  │
          │  · US Constitution  │
          │  · Federal statutes │
          │  · Model agreements │
          └──────────┬──────────┘
                     │  Top-K relevant passages
          ┌──────────▼──────────┐
          │    Claude API       │  ← Sonnet 4 / Opus 4
          │  (Reasoning Layer)  │
          └──────────┬──────────┘
                     │
          ┌──────────▼──────────┐
          │   Grounded Answer   │  ← cited, risk-scored, plain English
          └─────────────────────┘
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Orchestration | LangChain / LlamaIndex |
| Embeddings | `sentence-transformers/all-mpnet-base-v2` |
| Vector Store | Pinecone (cloud) or ChromaDB (local) |
| LLM | Claude API (`claude-sonnet-4` / `claude-opus-4`) |
| Document Parsing | PyMuPDF, python-docx |
| Storage | Google Drive / AWS S3 |
| Export | Pandoc, python-docx |

---

## Getting Started

### Prerequisites

```bash
python >= 3.10
pip install langchain chromadb sentence-transformers anthropic pymupdf python-docx
```

### Clone & Configure

```bash
git clone https://github.com/your-org/lexagent.git
cd lexagent
cp .env.example .env
# Add your ANTHROPIC_API_KEY and PINECONE_API_KEY to .env
```

### Build the Legal Corpus Index

```bash
python scripts/ingest.py --corpus ./data/corpus/ --store pinecone
```

This embeds the full legal corpus (~70M tokens) and uploads to your vector store. Run once. Takes ~4 hours on a T4 GPU.

### Run the QA Bot

```bash
python app.py
# Navigate to http://localhost:8000
```

### Analyze a Contract

```bash
python scripts/analyze.py --file ./contracts/your_nda.pdf --output ./reports/
```

---

## Development on Google Colab Pro

The entire v1.0 pipeline can be built and validated on **Google Colab Pro** (300 compute units is ~2x what you need).

| Task | GPU | Units |
|---|---|---|
| Corpus embedding | T4 | ~10 |
| RAG pipeline dev & testing | T4 | ~25 |
| QA Bot integration | T4 | ~12.5 |
| Clause classifier fine-tune (optional) | A100 | ~66 |
| Iteration buffer + reserve | — | ~101 |
| **Total** | | **~234 / 300** |

> 💡 **Unit conservation tip:** Always run `Runtime → Disconnect and delete runtime` when stepping away. Your Pinecone index persists. Your session does not. Don't pay for idle GPUs — Harvey Specter doesn't bill for hours he didn't work, and neither should your Colab instance.

Open the notebook to get started:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/your-org/lexagent/blob/main/notebooks/LexAgent_Build.ipynb)

---

## Legal Corpus (v1.0)

LexAgent ships with embeddings for:

**Constitutional**
- US Constitution — all Articles and all 27 Amendments
- Selected Federalist Papers (interpretive context)

**Federal Statutes**
- Sherman Antitrust Act, Clayton Act, Hart-Scott-Rodino Act
- Securities Exchange Act (1934), Securities Act (1933)
- Dodd-Frank, Sarbanes-Oxley
- UCC Articles 1, 2, 8, 9
- NLRA, Title VII, ADA, ADEA, ERISA

**Model Agreements**
- ABA Model Asset Purchase Agreement
- ABA Model Stock Purchase Agreement
- NVCA Model Venture Documents
- ISDA 2002 Master Agreement

---

## Risk Flag Reference

```
🔴 CRITICAL  — Deal-blocking. Uncapped liability, waived fundamental rights.
               Do not proceed without attorney review.

🟠 HIGH      — Significant deviation from market standard.
               Requires negotiation.

🟡 MEDIUM    — Aggressive but defensible. Flag for discussion.

🟢 LOW       — Minor ambiguity. Noted for completeness.

🔵 INFO      — Key term. No inherent risk.
```

---

## Roadmap

- [x] RAG pipeline architecture
- [x] US Constitution + federal statute corpus
- [ ] v1.0 — NDA, SPA, APA analysis engine
- [ ] v1.1 — Team collaboration, redline export, enterprise SSO
- [ ] v2.0 — Client-specific playbook training, data room integrations
- [ ] v2.5 — International law (UK, EU, APAC), tax module
- [ ] v3.0 — Autonomous negotiation drafting (human-approved)

---

## Disclaimer

LexAgent is a legal **intelligence** tool, not a licensed attorney. Nothing it produces constitutes legal advice, creates an attorney-client relationship, or should substitute for qualified legal counsel in actual transactions.

Mike Ross eventually got his law degree. LexAgent is still working on it. In the meantime, always have a Harvey review the final draft.

---

## Contributing

PRs welcome. Please read `CONTRIBUTING.md` before submitting. All contributions must pass the **Mike Ross Test**: *would this embarrass the firm in front of a federal judge?* If yes, revise.

---

## License

MIT License — see `LICENSE` for details.

---

<p align="center">
  <em>Built for deal teams who move fast and can't afford to be wrong.</em><br/>
  <em>⚖️ LexAgent — The associate who actually read the whole contract.</em>
</p>
