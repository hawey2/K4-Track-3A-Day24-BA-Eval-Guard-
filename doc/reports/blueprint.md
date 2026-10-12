# Blueprint CI/CD cho RAG và guardrails — đề xuất, chưa được triển khai

Nhóm: BA  
Người phụ trách: Nguyen Ngoc Han - 2A202602511

Đây là kiến trúc đề xuất. Ba live phases chưa tạo artifacts nên chưa có timestamp,
model provenance hay số đo thực nghiệm để báo cáo.

## Kiến trúc và failure action

Luồng đề xuất: input → Presidio PII scan/anonymize → NeMo input rails → RAG retrieval,
reranking và generation → NeMo output rails → response. Ở request thực, PII hoặc
input rail block thì dừng sớm; dữ liệu nhạy cảm cần redact trước log, không ghi raw
PII. Output bị flag thì không trả nguyên văn câu trả lời. API keys chỉ lấy từ secret
store/biến môi trường; không đưa vào source, report hoặc log. Fallback extractive
chỉ được cho phép khi có policy rõ ràng, phải gắn nhãn mode và không được trình bày
như generation LLM.

`OPENAI_API_KEY` đã được xác nhận có trong `.env` và được cấu hình nạp thành công;
giá trị không được tiết lộ. Key không khắc phục được các blocker độc lập là thiếu
pipeline `m1`–`m5` cho A/B và thiếu thư viện Presidio/NeMo cho C.

## Phạm vi latency và bằng chứng hiện có

Task 12 yêu cầu hai warm-up thật và tối thiểu 20 timed samples trong scope
`input_layers_sequential`, đo Presidio rồi NeMo input mọi lần, percentile tuyến tính.
Ngày 12/10/2026, lệnh
`python scripts/run_experiments.py --phase c --live --latency-samples 20` dừng ngay
khi import Presidio (`ModuleNotFoundError: presidio_analyzer`). Vì không có samples,
latency hiện là **chưa đo**; không có budget pass/fail thực nghiệm.

| Layer / scope | P50 ms | P95 ms | P99 ms | Trạng thái |
|---|---:|---:|---:|---|
| Presidio | chưa đo | chưa đo | chưa đo | Chưa cài Presidio |
| NeMo input | chưa đo | chưa đo | chưa đo | Chưa cài NeMo Guardrails |
| Input guard sequential | chưa đo | chưa đo | chưa đo | Không có timed samples |
| RAG | chưa đo | chưa đo | chưa đo | Ngoài phạm vi Task 12; chưa có pipeline |
| NeMo output | chưa đo | chưa đo | chưa đo | Ngoài phạm vi Task 12; chưa cài NeMo |
| Full guard / short-circuit request | chưa đo | chưa đo | chưa đo | Không suy từ input guard |

Kế hoạch đo: trong runtime Python 3.11 đã cài đủ stack, khởi tạo engine trước đo,
dùng 8 clean inputs, chạy hai warm-up thật, ghi ≥20 raw samples theo từng layer
bằng `perf_counter`, rồi tính P50/P95/P99 nội suy tuyến tính. Đo riêng end-to-end
request và RAG/output nếu cần đánh giá production.

## CI gates đề xuất

- Phase A: chạy 50 câu; đề xuất faithfulness ≥0.75 và avg_score ≥0.65; lưu per-question,
  distribution summaries và nguồn evaluator.
- Phase B: kiểm tra đúng nguồn pair, 5 cặp swap, 10 binary judgments trên supplied
  model_answer; dùng κ làm tín hiệu review, không auto-pass tuyệt đối.
- Phase C: tối thiểu lab 15/20 attacks và clean 8/8 allowed; gate đề xuất ≥18/20
  attacks, output cases đúng, không phát sinh clean false positives.
- Chỉ đặt full-guard P95 <500ms gate sau khi có phép đo end-to-end thực sự.

Đề xuất pipeline CI chạy unit tests trước, kiểm tra schema/provenance bằng
`python scripts/check_lab.py`, rồi chạy quality evaluation ở môi trường có API key
được quản lý an toàn. Các quality thresholds trên là đề xuất, chưa được cấu hình
thành workflow hoặc enforced bởi runner hiện tại.

## Monitoring và cải tiến

Hiện chưa có số liệu RAGAS, κ, attack pass rate, clean false-positive rate hoặc
latency. Khi có batch thật, theo dõi các chỉ số này theo model/version; lưu aggregate
và mã lỗi đã redact, theo dõi drift/false positives, review lại các ca bị chặn nhầm
và cập nhật fixtures. Giới hạn đánh giá cần công bố: bộ lab nhỏ 50/20/8/4, phụ thuộc
model và một số lượt judge; không đủ để khẳng định production readiness.

Các bước có thể kiểm chứng tiếp theo: (1) tích hợp và kiểm tra interface m1–m5;
(2) sinh 50 answers thật và chạy Phase A; (3) chạy Phase B với đúng hai nguồn
candidate và đối chiếu nhãn sau judge; (4) cài guard runtime, chạy Phase C với clean
controls và raw latency; (5) chỉ khi có JSON artifacts mới tính gates, cập nhật
reports và đánh giá cải thiện qua batch lặp lại.
