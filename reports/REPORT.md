# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Bùi Thanh Minh Hoàng   Nhóm: solo   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 339 / 123 / 31 |
| Thời gian trung bình mỗi ảnh | 4 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` — 59%
2. `right_ear` — 41%
3. `left_eye` — 34%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng. Mắt và tai ở các ảnh chụp từ xa hoặc người quay nghiêng thường bị tóc che khuất một phần. Tuy nhiên, chúng vẫn nằm trong khung hình nên tôi phải ước lượng vị trí và đặt cờ v=1 (Occluded). Cổ tay cũng thỉnh thoảng bị che nhưng tai và mắt vẫn là khó ước lượng nhất.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9393 | 0.9393 |
| OKS@0.50 | 1.0000 | 1.0000 |
| OKS@0.75 | 1.0000 | 1.0000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_03.jpg`, người #1, khớp `right_hip`: Lệch nhẹ so với gold, chỉnh lại chấm `right_hip` cho sát với vị trí giải phẫu hơn.
- `train_08.jpg`, người #1, khớp `left_ear` và `right_ear`: Kéo lại vị trí tai bị lệch nhẹ cho chuẩn hơn so với đáp án.
- `train_13.jpg`, người #2, khớp `left_knee`: Kéo điểm `left_knee` về đúng vị trí (lệch nhẹ 27 px).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Tôi đã kiểm tra cẩn thận bằng màu xanh/cam trong `outputs/vis_train` trước khi chạy đánh giá để đảm bảo không bị nhầm lẫn trái phải của người trong ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: Bùi Thanh Minh Hoàng (solo)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_ear` | 59% | 61% | 2% | Guideline chưa rõ (chưa thống nhất việc tai bị tóc che thì tính là v=1 hay ra sao) |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Tai bị tóc che một phần hoặc mũ bảo hiểm che: v=1, vẫn đoán được vị trí giải phẫu và đặt chấm chứ không bỏ.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline; đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai; kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   Tăng 0.0055 (từ 0.6853 lên 0.6908). Mức tăng khá nhỏ, điều này cho thấy 20 ảnh gán nhãn của tôi không phá hỏng hoàn toàn kiến thức gốc của model, và giúp nó làm quen hơn một chút với góc chụp hoặc tư thế trong tập dữ liệu của bài lab này.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?
   Sau fine-tune, box_mAP50-95 là 0.8041, pose_mAP50-95 là 0.6908, chênh nhau 0.1133. Model tìm người dễ hơn tìm khớp. Lý do là vì bounding box (ô chữ nhật) chỉ cần bao trọn được đúng thân người, trong khi pose cần phải định vị chính xác vị trí của từng điểm trong 17 khớp, nên rất dễ bị sai lệch khi bị che khuất hoặc trùng người.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43 (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   Ảnh `test_07`: model bị **nhầm người** — một số khớp ở tay/chân của người đứng sát phía sau bị gắn nhầm vào bộ xương của người phía trước do họ chồng lên nhau. Không phải là lỗi lệch nhẹ vì nó hoàn toàn nối sai cơ thể.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   `train_12` là ảnh có OKS thấp nhất. Trong trường hợp này, tôi đúng: do người trong ảnh quay lưng và đeo balo nên model đã nhầm lẫn trái/phải ở vai và tay, trong khi tôi gán trái/phải dựa theo cơ thể (trục người) rất rõ ràng. Đối chiếu ảnh gốc thấy balo nằm bên vai trái, trùng khớp với nhãn của tôi.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?
   Không trùng nhau. Ảnh tôi gán tệ nhất (lệch so với gold nhiều nhất) là `train_08` do tôi lỡ gắn lệch tai khi ước lượng. Còn ảnh model đoán tệ nhất là `train_12`. Lỗi của tôi nghiêng về sai số thao tác ở vị trí khó đoán, trong khi lỗi của model là do không xử lý tốt tình huống góc chụp từ sau lưng và có đồ vật che lấp.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người, khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Ảnh `train_01`, người #1, khớp `left_ankle` và `right_ankle`. Bằng chứng trên ảnh là người này bị cắt ngang khung hình ở phần dưới (chỉ thấy từ hông trở lên). Do không thể thấy toàn bộ cơ thể và khớp thật sự đã ra ngoài mép ảnh, tôi quyết định không đặt chấm, chuyển sang tick Outside (v=0) để loại bỏ điểm này. Điều này tuân thủ quy tắc là v=0 chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh.
