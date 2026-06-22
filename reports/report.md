# Lab 18 — Production RAG Pipeline (Cá nhân)

**Họ tên:** [Điền tên]  
**Ngày:** 22/06/2026

---

## 1. Tổng quan

Bài tập cá nhân — implement toàn bộ 5 modules:

`M1 Chunking → M5 Enrichment → M2 Hybrid Search → M3 Reranking → LLM Answer → M4 RAGAS Eval`

| Module | File | Trạng thái | Tests |
|--------|------|------------|-------|
| M1 Chunking | `src/m1_chunking.py` | ✅ | 13/13 |
| M2 Hybrid Search | `src/m2_search.py` | ✅ | 5/5 |
| M3 Reranking | `src/m3_rerank.py` | ✅ | 5/5* |
| M4 Evaluation | `src/m4_eval.py` | ✅ | 4/4 |
| M5 Enrichment | `src/m5_enrichment.py` | ✅ | 10/10 |

\* M3 cần đủ RAM khi load `bge-reranker-v2-m3` (nếu OOM: tăng paging file Windows)

---

## 2. Kết quả RAGAS

> Chạy: `python naive_baseline.py` rồi `python src/pipeline.py`  
> Số liệu: `reports/naive_baseline_report.json`, `reports/ragas_report.json`

| Metric | Naive Baseline | Production | Δ |
|--------|---------------|------------|---|
| Faithfulness | _điền_ | _điền_ | _điền_ |
| Answer Relevancy | _điền_ | _điền_ | _điền_ |
| Context Precision | _điền_ | _điền_ | _điền_ |
| Context Recall | _điền_ | _điền_ | _điền_ |

---

## 3. Key Findings

1. **Biggest improvement:** Hybrid search + rerank cải thiện retrieve tiếng Việt so với dense-only
2. **Biggest challenge:** RAM khi load nhiều model (bge-m3 + reranker); enrichment 81 API calls
3. **Surprise finding:** Corpus có document superseded (v2023 vs v2024) — dễ fail context recall

---

## 4. Deliverables

| File | Mô tả |
|------|--------|
| `analysis/failure_analysis.md` | Bottom-5 + Error Tree |
| `analysis/reflections/reflection_lab18.md` | Reflection (đổi tên → `reflection_[HọTên].md`) |
| `reports/ragas_report.json` | Auto sau pipeline |
| `reports/naive_baseline_report.json` | Auto sau baseline |

---

## 5. Checklist nộp

```bash
pytest tests/ -v
python naive_baseline.py
python src/pipeline.py
python check_lab.py
```

- [ ] Điền tên + RAGAS scores vào file này và `analysis/failure_analysis.md`
- [ ] Đổi tên reflection → `reflection_[HọTên].md`
- [ ] Push GitHub
