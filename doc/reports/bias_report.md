# Đánh giá LLM Judge Phase B — trạng thái chạy

Nhóm: BA  
Người phụ trách: Nguyen Ngoc Han - 2A202602511

## Trạng thái và bằng chứng

**Chưa có kết quả judge/calibration thực nghiệm.** Ngày 12/10/2026,
`OPENAI_API_KEY` đã được xác nhận có trong `.env` và `config.py` nạp được key;
giá trị key không được hiển thị hoặc ghi vào báo cáo. Lệnh
`python scripts/run_experiments.py --phase b --live` dừng do thiếu
`artifacts/answers_50q.json`. Không có lần gọi judge/binary judge, run timestamp,
hoặc provenance model nào được ghi nhận; không có giá trị κ hay bias rate để báo cáo.

Nguồn cặp theo hợp đồng vẫn là A=`artifacts/answers_50q.json:answer`,
B=`fixtures/human_labels_10q.json:model_answer`, reference=`fixtures/test_set_50q.json:ground_truth`;
IDs là 1, 5, 12, 21, 41. Chưa tồn tại candidate A của pipeline nên không có 5 cặp
được gửi chấm. Chưa có 10 binary judgments trên model_answer gốc, vì vậy không đưa
human labels vào judge và không suy đoán kết quả.

| Bằng chứng | Kết quả hiện có |
|---|---|
| Phase B mock/unit tests | Pass trong suite offline (86 passed toàn suite) |
| `artifacts/judge_results.json` | Chưa được tạo |
| Pairwise swap results | Chưa chạy |
| Binary judgments / Cohen’s κ | Chưa chạy / chưa tính |

## Position và verbosity

Position inconsistency count/rate và verbosity win rate: **chưa đo**. Không có
các lượt judge hoặc candidate pair để xác định case không nhất quán, winner dài
hơn, observed agreement hay chance agreement.

## Giới hạn và cách hoàn tất

Unit tests dùng mock chỉ kiểm tra cấu trúc lời gọi, đổi winner/scores sau swap,
cách tính κ và bias; không phải bằng chứng judge API đã được gọi. Cần cung cấp
module RAG còn thiếu, tạo answers thật, cấu hình `OPENAI_API_KEY`, sau đó chạy
`python scripts/run_experiments.py --phase b --live`. Khi artifact có, cập nhật
báo cáo bằng 5 cặp hai lượt, đúng 10 supplied model_answers và nhãn người chỉ
dùng để đối chiếu sau khi judge; κ từ 10 cặp nhỏ vẫn cần được xem là ước lượng,
không phải phán quyết tuyệt đối.
