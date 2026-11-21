# Danh sách lớp v0 — Nguyễn Tiến Đạt · G03

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face` + `eye_left` / `eye_right` | `open` / `closed` / `unknown` | Theo dõi trạng thái đóng/mở mắt theo chuỗi thời gian (PERCLOS) để phát hiện tài xế buồn ngủ, ngủ gật hoặc chớp mắt bất thường | Có trên NIR cả ngày lẫn đêm, trừ khi bị đốm phản xạ LED trên tròng kính hoặc kính râm chắn IR (khi đó là `unknown`) |
| 2 | `face.gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Xác định vùng chú ý của tài xế để cảnh báo mất tập trung (distraction) khi tài xế rời mắt khỏi đường (`road`) nhìn xuống đùi (`lap`) hoặc màn hình quá lâu | Có, dựa vào hướng đầu và vị trí đồng tử/mắt trên ảnh NIR |
| 3 | `face.glasses` | `none` / `clear` / `sun` / `unknown` | Nhận diện tài xế có đeo kính cận hay kính râm để hệ thống DMS tự điều chỉnh thuật toán ước lượng ánh nhìn (gaze) hoặc chuyển sang dựa vào hướng đầu (head pose) khi mắt bị lóa | Có (thấy gọng kính và đốm phản xạ LED), nhưng trên NIR kính râm đôi khi trông trong suốt nên khó phân biệt `clear` và `sun` |
| 4 | `face13` (13 điểm mốc mặt) | Tọa độ `(x, y)` + cờ `occluded` / `outside` | Ước lượng góc quay đầu 3D (yaw, pitch, roll), độ mở mí mắt (EAR) và độ mở miệng (MAR - phát hiện ngáp) phục vụ cảnh báo mệt mỏi và mất tập trung | Có, nhận diện rõ các mốc mắt, mũi, miệng trên NIR; điểm bị che bởi tay/kính cần gắn cờ `occluded` |
| 5 | `hand` (`whose`, `side`, `on_wheel`) | `driver`/`passenger` · `left`/`right` · `yes`/`no`/`unknown` | Kiểm tra trạng thái tay đặt trên vô lăng (Hands-On-Wheel Detection) để quyết định cảnh báo buông tay hoặc kiểm tra điều kiện chuyển giao quyền lái (takeover readiness) trên xe tự hành | Có, thấy rõ biên dạng bàn tay và vị trí tương đối với vô lăng trên ảnh NIR |
| 6 | `phone` (`state`) | `held` / `mounted` / `lap` | Phát hiện hành vi sử dụng điện thoại khi lái xe (cầm tay, nghe gọi, nhắn tin) để phát cảnh báo mất tập trung nguy hiểm mức độ cao | Có, nhận diện được qua hình dạng chữ nhật, góc cạnh hoặc màn hình phát sáng của điện thoại |
| 7 | `person.seatbelt` (cho `seat=driver`) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Kiểm tra tài xế có thắt dây an toàn và thắt đúng quy cách (vắt chéo qua vai–ngực) hay luồn dưới nách/sau lưng để cảnh báo an toàn | Có khi dải dây tương phản với áo; khó thấy (`unknown`) khi vùng ngực bị bóng tối hoặc trùng độ phản xạ NIR với áo |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | `front_passenger` / `rear_left` / `rear_middle` / `rear_right` | Đếm số lượng hành khách và xác định chính xác vị trí ghế có người ngồi để kích hoạt cảnh báo dây an toàn theo ghế và tối ưu điều hòa/túi khí | Có, nhận diện tốt qua hình dáng cơ thể ngay cả khi cabin tối |
| 2 | `person.seatbelt` (hành khách) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Nhắc nhở hành khách hàng ghế sau và ghế phụ thắt dây an toàn đúng cách trước và trong khi xe di chuyển | Có nếu đủ sáng hồng ngoại, nhưng dễ mất chi tiết (`unknown`) nếu dây chìm vào nền áo tối hoặc bị vật che |
| 3 | `infant_seat` (`seat`, `facing`) | `rear` / `front` theo từng vị trí ghế | Phát hiện ghế nôi em bé (đặc biệt loại quay ngược `facing=rear`) để tự động vô hiệu hóa túi khí ghế phụ (tránh chấn thương khi bung túi khí) và kích hoạt cảnh báo quên trẻ (CPD) | Có, nhận diện rõ qua hình khối đặc trưng và hướng đặt của nôi xách |
| 4 | `child_seat` (`seat`, `occupied`) | `no` / `yes` / `unknown` theo vị trí ghế | Phân biệt ghế trẻ em đang trống hay đang có trẻ ngồi để kích hoạt hệ thống Child Presence Detection (CPD) cảnh báo khi tài xế khóa cửa bỏ quên trẻ trong xe | Có (hình dạng khung ghế và hình thể bên trong), tuy nhiên vùng sâu trong lòng ghế bị bóng tối che phủ phải ghi `unknown` |
| 5 | `object` (`seat`, `type`) | `bag` / `box` / `cup` / `other` | Phát hiện đồ vật/hành lý bỏ quên trong cabin sau khi hành khách xuống xe (Forgotten Object Detection) và tránh báo động nhầm trọng lượng trên ghế là người | Có, nhận diện qua hình dáng hình học nhưng mất hoàn toàn thông tin màu sắc và chữ in trên bề mặt vật |
| 6 | `frame_meta` (`modality`, `light`, `camera_blocked`) | `rgb`/`ir` · `day`/`night`/`unknown` · `yes`/`no` | Giám sát tình trạng hoạt động của cảm biến camera cabin; phát hiện ngay khi ống kính bị che khuất/bẩn (`camera_blocked=yes`) để báo lỗi hệ thống DMS/OMS | Có, nhận biết trực tiếp từ toàn khung hình |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. **Câu hỏi:** Trong tài liệu Guideline v1.0 (mục 4.1 và 4.8) ghi `child_seat` chỉ có `occupied: no, unknown` (không có `yes`) và mục 3 nói bộ ảnh không có trẻ em ngồi, nhưng thực tế trong bộ ảnh SVIRO `G03` có nhiều khung hình xuất hiện mô hình trẻ em ngồi trực tiếp trong ghế trẻ em (và schema `labels_cabin_v1.json` của gói `G03` đã được cập nhật thêm giá trị `yes`). Vậy với trẻ em ngồi trong `child_seat`, ngoài việc chọn `child_seat (occupied=yes)`, chúng ta có cần vẽ thêm box `person` và `face` cho đứa trẻ đó giống như người lớn hay chỉ vẽ duy nhất 1 box `child_seat`?
   - **Khung ví dụ:** `G03 S01_ir.png` (ghế `rear_right`), `G03 S03_ir.png` / `S03_rgb.png` (ghế `rear_left`), `G03 S05_ir.png` / `S05_rgb.png` (ghế `rear_right`).
   - **Bạn tạm gán thế nào và vì sao:** Tạm gán 1 box `child_seat` với thuộc tính `occupied = yes` (theo đúng file `G03/labels_cabin_v1.json` và `cabin_schema.py` đã bổ sung giá trị `yes`) và không vẽ thêm box `person` chồng lên để tránh lỗi cảnh báo nhiều box chính trên cùng một ghế.

