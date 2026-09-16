# Review partner report

## Phạm vi review

Lượt review độc lập cho bài hiện tại dùng ba nguồn: `tools/check_pose_labels.py`, visualization trong
`outputs/vis_train/`, và đối chiếu với protected reference ở `gold/labels/train/`.
Đối chiếu visibility được ghi riêng tại `reports/visibility_compare.md` và
`outputs/visibility_compare.json`; đây là gold reference, không phải nhãn của một bạn
học khác.

## Kết quả tổng quát

| Hạng mục | Kết quả review |
| --- | --- |
| File ảnh/nhãn đọc được | 20/20 |
| Skeleton của bài | 27 |
| Skeleton trong gold | 29 |
| Mỗi skeleton có đủ 17 điểm | Đạt về định dạng |
| COCO keypoints | 20 ảnh, 27 annotation, mỗi `keypoints` có 51 số |
| YOLO Pose | `kpt_shape: [17, 3]`, mỗi dòng 56 số |
| OKS trung bình | 0.9078 |
| OKS@0.50 / OKS@0.75 | 0.9310 / 0.8621 |
| Lỗi đảo trái/phải đã kết luận bởi gold | 0 |
| Lỗi nhầm người đã kết luận bởi gold | 0 |
| Lỗi xoá khớp bị che | 7 |
| Người bị thiếu | 2, đều ở `train_13.jpg` |

## Lỗi cần người gán rework

| Ảnh | Người | Khớp / phạm vi | Bằng chứng | Hành động đề nghị |
| --- | ---: | --- | --- | --- |
| `train_02.jpg` | 1 | Vai và hông trái/phải | Checker cảnh báo thứ tự trái/phải có dấu hiệu ngược chiều với mắt | Mở visualization, xác nhận theo cơ thể người rồi đổi cặp trái/phải nếu cần. |
| `train_04.jpg` | 1 | 4 khớp `v=0` | Người nằm trong ảnh nhưng có nhiều khớp outside | Khớp còn trong khung phải đặt chấm và đổi thành `v=1` nếu bị che. |
| `train_04.jpg` | 2 | 7 khớp `v=0` | Người nằm trong ảnh nhưng có nhiều khớp outside | Rà từng khớp theo mép ảnh; không dùng `v=0` chỉ vì không thấy bề mặt. |
| `train_08.jpg` | 1 | `left_ankle` | Gold `v=1`, bài `v=0` | Đặt chấm ước lượng và dùng `v=1`. |
| `train_10.jpg` | 1 | 4 khớp `v=0` | Checker cảnh báo người nằm trong ảnh | Rà lại các khớp sau xe/tay lái; dùng `v=1` nếu còn trong khung. |
| `train_12.jpg` | 1 | `left_knee`, `left_ankle` | Gold `v=1`, bài `v=0` | Đặt lại theo chuỗi chân và dùng `v=1`. |
| `train_13.jpg` | 1–2 | Toàn skeleton | Gold có 3 người, bài có 1; thiếu 2 người | Gán đủ 17 điểm cho hai người nhỏ ở nền. |
| `train_13.jpg` | 1 | 5 khớp `v=0` | Checker cảnh báo người nằm trong ảnh | Rà lại v=0 theo mép ảnh thật, không theo độ khó nhìn thấy. |
| `train_15.jpg` | 1 | `right_eye` | Gold nhìn thấy, bài `v=0` | Đặt chấm và dùng `v=1` hoặc `v=2` theo mức thấy được. |
| `train_15.jpg` | 1 | `right_wrist`, `left_knee`, `left_ankle` | Gold `v=1`, bài `v=0`; 3 finding `xoa_khop_bi_che` | Giữ điểm trong khung, ước lượng vị trí và dùng `v=1`. |
| `train_15.jpg` | 2 | `right_shoulder`, `right_elbow`, `right_hip` | Gold có `v=2`, bài `v=0` | Gán lại ba khớp nhìn thấy và dùng `v=2`. |
| `train_16.jpg` | 2 | `nose`, `right_eye` | Gold có khớp, bài để `v=0` | Đặt chấm theo mặt; dùng `v=1` nếu bị che, `v=2` nếu nhìn rõ. |

## Nhận xét visibility

Trong bảng đối chiếu với gold, chênh lệch lớn nhất là `left_ear`: bài 26% `v=1`,
gold 3%, lệch 22 điểm phần trăm. Tiếp theo là `left_wrist`: 22% so với 7%, lệch 15
điểm. Đây là tín hiệu cần thống nhất rõ hơn về tai sau mũ/tóc và cổ tay sau tay lái;
không nên tự động coi mọi khác biệt visibility là lỗi vị trí.

## Kết luận reviewer

Bài đạt định dạng kỹ thuật và không có lỗi đảo trái/phải hoặc nhầm người được gold kết
luận. Bài chưa đạt hoàn toàn về độ bao phủ và visibility vì thiếu 2 skeleton ở `train_13`
và còn 7 trường hợp xoá khớp bị che. Sau khi rework các dòng trên, nên chạy lại:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python3 tools/visibility_report.py --labels dataset/labels/train \
    --out outputs/visibility_report.json --markdown reports/visibility_report.md
python3 tools/evaluate_pose_annotations.py --pred dataset/labels/train \
    --gold gold/labels/train --images dataset/images/train --out outputs/eval_vs_gold.json
```
