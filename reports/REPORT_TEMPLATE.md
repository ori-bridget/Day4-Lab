# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Phù Ngô Việt Anh   Nhóm: Solo   Ngày: 16/09

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 27 |
| v=2 / v=1 / v=0 | 340 / 66 / 53 |
| Thời gian trung bình mỗi ảnh | Chưa có dữ liệu thời gian trong repo |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 26% (7/27)
2. `left_wrist`: 22% (6/27)
3. `right_wrist`: 22% (6/27; `right_ankle` cũng đồng hạng 22%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

Trả lời: `left_ear` có tỷ lệ bị che cao nhất, phù hợp với các ảnh có tóc hoặc mũ bảo hiểm che tai. Hai cổ tay thường nằm sau tay lái, thân xe hoặc thân người nên hay cần ước lượng vị trí, nhưng đó là vấn đề bị che chứ không đồng nghĩa với ra ngoài khung. Hông có tỷ lệ `v=1` đáng kể (19% ở `left_hip`, 15% ở `right_hip`) vì khó xác định mốc giải phẫu dưới quần áo dài; đây là khó định vị, không chỉ là che khuất.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | Chưa có snapshot | 0.9078 |
| OKS@0.50 | Chưa có snapshot | 0.9310 |
| OKS@0.75 | Chưa có snapshot | 0.8621 |
| Lỗi `dao_trai_phai` | Chưa có snapshot | 0 |
| Lỗi `nham_nguoi` | Chưa có snapshot | 0 |
| Lỗi `xoa_khop_bi_che` | Chưa có snapshot | 7 |

Lưu ý: `outputs/eval_vs_gold.json` chỉ chứa một lần chạy; cột “Sau rework” là lần chạy hiện có, còn số liệu trước rework chưa được lưu nên không thể tính mức cải thiện.

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

-
- Repo không có log hoặc file JSON của lần chạy trước, nên chưa thể xác nhận thao tác sửa giữa hai lần chạy.
- Lần chạy hiện có vẫn ghi nhận cần rà lại: `train_15.jpg`, người 1, `right_wrist`, `left_knee`, `left_ankle` (đều là `xoa_khop_bi_che`).
- Các điểm thiếu được script chỉ ra thêm: `train_15.jpg` người 1 `right_eye`; người 2 `right_shoulder`, `right_elbow`, `right_hip`; `train_16.jpg` người 2 `right_eye`.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

Trả lời: Không có lỗi `dao_trai_phai` trong `outputs/eval_vs_gold.json`. Tuy vậy, `check_pose_labels.py` cảnh báo dấu hiệu đảo trái/phải ở `train_02.txt`, người 1, tại cặp vai và cặp hông; đây là ảnh có người đi xe đạp nên cần mở visualization để xác nhận, không nên coi cảnh báo là lỗi đã kết luận.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_ear` | 26% | 3% (gold reference) | 22 điểm % | Guideline về tai bị tóc/mũ che chưa đủ cụ thể; cần phân biệt bị che với ngoài khung |
| `left_wrist` | 22% | 7% (gold reference) | 15 điểm % | Guideline về cổ tay sau tay lái/thân xe cần ví dụ và vị trí ước lượng rõ hơn |

Ghi chú: `reports/visibility_compare.md` và `outputs/visibility_compare.json` đã được tạo trong lượt kiểm này. Cột “Họ” là protected gold reference dùng để đối chiếu kỹ thuật, không phải số liệu của một bạn học khác.

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-
- Nếu khớp còn nằm trong khung nhưng bị vật thể, tay lái, tóc, mũ hoặc thân người che thì vẫn đặt điểm ước lượng và dùng `v=1`; chỉ dùng `v=0` khi vị trí giải phẫu thực sự nằm ngoài mép ảnh.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   Trả lời: `pose_mAP50-95` tăng `0.0055` (từ `0.6853` lên `0.6908`), nên nhánh “nếu giảm” không áp dụng. Dữ liệu core có nhiều mẫu che khuất bởi xe, tay lái, mũ bảo hiểm và quần áo; fine-tune có thể giúp model thích nghi nhẹ với các mẫu này. Đổi lại, `box_mAP50-95` giảm `0.0078`, cho thấy cải thiện pose nhỏ đi kèm suy giảm nhẹ khả năng tổng quát hóa box trên tập test.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Trả lời: Ở sau fine-tune, `box_mAP50-95 - pose_mAP50-95 = 0.8041 - 0.6908 = 0.1133`; ở baseline, khoảng cách là `0.8119 - 0.6853 = 0.1266`. Model tìm người dễ hơn tìm đúng vị trí 17 khớp, vì box chỉ cần bao quanh người còn pose phải định vị từng khớp, đặc biệt các khớp bị che hoặc khó thấy.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Trả lời: Ở ảnh test `test_03` trong output prediction của notebook, người lái xe đang chuyển động trên nền đất bụi và một detection chỉ có confidence `0.32`; các điểm tay/chân bị lệch khỏi trục cơ thể. Đây phù hợp nhất với lỗi **lệch nhẹ** (một vài điểm trượt do chuyển động/che khuất), chưa có bằng chứng để gọi là đảo trái/phải hay nhầm người. Notebook không lưu ground-truth overlay theo từng keypoint nên kết luận này dựa trên visualization trực quan.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Trả lời: Ảnh thấp nhất là `train_11` với OKS `0.599`. Nhãn của bài được gold ủng hộ mạnh hơn: cùng ảnh đạt OKS nhãn-so-gold `0.9038` và không có lỗi `dao_trai_phai`, `nham_nguoi` hay lỗi khớp nghiêm trọng trong báo cáo. Vì vậy, bằng chứng hiện có nghiêng về model sai do cảnh che khuất, không phải nhãn sai.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   Trả lời: Không. Ảnh gán tệ nhất là `train_15.jpg`, người 1, OKS so với gold `0.6588`; ảnh model bất đồng nhất là `train_11` với `0.599`. Điều này cho thấy hai vấn đề khác nhau: `train_15` có các lỗi visibility/thiếu khớp quanh xe máy, còn `train_11` là một cảnh khó đối với model vì người bị con mèo che lớn.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

Trả lời: Ở `train_15.jpg`, người 1, chọn `right_wrist` là `v=1`. Visualization cho thấy cánh tay và vùng cổ tay bị tay lái/thân xe che, nhưng vị trí khớp vẫn nằm trong vùng ảnh và có thể ước lượng từ khuỷu tay, bàn tay cùng hướng của cẳng tay. Vì vậy phải đặt chấm tại vị trí ước lượng và dùng `v=1`; không dùng `v=0`, vì `v=0` chỉ dành cho khớp thực sự nằm ngoài mép ảnh. Báo cáo gold hiện đang chỉ ra đúng điểm này là một lỗi `xoa_khop_bi_che` cần rà lại.
