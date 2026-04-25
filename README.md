# 🧠 Applied AI Systems Engineer — PhD-Level Roadmap

> Objective:
> Build end-to-end AI systems from **data acquisition → representation learning → retrieval → reasoning → agents → evaluation → production**.
>
> Constraint:
> Avoid API-dependency. Prefer **open models, reproducible pipelines, and measurable systems**.

---

# 🧭 0. Meta-Strategy (How to Use This Roadmap)

- Each phase has:
  - **Theory kernels** (what to truly understand)
  - **Systems constraints** (latency, cost, reliability)
  - **Artifacts** (what you must build)
  - **Evaluation** (how you know it works)
- Maintain a **lab notebook** (Markdown + experiments + metrics).
- Every component must be:
  - **deterministic where possible**
  - **observable (logs/metrics/traces)**
  - **replaceable (clean interfaces)**

---

# 🧭 1. Computational Foundations (Systems + Numerics)

## 1.1 Python for Systems
- Memory model (ref counts, GC)
- Data locality, cache behavior
- `asyncio`, event loops, backpressure
- Multiprocessing vs threading (GIL implications)

## 1.2 Numerical Computing
- Vectorization, broadcasting
- Stability (overflow/underflow, log-sum-exp)
- Linear algebra primitives (BLAS/LAPACK mental model)

## 1.3 OS & Networking
- File descriptors, epoll/kqueue
- TCP vs UDP, HTTP/2, keep-alive
- Serialization: JSON vs MsgPack vs Protobuf (trade-offs)

## Artifacts
- High-throughput **ingestion service** (async, rate-limited, retry/backoff)
- **Columnar pipeline** for large files (chunked I/O, memory mapping)
- **Custom task queue** (idempotency keys, dead-letter queue)

## Evaluation
- Throughput (req/s), tail latency (p95/p99)
- Memory footprint vs data size
- Failure injection (network drops)

## Resources
- https://docs.python.org/3/
- https://numpy.org/doc/
- https://realpython.com/

---

# 🧭 2. Statistical Learning (From First Principles)

## 2.1 Risk Minimization
- Empirical risk, regularization (L1/L2)
- Bias-variance decomposition

## 2.2 Optimization
- SGD, Momentum, Adam (derivations, convergence intuition)
- Learning rate schedules, warmup

## 2.3 Generalization
- Cross-validation, leakage, calibration

## Artifacts
- **Logistic regression from scratch** (vectorized)
- **Calibration module** (Platt scaling / isotonic)
- **Feature pipeline** (encoding, normalization, leakage guards)

## Evaluation
- ROC-AUC, PR-AUC, calibration curves
- Ablations on features/regularization

## Resources
- https://scikit-learn.org/stable/
- https://www.statlearning.com/

---

# 🧭 3. Deep Learning Mechanics (Beyond API Usage)

## 3.1 Backpropagation
- Computational graphs, autodiff
- Jacobians, chain rule in practice

## 3.2 Architectures
- MLPs, CNNs, RNNs (limits)
- Initialization, normalization (LayerNorm/BatchNorm)

## 3.3 Training Systems
- Mixed precision (fp16/bf16)
- Gradient clipping, accumulation
- Checkpointing, resuming

## Artifacts
- **Autodiff engine (mini)** (forward/backward)
- **Training loop** with hooks (logging, checkpoints)
- **Text classifier** (tokenize → embed → train)

## Evaluation
- Loss curves, gradient norms
- Overfit small batch sanity test

## Resources
- https://pytorch.org/tutorials/
- https://d2l.ai/

---

# 🧭 4. Transformers & LLM Internals

## 4.1 Tokenization
- BPE/WordPiece, vocabulary trade-offs

## 4.2 Attention
- Scaled dot-product attention
- Complexity O(n²), long-context limits

## 4.3 Transformer Stack
- Multi-head attention, FFN, residuals, LayerNorm
- KV-cache, decoding strategies (greedy, beam, sampling)

## 4.4 Efficiency
- Flash attention concepts
- Quantization (int8/4), KV-cache reuse

## Artifacts
- **Mini-transformer** (character/subword)
- **Decoder with KV-cache**
- **Benchmark** (latency vs sequence length)

## Evaluation
- Perplexity, throughput tokens/s
- Memory vs context length

## Resources
- https://arxiv.org/abs/1706.03762 (Attention Is All You Need)
- https://huggingface.co/learn

---

# 🧭 5. Representation Learning & Embeddings

## 5.1 Embedding Spaces
- Cosine similarity, anisotropy
- Contrastive learning basics

## 5.2 Indexing
- ANN (HNSW, IVF)
- Trade-offs: recall vs latency

## Artifacts
- **Embedding service** (batching, caching)
- **FAISS index** (HNSW/IVF variants)
- **Recall@k evaluator**

## Evaluation
- Recall@k, MRR
- Latency vs recall curve

## Resources
- https://github.com/facebookresearch/faiss

---

# 🧭 6. Data Pipelines (Batch + Streaming)

## 6.1 Ingestion
- Connectors (files, APIs, webhooks)
- Idempotency, deduplication

## 6.2 Processing
- Cleaning, normalization, PII handling
- Chunking strategies (fixed, semantic, sliding window)

## 6.3 Orchestration
- DAGs, retries, backfills
- Data versioning (dataset snapshots)

