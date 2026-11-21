# TEAM — Ngày 12

Mã bộ ảnh: **G03** · Hình thức: cá nhân (ghép cặp review chéo cùng mã G03)

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Nguyễn Tiến Đạt | 2A202603289 | có | Lê Minh Thắng (`thang` - `peer_export_final.zip`) | 64 |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- `G03 S01_ir, S03, S04_ir, S05, S06_ir, S07` (`child_seat`): Quy ước gán `child_seat` với thuộc tính `occupied = yes` khi có mô hình trẻ em ngồi trong ghế trẻ em (theo bản cập nhật `G03/labels_cabin_v1.json` và `scripts/cabin_schema.py`).
- `G03 S03_ir` (`object` ở ghế `rear_right`): Vật kích thước nhỏ ở góc ghế `rear_right` trên ảnh IR-sim khó phân biệt rõ là `cup` hay vật khác nên tạm gán `type = other` theo quy tắc 4.8.

## Cá nhân

Mỗi người một mục, 1–2 dòng mỗi ý.

### NguyenTienDat-2A202603289
- **Phần mình làm (ngoài 22 ảnh của mình, ví dụ ai review ai, ai tổng hợp ca hỏi coach):** Tự thiết lập CVAT local, gán đầy đủ 22 ảnh cốt lõi của bộ `G03`, chạy tự kiểm ở cả 3 mốc (`check_cpA.txt`, `check_cpB.txt`, `check_final.txt`), chạy `cabin_check.py diff` với bài của bạn Lê Minh Thắng để phân tích toàn bộ 64 dòng trong `disagreement_log.csv`, và cập nhật bản sửa lỗi `export_final_v2.zip` / `check_final_v2.txt`.
- **Điều mình học được hôm nay:** Nắm vững quy tắc bất biến khi gán nhãn cabin: vị trí ghế luôn gọi theo chiều xe (`rear_right` nằm bên trái ảnh OMS), mắt/tay trái–phải luôn gọi theo cơ thể người thật (trên ảnh DMS xoay 90° thì mắt trái có tọa độ `y` lớn hơn), không bao giờ đoán nhãn khi bị che/lóa kính mà phải dùng `unknown` đồng bộ với cờ `occluded`/`outside` trên `face13`.
