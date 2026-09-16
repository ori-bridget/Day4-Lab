# Mini guideline - nhóm: Solo  |  người gán: Phù Ngô Việt Anh  |  ngày: 16/09

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
| Hông của người mặc quần áo dài | Ước lượng theo trục vai–thân–đùi; nếu hông còn trong khung nhưng bị áo che thì đặt chấm và dùng `v=1`. | Hông là mốc giải phẫu, không đòi hỏi phải thấy bề mặt quần áo đúng tại khớp; chỉ dùng `v=0` khi vị trí hông thực sự nằm ngoài mép ảnh. Ảnh mẫu: `outputs/vis_train/train_15.jpg`. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Còn trong khung và bị che một phần thì đặt tại vị trí tai ước lượng, `v=1`; thấy rõ thì `v=2`; ra ngoài ảnh mới `v=0`. | Giữ phân biệt “bị che” và “ra ngoài”; các ảnh mũ bảo hiểm cho thấy điểm tai vẫn có thể suy ra từ mắt và hai bên đầu. Ảnh mẫu: `outputs/vis_train/train_04.jpg`. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp dưới mép ảnh dùng `v=0` và không đặt chấm; khớp còn trong khung vẫn gán đủ, bị che thì `v=1`. | Mép ảnh là căn cứ của `v=0`, không phải việc không nhìn thấy khớp do quần áo. Ảnh mẫu: `outputs/vis_train/train_10.jpg`. |
| Cổ tay nằm sau tay lái / sau thân mình | Cổ tay còn trong ảnh nhưng bị tay lái hoặc thân che thì đặt chấm ước lượng và dùng `v=1`. | Tay lái che không làm khớp ra khỏi khung; phải giữ tín hiệu vị trí cho model. Ảnh mẫu: `outputs/vis_train/train_15.jpg`. |
| Hai người chồng lên nhau | Gán trọn 17 điểm cho từng người; theo dõi chuỗi mắt–vai–hông–gối để giữ đúng danh tính, điểm bị người kia che nhưng còn trong ảnh dùng `v=1`. | Không lấy điểm của người phía trước gán sang người phía sau. Ảnh mẫu: `outputs/vis_train/train_16.jpg`. |
| Người nhỏ đến mức nào thì không gán nữa | Không có ngưỡng bỏ gán trong bộ core: mọi người xuất hiện đều phải có đủ 17 điểm; dùng zoom và visibility thay vì bỏ skeleton. | Bộ dữ liệu có cả người nhỏ ở nền; bỏ họ tạo lỗi thiếu người. Ảnh mẫu: `outputs/vis_train/train_13.jpg`. |

Ghi chú về ảnh mẫu: repo hiện có ảnh `visualize_pose.py` trong `outputs/vis_train/`, chưa có screenshot trực tiếp từ CVAT. Các đường dẫn trên là bằng chứng hình ảnh hiện có; nếu nộp theo rubric yêu cầu nghiêm ngặt, cần thay hoặc bổ sung bằng screenshot CVAT tương ứng.

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `______`, người thứ `___`, khớp `______`

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

Trả lời: Ca này là `train_02.jpg`, người 1, cặp `left_shoulder/right_shoulder` và `left_hip/right_hip`; người đi xe đạp quay lệch nên trái/phải trên ảnh dễ bị hiểu ngược. Mình quyết theo cơ thể người: dùng hướng mặt và chuỗi vai–khuỷu–cổ tay để xác định bên trái của chính người đó, không theo bên trái khung hình. Nếu đảo ngược, model sẽ học sai danh tính trái/phải và augmentation lật ảnh sẽ củng cố lỗi đó thêm một lần. `check_pose_labels.py` vẫn cảnh báo ca này, nên cần rà lại visualization trước khi khóa nhãn.

### Ca 2 - ảnh `______`, người thứ `___`, khớp `______`

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

Trả lời: Ca này là `train_15.jpg`, người 1, `right_wrist`, nằm sau tay lái/thân xe. Mình quyết định đặt chấm ở vị trí ước lượng và dùng `v=1` vì vùng khớp còn trong ảnh dù bề mặt không thấy rõ. Nếu dùng `v=0`, model sẽ học rằng tư thế có tay lái che thì cổ tay không tồn tại; gold cũng đã gọi đây là lỗi `xoa_khop_bi_che` của nhãn hiện có.

### Ca 3 - ảnh `______`, người thứ `___`, khớp `______`

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

Trả lời: Ca này là `train_13.jpg`, người nhỏ ở nền, các khớp `left_hip/right_hip` và chân khó thấy. Mình quyết định vẫn gán đủ 17 điểm cho từng người trong ảnh, dùng zoom và đặt `v=1` cho khớp còn trong khung nhưng bị nền/người khác che; không tự bỏ người vì nhỏ. Nếu bỏ người nhỏ, model sẽ học sai rằng người ở xa không cần phát hiện, và báo cáo gold hiện cho thấy ảnh này còn thiếu hai skeleton cần rà lại.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:

Trả lời: Đã tạo `reports/visibility_compare.md` và `outputs/visibility_compare.json` bằng cách đối chiếu bài với gold reference. Chênh lệch lớn nhất là `left_ear`: bài 26% `v=1`, gold 3%, lệch 22 điểm phần trăm; tiếp theo là `left_wrist`: 22% so với 7%, lệch 15 điểm. Đây là reference kỹ thuật để kiểm tra, không phải bảng của một bạn học; luật cần thống nhất là khớp còn trong khung nhưng bị vật thể che thì vẫn đặt điểm và dùng `v=1`, chỉ dùng `v=0` khi vị trí giải phẫu ở ngoài mép ảnh.
