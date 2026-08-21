# AI Support Log

> Nhật ký ghi lại quá trình phối hợp sử dụng AI khi thực hiện capstone deliverables (Day 20–21).

| # | Bước (Phase) | AI được dùng để làm gì | Bạn kiểm chứng & kiểm soát kết quả thế nào |
|---|---|---|---|
| 1 | **P1. Thiết kế User Input Grid & Dataset** | Gợi ý các câu hỏi paraphrase, bồi thêm ràng buộc nhiễu đời thực (viết tắt `sd`, `j`, không dấu, giả định sai). | Kiểm tra và đối chiếu 100% độ phủ tài liệu trong `manifest.json` và slide Day 20, chỉnh sửa lại tone giọng học viên. |
| 2 | **P2. Human Baseline & Agreement** | Phân tích tỉ lệ đồng thuận (% agreement) và tổng hợp 6 cases bất đồng từ 3 file nhãn cá nhân. | Chạy script Python `eval/agreement.py` kiểm chứng số liệu (Bính 80%, Mỹ 84%, Vinh 84%, tổng 76%). |
| 3 | **P3. Rubric & Routing Map** | Gợi ý cấu trúc Rubric v1 gồm 6 tiêu chí và đề xuất phân làn Routing 4 mức. | Rà soát tính khả thi: chuyển các rule có định dạng rõ ràng (JSON schema, doc_id, security) về làn **Code Check**. |
| 4 | **P4. Calibration Loop (Judge v1 & v2)** | Phân tích Confusion Matrix Vòng 1 và đề xuất thêm 3 ví dụ Near-Miss cho Judge v2. | Chạy `python3 eval/judge.py` kiểm chứng % agreement nhảy từ **46% $\rightarrow$ 58%**, copy data thô vào `evidence/`. |
| 5 | **P5. Slice Analysis & Scorecard** | Gom nhóm kết quả evaluation theo 4 lát cắt (Happy Path, Out-of-Scope, False Premise, High Risk). | Mở `report.html`, soi từng slice và đối chiếu với ngưỡng Gate chốt trước. |
| 6 | **P6. Trace Deep-Dive** | Gợi ý dàn ý phân tích nguyên nhân gốc rễ (Root Cause Analysis) cho 3 trace fail quan trọng. | Trực tiếp đọc tay từng dòng log trong `results.jsonl` cho các case `S04_1`, `S05_1`, `S05_2` để kết luận root cause. |
| 7 | **P7. Verdict & Class Pitch** | Gợi ý cấu trúc báo cáo PM 1 trang và kịch bản thuyết trình 2 phút trước lớp. | Tự ra quyết định Gate **HOLD / CHƯA SHIP**, viết bộ câu hỏi phản biện bảo vệ quan điểm trước Coach. |

---

### 🛡️ Quyết định Kiểm soát Chất lượng (Quality Control Decisions):

1. **Phần AI gợi ý mà bạn BÁC BỎ (Rejected AI Suggestions)**:
   - *Bác bỏ 1*: AI gợi ý dùng LLM Judge để kiểm tra định dạng JSON và trích dẫn `doc_id`. Tôi đã **bác bỏ** và chuyển toàn bộ sang làn **Code Check** (`eval/code_checks.py`) để đạt độ chính xác 100%, chi phí $0 và tốc độ tức thì.
   - *Bác bỏ 2*: AI gợi ý chấm `PASS` cho câu trả lời `S04_1` vì giải thích khái niệm G-Eval hay. Tôi đã **bác bỏ** và đánh `FAIL` vì Tutor đã vi phạm tiêu chí Blocker — trôi theo giả định sai của học viên mà không đính chính trước.

2. **Phần bạn HOÀN TOÀN TỰ LÀM (100% Human Work)**:
   - Thảo luận nhóm và chốt Nhãn vàng chung (Consensus Golden Labels) cho 6 case bất đồng (`S03_1`, `S04_1`, `S05_1`, `S05_2`, `S13_1`, `S13_2`).
   - Đặt trước các ngưỡng Gate Thresholds (Schema 100%, Security 100%, Groundedness $\ge 90\%$) trước khi chạy evaluation.
   - Đọc tay từng trace log để chẩn đoán nguyên nhân gốc rễ và ra quyết định Gate chính thức (**HOLD / CHƯA SHIP**).
   - Xây dựng kịch bản thuyết trình 2 phút và bộ khung trả lời phản biện trước lớp.
