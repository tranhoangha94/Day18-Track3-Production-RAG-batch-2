# Failure Analysis — Lab 18: Production RAG

**Bài tập:** Cá nhân (1 người implement M1–M5)  
**Ngày:** 22/06/2026

> Cập nhật số RAGAS sau khi chạy `python main.py`.  
> Lấy bottom-5 từ `reports/ragas_report.json` → field `failures` hoặc `per_question` scores thấp nhất.

---

## RAGAS Scores

| Metric | Naive Baseline | Production | Δ |
|--------|---------------|------------|---|
| Faithfulness | _điền_ | _điền_ | _điền_ |
| Answer Relevancy | _điền_ | _điền_ | _điền_ |
| Context Precision | _điền_ | _điền_ | _điền_ |
| Context Recall | _điền_ | _điền_ | _điền_ |

---

## Bottom-5 Failures

### #1

- **Question:** _từ ragas_report.json_
- **Expected:** _ground_truth_
- **Got:** _answer_
- **Worst metric:** _faithfulness | context_recall | context_precision | answer_relevancy_
- **Error Tree:** Output sai → Context đúng? → Query OK? →
- **Root cause:** _ví dụ: chunk version cũ (v2023) được retrieve thay vì v2024_
- **Suggested fix:** _metadata filter / rerank / chunking theo version_

### #2

- **Question:**
- **Expected:**
- **Got:**
- **Worst metric:**
- **Error Tree:** Output sai → Context đúng? → Query OK? →
- **Root cause:**
- **Suggested fix:**

### #3

- **Question:**
- **Expected:**
- **Got:**
- **Worst metric:**
- **Error Tree:** Output sai → Context đúng? → Query OK? →
- **Root cause:**
- **Suggested fix:**

### #4

- **Question:**
- **Expected:**
- **Got:**
- **Worst metric:**
- **Error Tree:** Output sai → Context đúng? → Query OK? →
- **Root cause:**
- **Suggested fix:**

### #5

- **Question:**
- **Expected:**
- **Got:**
- **Worst metric:**
- **Error Tree:** Output sai → Context đúng? → Query OK? →
- **Root cause:**
- **Suggested fix:**

---

## Case Study (presentation)

**Question chọn phân tích:** _ví dụ: "Nhân viên được nghỉ bao nhiêu ngày phép năm?" (version conflict v2023 vs v2024)_

**Error Tree walkthrough:**

1. Output đúng? → _LLM trả 12 ngày nhưng ground truth là 15 ngày_
2. Context đúng? → _Context chứa `nghi_phep_nam_v2023.md` thay vì v2024_
3. Query rewrite OK? → _Query không chỉ rõ năm hiện hành_
4. Fix ở bước: _M1 structure-aware + metadata `version` / M5 enrichment gắn ngày hiệu lực_

**Nếu có thêm 1 giờ, sẽ optimize:**

- Thêm metadata filter theo `effective_date` / `superseded`
- Giảm enrichment API calls (batch hoặc skip chunk ngắn)
- Tune RRF `k` và `top_k` sau khi xem failure analysis
