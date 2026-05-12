
#  RAG Evaluation with RAGAS

Evaluating a Retrieval-Augmented Generation (RAG) system using purpose-built RAG metrics.

## Business Scenario

**Meridian Wealth Partners** uses a RAG-powered Financial Analyst AI to answer client questions about investment policies. The system retrieves from 5 policy documents (Balanced Portfolio, Growth Portfolio, Conservative Income, NRI Investment, SIP Policy) and generates answers.

**The Problem:** Without measurement, there's no way to know if the AI is:
- Hallucinating answers not present in the documents
- Pulling irrelevant chunks during retrieval
- Missing critical context for accurate answers

For a wealth management firm, one wrong investment recommendation can result in significant client lawsuits (~₹12 crore exposure).

## What I Built

A complete RAG evaluation pipeline using the RAGAS framework, measuring 4 specialized metrics designed specifically for retrieval-augmented systems.

## The 4 RAGAS Metrics

| Metric | What It Measures | Question It Answers |
|--------|-----------------|---------------------|
| **Faithfulness** | Is the answer grounded in retrieved context? | "Did the generator hallucinate?" |
| **Answer Relevancy** | Is the answer relevant to the question? | "Did the generator address the query?" |
| **Context Precision** | Is the retrieved context actually useful? | "Did the retriever pull relevant chunks?" |
| **Context Recall** | Does the context contain all needed info? | "Did the retriever miss important chunks?" |

## Architecture

```
Client Question
      │
      ▼
┌─────────────┐
│  Retriever  │ ◄── Context Precision + Context Recall
│   (FAISS)   │     (Did it pick the right pages?)
└─────────────┘
      │
      ▼
   3 Context Chunks
      │
      ▼
┌─────────────┐
│  Generator  │ ◄── Faithfulness + Answer Relevancy
│   (GPT-4)   │     (Did it answer truthfully and on-topic?)
└─────────────┘
      │
      ▼
   Final Answer
```

## Results on 15-Question Test Set

| Metric | Score | Verdict |
|--------|-------|---------|
| Faithfulness | 0.93 | ✅ Strong — AI stays grounded in documents |
| Context Precision | 1.00 | ✅ Perfect — retriever picks relevant chunks |
| Context Recall | 1.00 | ✅ Perfect — no missing context |
| Answer Relevancy | N/A* | Compatibility issue with embeddings API |

*Answer Relevancy encountered a known RAGAS/OpenAI Embeddings compatibility issue in newer versions. The other 3 metrics ran successfully and showed strong performance.

## Improvement Experiment

Tested whether smaller chunk sizes would improve retrieval quality:

| Configuration | Chunks Created | Context Precision |
|---------------|----------------|-------------------|
| V1 (chunk_size=500) | 7 chunks | 1.00 |
| V2 (chunk_size=300) | 11 chunks | 0.97 |

**Finding:** Smaller chunks did not improve precision in this case — the original 500-char chunking was already optimal for these short policy documents. This demonstrates an important lesson: **measure before AND after changes** — sometimes the "fix" doesn't help, and that's a valuable signal.

## Skills Demonstrated

- RAG system design (retriever + generator architecture)
- FAISS vector store construction with chunking strategies
- Specialized RAG evaluation using RAGAS framework
- LLM-as-Judge evaluation patterns
- A/B comparison of retrieval configurations
- Honest interpretation of mixed results (perfect, weak, and failed metrics)
- Debugging library compatibility issues in production-grade evaluation pipelines

## Tech Stack

- Python 3.12
- LangChain + LangChain Community (vector store integration)
- FAISS (similarity search)
- RAGAS (RAG-specific evaluation metrics)
- OpenAI (gpt-4.1-mini, text-embedding-3-small)
- Matplotlib (results visualization)
- Pandas (results analysis)

## Key Learnings

1. **RAG needs RAG-specific metrics.** General LLM evaluation (accuracy, helpfulness) misses retrieval-side issues. Without measuring context precision and recall, you can't tell if the retriever is the problem or the generator.

2. **Library compatibility is a real production challenge.** The Answer Relevancy metric hit a `OpenAIEmbeddings.embed_query` AttributeError — a known issue between newer RAGAS versions and the LangChain embeddings API. Production evaluation pipelines need robust error handling.

3. **Improvements aren't guaranteed.** Reducing chunk size from 500 to 300 characters slightly *decreased* context precision (1.00 → 0.97). This is a real engineering lesson — measure before and after every change. "It seems better" is not evidence.

4. **Even with one broken metric, you can ship.** 3 out of 4 metrics gave clear, actionable signal. Faithfulness at 0.93 told me the AI was grounded; precision/recall at 1.00 told me the retriever was working. Production engineering is about working with imperfect data.

## How to Run This Lab

```bash
# Install dependencies
pip install ragas langchain langchain-openai langchain-community faiss-cpu datasets matplotlib

# Set OpenAI API key
export OPENAI_API_KEY="your-key-here"

# Open the notebook
jupyter notebook RAG_Evaluation_RAGAS.ipynb
```

## Files

- `RAG_Evaluation_RAGAS.ipynb` — Complete executed notebook with all outputs

 

 

 
