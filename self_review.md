# Tự đánh giá — Nguyễn Tiến Đạt · G03

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S05_ir.png` | `frame_meta` (tag) | `missing` | Ở lần export đầu tiên bị quên gắn tag `frame_meta` cho khung `S05_ir.png` (bị `cabin_check` báo 1 ERROR) | Đã sửa (đã gắn đủ `frame_meta` với `modality=ir, light=unknown, camera_blocked=no`) |
| `S04_rgb.png` | `object` (`seat=rear_right`, `type=box`) | `missing` | Đã vẽ box `object` (`type=box`) ở ghế `rear_right` trên `S04_ir.png` nhưng quên copy sang bản `S04_rgb.png` | Đã sửa ở bản `export_final_v2.zip` |
| `D06.png` | `face` (`eye_right`) & `face13` | `flag` | Ở bản đầu tiên bật cờ `occluded=1` cho cả 4 điểm `right_eye_*` trên `face13` nhưng trên box `face` lại để `eye_right=open` gây cảnh báo mâu thuẫn | Đã sửa (đổi `eye_right=unknown` khớp với các điểm mắt bị che sau kính) |
| `D05.png` | `face13` (`left_eye_*`, `right_eye_*`) | `ok` | Kiểm tra lại quy ước trái/phải trên ảnh xoay 90°: `left_eye_outer` có `y=620.5 > right_eye_outer (y=523.3)`, đã bật đủ cờ `occluded=1` cho 8 điểm mắt khi `eye_left=eye_right=unknown` | Không có lỗi (đã ghi giải thích cho WARN trục x trong `check_final.txt`) |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

- **Lỗi:** Quên đồng bộ đầy đủ đối tượng/thuộc tính giữa cặp ảnh `_ir` và `_rgb` của cùng một cảnh OMS (hoặc quên gắn tag `frame_meta` trước khi vẽ box như ở `S05_ir.png`, và quên copy box `object` từ `S03_ir`/`S04_ir` sang `S03_rgb`/`S04_rgb`, hoặc vẽ box `face` trên `S08_ir.png` rộng hơn `S08_rgb.png`).
- **Vì sao mình mắc:** Khi chuyển qua lại giữa các khung hình SVIRO, mình vẽ trực tiếp từng ảnh hoặc copy-paste thiếu một số box phụ (`object`), đồng thời đôi khi bắt đầu vẽ box ngay mà chưa bấm tạo tag `frame_meta` ở bước đầu tiên.
- **Lần sau tránh bằng cách:** Tuân thủ nghiêm ngặt quy trình 4 bước ở mục 4.3 của Guideline: (1) Luôn đặt tag `frame_meta` đầu tiên ngay khi mở ảnh → (2) Gán hoàn chỉnh trên bản nhìn rõ hơn rồi dùng `Ctrl+C` / `Ctrl+V` copy toàn bộ shape sang ảnh cặp của nó, sau đó chỉ chỉnh sửa lại các thuộc tính không nhìn thấy thành `unknown` → (3) Kiểm tra nhanh số lượng box ở danh sách Objects bên phải giữa cặp `_rgb` và `_ir` → (4) Bấm `Ctrl+S`.

## 3. Ghi chú về disagreement_log

- **Một dòng bạn của mình đúng và mình sai:** Dòng `S04_rgb.png | object | box | chỉ có ở peer` (và tương tự ở `S03_rgb.png | object | box` cùng `S08_ir.png | face | box`). Trên `S04_rgb.png`, bạn cùng nhóm (`peer`) đã gán đầy đủ box `object` (`seat=rear_right, type=box`), trong khi mình chỉ gán box này trên `S04_ir.png` mà bỏ sót trên `S04_rgb.png`; ngoài ra ở `S08_ir.png`, bạn vẽ box `face` ôm sát từ trán đến cằm đúng mục 4.1 hơn box của mình (vốn bị lấn lên phần đỉnh tóc).
- **Mình học được gì từ dòng đó:** Review chéo bằng `cabin_check.py diff` cực kỳ hữu ích để phát hiện các lỗi bỏ sót đối tượng (`missing box`) giữa cặp ảnh RGB/IR và lỗi kích thước hộp (`box tightness`) mà script `cabin_check.py check` đơn lẻ trên 1 file không thể tự phát hiện được vì script không có ground-truth.