## 6.4 Streaming
- Event-driven pipelines (producers/consumers)
- Exactly-once vs at-least-once semantics

## Artifacts
- **End-to-end ingestion DAG** (documents → chunks → embeddings)
- **Incremental re-indexer** (change data capture)
- **Streaming pipeline** (event → transform → sink)

## Evaluation
- Freshness (lag), throughput
- Data quality checks (schema, nulls, duplicates)

## Resources
- https://airflow.apache.org/
- https://kafka.apache.org/

---

# 🧭 7. RAG (Retrieval-Augmented Generation) — Production

## 7.1 Retrieval Stack
- Hybrid search (BM25 + dense)
- Query rewriting, expansion

## 7.2 Re-ranking
- Cross-encoders vs bi-encoders

## 7.3 Context Engineering
- Window packing, compression
- Citation grounding

## 7.4 Systems Concerns
- Caching (query/result/embedding)
- Access control (per-tenant docs)

## Artifacts
- **Hybrid retriever** (BM25 + FAISS)
- **Re-ranker module**
- **RAG service** (multi-tenant, ACL)

## Evaluation
- Answer correctness (gold set)
- Faithfulness (citation checks)
- Latency budget breakdown

## Resources
- https://docs.langchain.com/
- https://docs.llamaindex.ai/

---

# 🧭 8. Agent Architectures (Deterministic + Stochastic Control)

## 8.1 Formalization
- Agent loop: observe → plan → act → reflect
- State machine vs planner-based control

## 8.2 Tooling Layer
- Typed tool schemas (strict I/O)
- Deterministic wrappers, timeouts

## 8.3 Memory
- Short-term (context)
- Long-term (vector DB / structured store)

## 8.4 Planning
- ReAct, Plan-and-Execute
- Task graphs, dependency resolution

## 8.5 Multi-Agent
- Coordinator + specialists
- Parallel vs sequential execution

## Artifacts
- **Agent runtime** (planner, executor, memory, tools)
- **Code agent** (repo index → edit → test loop)
- **Web agent** (browser automation, extraction)

## Evaluation
- Task success rate
- Tool call accuracy
- Loop convergence (iterations to success)

## Resources
- https://arxiv.org/abs/2303.17580 (ReAct)

---

# 🧭 9. Tooling & Sandboxing

## 9.1 Isolation
- Containers (namespaces, cgroups)
- Filesystem isolation

## 9.2 Execution
- Deterministic runners
- Resource limits (CPU/mem/time)

## Artifacts
- **Sandbox executor** (Docker-based)
- **Tool registry** (versioned, typed)
- **Audit logs** (per action trace)

## Evaluation
- Escape attempts (security tests)
- Determinism (same input → same output)

---

# 🧭 10. Evaluation Science (Critical)

## 10.1 Offline
- Static datasets (gold Q/A)
- Regression tests

## 10.2 Online
- A/B testing, shadow traffic

## 10.3 Metrics
- Accuracy, faithfulness
- Latency, cost per request
- Tool success rate

## Artifacts
- **Eval harness** (batch runner)
- **Dashboard** (metrics + traces)
- **Red-team suite** (prompt injection, adversarial)

## Evaluation
- Statistical significance (A/B)
- Drift detection

---

# 🧭 11. Serving & Scaling

## 11.1 API Layer
- Request validation, rate limits
- Streaming responses

## 11.2 Workers
- Async jobs, queues, retries

## 11.3 Caching
- Multi-layer (in-memory, Redis, CDN)

## 11.4 Model Serving
- Batching, dynamic routing
- Quantized models for cost

## Artifacts
- **Scalable service** (API + workers + queues)
- **Autoscaling policy**
- **Cost optimizer** (route cheap/expensive models)

## Evaluation
- p95 latency, error rates
- Cost per 1k requests

## Resources
- https://fastapi.tiangolo.com/
- https://docs.docker.com/

---

# 🧭 12. Multi-Model Orchestration

## 12.1 Routing
- Heuristics vs learned routers
- Confidence-based fallback

## 12.2 Composition
- Pipeline (retrieval → reasoner → verifier)
- Ensemble strategies

## Artifacts
- **Router service** (policy engine)
- **Fallback graph** (graceful degradation)

## Evaluation
- Cost vs accuracy frontier

---

# 🧭 13. Security & Governance

## Topics
- PII detection/masking
- Access control (row/document level)
- Prompt injection defenses
- Auditability

## Artifacts
- **Policy layer** (pre/post filters)
- **Audit trail** (who/what/when)

---

# 🧪 Capstones (End-to-End)

## A) Production RAG Platform
- Ingestion DAG → hybrid retrieval → re-ranking
- Multi-tenant ACL
- Eval + cost dashboard

## B) Autonomous Code Agent
- Repo indexing → planning → edits → test loop
- Sandbox execution + PR generation

## C) Enterprise Automation Agent
- Email/CRM/calendar tools
- Human-in-the-loop approvals
- Task graphs

---

# 📊 Milestones

- Month 1–2: Systems + DL basics
- Month 3–4: Transformers + embeddings
- Month 5–6: Pipelines + RAG
- Month 7–8: Agents + evaluation
- Month 9+: Production + scaling

---

# 🧠 Final Principles

- Systems > models
- Data pipelines are the real moat
- Evaluation is non-negotiable
- Determinism where possible, stochasticity where needed

---

# 📌 END
