# Khai báo dùng AI — Nguyễn Tiến Đạt

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Antigravity AI | Đọc tài liệu trong repo (`README.md`, `GUIDE.md`, `CHECKPOINTS.md`, `RULES.md`, `SUBMISSION.md`) và tóm tắt lộ trình các bước cần thực hiện cùng quy tắc gán nhãn OMS/DMS | Nắm rõ trình tự thực hiện từ Task 0 đến Task 4, danh sách thuộc tính cần chú ý cho từng loại nhãn và quy định bảo mật ảnh trong `G03/` | Có, đối chiếu trực tiếp với `GUIDE.md` và `RULES.md` |
| 2 | Antigravity AI | Hỗ trợ thiết lập Task 0 trên CVAT Local (`http://localhost:8080`): nạp `labels_cabin_v1.json` vào project `Day12` và tạo task `Day12-G03` với 22 ảnh cốt lõi, cập nhật thuộc tính `occupied` trên CVAT | Mở trực tiếp giao diện CVAT Local để tự tay gán nhãn toàn bộ 22 ảnh của bộ `G03` | Có, kiểm tra trên giao diện web `localhost:8080` thấy đủ 9 nhãn và đúng 22 frames |
| 3 | Antigravity AI | Chạy script `scripts/cabin_check.py` cho các mốc `check_cpA.txt`, `check_cpB.txt`, `check_final.txt` và chạy `cabin_check.py diff` với `peer_export_final.zip`, hỗ trợ rà soát câu chữ cho các file báo cáo (`ir_log.csv`, `class_list_v0.md`, `self_review.md`) | Phát hiện lỗi thiếu tag `frame_meta` ở `S05_ir.png` và bất đồng cờ mắt ở `D06.png` để sửa lại trên CVAT, hoàn thiện bảng xử lý `disagreement_log.csv` và các tài liệu nộp E1–E5 | Có, kiểm tra lại bằng lệnh `python scripts/cabin_check.py check --expect 22` báo `0 lỗi` và `python -m zipfile -l` xác nhận file zip không chứa ảnh |
