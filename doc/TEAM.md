# Nhóm thực hiện K4 Track 3A

Nhóm: BA

Repo chung: https://github.com/hawey2/K4-Track-3A-Day24-BA-Eval-Guard

Người phụ trách: Nguyen Ngoc Han - 2A202602511

Ngày lab/deadline áp dụng: 12th Oct, 2026 — Asia/Ho_Chi_Minh (UTC+07:00)

Commit dùng để chấm: Chưa chốt — working tree đang có thay đổi chưa commit (base HEAD `b9f8d5b53bc4f94899aa30fdc52c30be0e96eb6d`).

Mỗi nhóm nộp một repo. Điền đủ thành viên và một mục INDIVIDUAL cho từng người.
Thêm/xóa rows để khớp nhóm thật; mẫu ba rows không ép nhóm phải có ba người.

## Thành viên và phân công

| STT | Họ và tên | MSSV | Vai trò / công việc thật | Bằng chứng |
|---|---|---|---|---|
| 1 | Nguyen Ngoc Han | 2A202602511 | Phase A/B/C implementation; đọc hướng dẫn/schema; chạy offline tests và thử các live commands | Các thay đổi hiện tại trong working tree; offline: 86 passed; live blockers được ghi trong `doc/reports/` |

Người tích hợp/chạy batch/nộp link: Nguyen Ngoc Han

## Môi trường và chuẩn bị trước lớp

OS/Python/compiler: macOS; Python 3.12.2 trong môi trường hiện tại (khác mục tiêu Python 3.11); compiler/Qdrant chưa xác minh.
Virtualenv/dependencies/imports: Chạy trong môi trường hiện tại, chưa xác minh là virtualenv; `ragas`, `openai`, `jsonschema` import được; NeMo Guardrails, Presidio analyzer/anonymizer chưa cài.
Models/Qdrant/50 answers đã chuẩn bị: `OPENAI_API_KEY` có trong `.env` và được `config.py` nạp; không hiển thị key. Chưa có `m1`–`m5`, setup live dừng do thiếu modules; chưa tạo 50 answers và chưa xác minh model/Qdrant.
Nếu cài full NeMo chưa thành công, ghi lỗi thực tế; không dùng unit test pass
hoặc pip resolve để khẳng định runtime đã chạy.

## INDIVIDUAL

Sao chép mẫu cho mỗi thành viên; tiêu đề họ tên không dấu-MSSV.

### NguyenNgocHan-2A202602511

- Phần được giao và phần thực tế đã làm: Hoàn thiện các hàm Phase A/B/C trong ba module `src/phase_*.py`; rà soát README, hướng dẫn nộp, schema và corpus docs.
- Bằng chứng: Các thay đổi hiện tại trong working tree; không có artifacts experiment.
- Cách kiểm tra: `python -m pytest tests/ -q` — 86 passed; `python -m pytest tests/ -m support -q` — 60 passed. Ba lệnh live đã được thử, nhưng setup dừng do thiếu m1–m5; A/B thiếu answers; C thiếu Presidio.
- Điều đã học: Mock tests xác minh hợp đồng/hàm nhưng không thay thế RAGAS, LLM judge, guard stack hoặc số liệu latency thật.
- Giới hạn/cách xử lý: Chưa có m1–m5, API key, Python 3.11, NeMo/Presidio; reports nêu rõ trạng thái thay vì tạo số liệu không có bằng chứng.
- Nguồn/AI hỗ trợ nếu có: OpenAI coding assistant hỗ trợ đọc hướng dẫn, triển khai và kiểm tra; kết quả tự kiểm chứng bằng unit tests và lệnh runner/checker. Cần tuân thủ chính sách AI của lớp.

## Cập nhật sau khi nộp

Chưa nộp; chưa có commit nộp. Cập nhật sau nộp: chưa áp dụng.