2. **Câu hỏi:** Ở các cặp ảnh SVIRO có độ sáng rất thấp trên bản RGB (như `S01_rgb.png` và `S06_rgb.png`), khi nhìn bằng mắt thường ở độ sáng mặc định gần như tối đen không thấy vật thể, nhưng khi tăng tối đa Brightness/Contrast (Gamma) trên CVAT hoặc đối chiếu với bản `_ir` cùng cảnh thì thấy mờ mờ đường viền ghế. Guideline quy định ngưỡng hiển thị như thế nào để quyết định một đối tượng trên ảnh RGB tối là "nhìn thấy được để vẽ box" hay "bỏ qua không vẽ box (coi như mất hoàn toàn trên RGB)"?
   - **Khung ví dụ:** `G03 S01_rgb.png` và `G03 S06_rgb.png`.
   - **Bạn tạm gán thế nào và vì sao:** Trên `S01_rgb.png` và `S06_rgb.png`, mình chỉ gắn tag `frame_meta` và không vẽ box cho các vật bị chìm hoàn toàn trong bóng tối của ảnh RGB (trong khi vẫn vẽ đầy đủ trên `S01_ir.png` và `S06_ir.png`), đồng thời ghi nhận sự khác biệt giữa RGB và IR-sim này vào `ir_log.csv` theo đúng quy tắc 4.4-1 ("Không thấy thì không đoán từ ảnh kia của cùng cảnh").
