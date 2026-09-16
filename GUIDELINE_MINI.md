# Mini guideline - nhóm: solo  |  người gán: Bùi Thanh Minh Hoàng  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài |che khuất | vẫn đoán được|
| Tai bị tóc hoặc mũ bảo hiểm che một phần |che khuất| vẫn đoán được| 
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) |bỏ từ hông trở xuống, không gán | vì không thấy toàn bộ cơ thể |
| Cổ tay nằm sau tay lái / sau thân mình |che khuất| vẫn đoán được |
| Hai người chồng lên nhau |cố gắng gán riêng từng người, không gộp chung nếu có thể |2 người khác nhau|
| Người nhỏ đến mức nào thì không gán nữa |không gán nếu  | người nhỏ hơn người cầm ảnh |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `1`, khớp `right_ankle,left_ankle`

- Mơ hồ ở chỗ nào: Người này bị cắt ngang, khó xác định vị trí cổ chân.
- Bạn quyết thế nào: bỏ cổ chân.
- Vì sao: Không thấy toàn bộ cơ thể.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể học sai quy luật về.

### Ca 2 - ảnh `train_01`, người thứ `2`, khớp `right_ankle,left_ankle`

- Mơ hồ ở chỗ nào: Người này bị cắt ngang, khó xác định vị trí cổ chân.
- Bạn quyết thế nào: bỏ cổ chân.
- Vì sao:Không thấy toàn bộ cơ thể.
- Nếu người khác quyết ngược lại thì model học sai cái gì:Model có thể học sai quy luật về.

### Ca 3 - ảnh `train_02`, người thứ `1`, khớp `nose,right_eye,left_eye,right_ear,left_ear`

- Mơ hồ ở chỗ nào: Che khuất.
- Bạn quyết thế nào: gán che khuất
- Vì sao:Không thấy toàn bộ mặt nhưng vẫn ước lượng được vị trí.
- Nếu người khác quyết ngược lại thì model học sai cái gì:Model có thể học sai quy luật về.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `59%` / họ `61%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:guideline chưa rõ
- Luật mới bổ sung vào mục 2 sau khi thống nhất:

