# Reflection — Lab 18: Production RAG (Cá nhân)

**Họ tên:** [Điền tên]  
**Ngày:** 22/06/2026

---

## Phần 1: Mapping bài giảng → code

| Lecture Concept | Module | Hàm cụ thể | Observation |
|----------------|--------|-------------|-------------|
| Semantic chunking | M1 | `chunk_semantic()` | Threshold 0.85 gộp câu cùng chủ đề; ít chunk hơn `chunk_basic()` |
| Hierarchical chunking | M1 | `chunk_hierarchical()` | Parent ≤2048 chars, child ≤256 chars; child có `parent_id` để retrieve precision → trả parent context |
| Structure-aware chunking | M1 | `chunk_structure_aware()` | Split theo `#` header; metadata `section` giữ cấu trúc markdown |
| BM25 + Vietnamese tokenization | M2 | `segment_vietnamese()`, `BM25Search` | `underthesea` + replace `_` để query "nghỉ phép" khớp token BM25 |
| Dense + Hybrid RRF | M2 | `DenseSearch`, `reciprocal_rank_fusion()` | bge-m3 + Qdrant; RRF merge BM25+dense → `method="hybrid"` |
| Cross-encoder reranking | M3 | `CrossEncoderReranker.rerank()` | bge-reranker-v2-m3; doc "nghỉ phép" rank cao hơn VPN |
| RAGAS 4 metrics | M4 | `evaluate_ragas()`, `failure_analysis()` | Faithfulness/context recall thường là điểm yếu khi retrieve sai version |
| Contextual enrichment | M5 | `_enrich_single_call()`, `enrich_chunks()` | 1 API call/chunk; `response_format=json` + parse JSON ổn định hơn |

---

## Phần 2: Khó khăn & giải quyết

### Lỗi 1: Pipeline crash — không phải import

```
UnicodeEncodeError: 'charmap' codec can't encode characters
```

- **Debug:** Chạy `python src/pipeline.py` trên Windows console cp1252
- **Fix:** `sys.stdout.reconfigure(encoding="utf-8")` trong `pipeline.py`

### Lỗi 2: M5 enrichment

```
AttributeError: 'NoneType' object has no attribute 'get'
Enrichment API failed: Expecting value: line 1 column 1 (char 0)
```

- **Debug:** `_enrich_single_call()` trả `None` khi JSON parse fail; LLM trả markdown fence
- **Fix:** `return {}` fallback + `_parse_llm_json()` + `response_format={"type": "json_object"}`

### Lỗi 3: M2 RRF

```
AttributeError: 'dict' object has no attribute 'text'
```

- **Debug:** `reciprocal_rank_fusion()` return list dict thay vì `SearchResult`
- **Fix:** Build `SearchResult(..., method="hybrid")` từ RRF scores

### Kiến thức bổ sung

- Qdrant chạy qua Docker (`docker compose up -d`), không phải module Python
- RAGAS 0.4.x trong venv khác `requirements.txt` (<0.2) — vẫn chạy được với `evaluate()`

---

## Phần 3: Action Plan cho project

## Project: [Tên project RAG của bạn]

### Hiện tại

- RAG pipeline: chunk cố định + vector search đơn giản
- Known issues: retrieve sai version tài liệu, không có rerank, không đo RAGAS

### Plan áp dụng

1. [ ] **Chunking:** hierarchical (parent/child) cho corpus policy dài
2. [ ] **Search:** hybrid BM25 + dense + RRF (tiếng Việt cần word segmentation)
3. [ ] **Reranking:** CrossEncoder sau hybrid top-20 → top-3
4. [ ] **Evaluation:** RAGAS 4 metrics + failure analysis định kỳ
5. [ ] **Enrichment:** combined single-call cho chunk quan trọng (không enrich toàn bộ nếu tốn API)

### Timeline

- **Tuần 1:** Hybrid search + Qdrant + baseline RAGAS
- **Tuần 2:** Rerank + fix top failures từ failure analysis
- **Tuần 3:** Enrichment có chọn lọc + metadata version filter
