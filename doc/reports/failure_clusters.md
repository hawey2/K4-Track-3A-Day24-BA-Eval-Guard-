# Phân tích lỗi Phase A — trạng thái chạy

Nhóm: BA  
Người phụ trách: Nguyen Ngoc Han - 2A202602511

## Trạng thái và bằng chứng

**Chưa có kết quả RAGAS thực nghiệm để phân tích.** Ngày 12/10/2026, các lệnh
`python scripts/setup_answers.py --live` và `python scripts/run_experiments.py
--phase a --live` được thử từ gốc repo. Setup dừng vì thiếu cả năm module `m1`–`m5`;
runner Phase A dừng vì `artifacts/answers_50q.json` không tồn tại. Vì vậy không có
timestamp/model provenance, điểm theo distribution, bottom 10 hoặc matrix để báo cáo.
Không dùng fixture ground truth, mock hoặc số ví dụ thay cho kết quả.

| Bằng chứng | Kết quả hiện có |
|---|---|
| `src/m1_chunking.py`–`src/m5_enrichment.py` | Chưa được cung cấp |
| `artifacts/answers_50q.json` | Chưa được tạo |
| `artifacts/ragas_50q.json` | Chưa được tạo |
| Phase A offline unit tests | 6 tests trong toàn bộ suite — kết quả: pass (toàn suite 86 passed) |

## Điểm theo distribution và bottom 10

Chưa đo: faithfulness, answer relevancy, context precision, context recall và
avg_score cho factual (20), multi_hop (20), adversarial (10). Chưa có bottom 10
để đối chiếu question, answer, retrieved contexts và ground truth.

## Matrix và nhóm yếu nhất

Chưa tính được worst_metric × distribution matrix hoặc mean theo distribution;
không thể xác định nhóm yếu nhất hay metric phổ biến nhất khi chưa có 50 metric rows.

## Bẫy adversarial và giới hạn

Chưa thể nhận xét về version conflict/negation: chưa có answers thực tế, contexts
hoặc điểm evaluator. Các kiểm thử mock chỉ xác nhận logic adapter, không chứng minh
pipeline/RAGAS chạy thật. Cần cung cấp `m1`–`m5`, Python 3.11 và cấu hình runtime;
sau đó sinh answers bằng `python scripts/setup_answers.py --live`, chạy Phase A,
rồi thay báo cáo trạng thái này bằng phân tích đối chiếu các JSON sinh ra.
