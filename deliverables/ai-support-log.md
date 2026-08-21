# AI Support Log

> Ghi lại các bước sử dụng AI khi thực hiện capstone deliverables.

| # | Bước | AI dùng để làm gì | Bạn kiểm chứng kết quả thế nào |
|---|------|-------------------|-------------------------------|
| 1 | P1. Thiết kế User Input Grid & Dataset | Sinh các câu hỏi paraphrase, bồi ràng buộc thực tế (viết tắt `sd`, `j`, không dấu, giả định sai). | Kiểm tra đối chiếu độ phủ 100% tài liệu trong `manifest.json` và slide Day 20. |
| 2 | P2. Human Baseline & Agreement | Phân tích tỉ lệ đồng thuận (% agreement) và trích xuất danh sách 6 cases bất đồng. | Chạy script Python `eval/agreement.py` đối chiếu nhãn 3 người (Bính, Mỹ, Vinh). |
| 3 | P3. Rubric & Routing Map | Gợi ý cấu trúc bảng Rubric v1 và đề xuất làn phân loại Routing (Code vs LLM Judge vs Expert). | Rà soát tính khả thi: chuyển các rule deterministic (JSON schema, doc_id, security) về Code Check. |
| 4 | P4. Calibration & Judge Evaluation | Phân tích Confusion Matrix và đề xuất cải tiến prompt cho LLM Judge. | Chạy `eval/judge.py` để tính % agreement giữa Judge và Nhãn vàng `labels.csv`. |

### Quyết định kiểm soát chất lượng:
- **Phần AI gợi ý mà bạn BÁC BỎ**: Ban đầu AI gợi ý dùng LLM Judge để kiểm tra định dạng JSON và cite doc_id; tôi đã bác bỏ và chuyển toàn bộ sang làn **Code Check** (`eval/code_checks.py`) để giảm 100% chi phí API và đạt độ chính xác tuyệt đối.
- **Phần bạn HOÀN TOÀN TỰ LÀM**: Thảo luận và chốt nhãn vàng chung cho 6 case bất đồng (`S03_1`, `S04_1`, `S05_1`, `S05_2`, `S13_1`, `S13_2`), trực tiếp viết tiêu chí Blocker cho Rubric v1, và ra quyết định Gate Ship/Chưa Ship.
