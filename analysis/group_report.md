# Lab 18 Report — Cá nhân

**Họ tên:** [Điền tên]  
**Ngày:** 22/06/2026

> Bài tập cá nhân — một người implement toàn bộ M1–M5.

## Modules & Tests

| Module | File | Hoàn thành | Tests |
|--------|------|------------|-------|
| M1 Chunking | `src/m1_chunking.py` | ✅ | 13/13 |
| M2 Hybrid Search | `src/m2_search.py` | ✅ | 5/5 |
| M3 Reranking | `src/m3_rerank.py` | ✅ | 5/5 |
| M4 Evaluation | `src/m4_eval.py` | ✅ | 4/4 |
| M5 Enrichment | `src/m5_enrichment.py` | ✅ | 10/10 |

## RAGAS Scores

| Metric | Naive | Production | Δ |
|--------|-------|------------|---|
| Faithfulness | _điền_ | _điền_ | _điền_ |
| Answer Relevancy | _điền_ | _điền_ | _điền_ |
| Context Precision | _điền_ | _điền_ | _điền_ |
| Context Recall | _điền_ | _điền_ | _điền_ |

## Key Findings

1. **Biggest improvement:** Hybrid BM25 + dense + RRF tìm được chunk tiếng Việt tốt hơn dense-only baseline
2. **Biggest challenge:** Version conflict (v2023 vs v2024) và enrichment tốn API + thời gian
3. **Surprise finding:** RRF fix nhỏ (return đúng `SearchResult`) mới chạy được end-to-end hybrid

## Presentation Notes

1. RAGAS: so sánh naive vs production sau `python main.py`
2. Biggest win: M2 hybrid + M3 rerank cho query "nghỉ phép"
3. Case study: failure do retrieve document superseded — xem `analysis/failure_analysis.md`
4. Next 1h: metadata version filter + giảm enrichment calls
