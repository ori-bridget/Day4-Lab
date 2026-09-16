# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Phù Ngô Việt Anh   Người kiểm: Nghiêm Việt Quân   Ngày:16/09

> Ghi chú: theo yêu cầu, trường thông tin người gán và người kiểm được để trống để điền sau.
> Lượt kiểm đã dùng checker, visualization và gold reference. Bảng visibility đối chiếu là
> `reports/visibility_compare.md`.

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels <bài của họ>
python3 tools/visualize_pose.py --images dataset/images/train --labels <bài của họ> --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare <bài của họ>
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☐ | Mỗi skeleton hiện có đủ 17 điểm, nhưng gold có 29 người còn bài có 27: thiếu 2 skeleton ở `train_13`. |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☐ | `train_02.txt`, người 1: cảnh báo vai và hông có dấu hiệu trái/phải ngược; cần mở visualization. |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Không có finding `nham_nguoi` trong `eval_vs_gold.json`; vẫn nên soi trực quan `train_03`, `train_13`, `train_14`. |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☐ | Có 7 lỗi `xoa_khop_bi_che`: `train_08` (left_ankle), `train_12` (left_knee, left_ankle), `train_15` (right_wrist, left_knee, left_ankle), `train_16` người 2 (nose). |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☐ | Script cảnh báo các skeleton có nhiều `v=0` dù người nằm trong ảnh: `train_04`, `train_10`, `train_13`, `train_15`, `train_16`. |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | File chỉ dùng các cờ hợp lệ `0/1/2`; không có trường `Hidden` trong định dạng YOLO. |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | `annotations/coco_keypoints/person_keypoints_default.json`: 20 ảnh, 27 annotation; tất cả mảng `keypoints` dài 51. |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | `data.yaml` ghi `[17, 3]`; checker đọc đủ 20/20 file và 27 skeleton. |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑* | Có `reports/visibility_report.md`, `outputs/visibility_report.json` và `reports/visibility_compare.md`; bảng đối chiếu dùng gold reference. |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑* | Đã ghi 6 luật nhóm và 3 ca mơ hồ; ảnh mẫu là visualization hiện có của repo, chưa phải screenshot trực tiếp từ CVAT. |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | Exit thành công và báo “ĐẠT định dạng”; đồng thời có 9 cảnh báo nội dung cần rà, không phải lỗi format. |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_15.jpg` | 1 | `right_wrist` | Xoá khớp bị che: bài `v=0`, gold `v=1` | Đặt chấm ước lượng sau tay lái và đổi thành `v=1`. |
| `train_15.jpg` | 1 | `left_knee`, `left_ankle` | Xoá khớp bị che: bài `v=0`, gold `v=1` | Giữ điểm trong khung, đặt lại theo chuỗi chân và dùng `v=1`. |
| `train_13.jpg` | 1–2 | Toàn skeleton | Gold có 2 người nhưng bài thiếu | Gán bổ sung đủ 17 điểm cho hai người nhỏ ở nền; không bỏ người vì kích thước. |
| `train_15.jpg` | 1–2 | `right_eye`, `right_shoulder`, `right_elbow`, `right_hip` | Một số khớp bị để `v=0` dù gold có | Đặt điểm ước lượng và dùng `v=1` hoặc `v=2` theo mức nhìn thấy. |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này:
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**?

Trả lời:

- Lỗi lặp đi lặp lại nhiều nhất là dùng `v=0` cho khớp bị che hoặc khó nhìn thấy, thay vì đặt chấm và dùng `v=1`; lỗi này tập trung ở `train_04`, `train_10`, `train_15` và `train_16`.
- Đây chủ yếu là lỗi guideline chưa cụ thể hóa bằng ví dụ “còn trong khung nhưng bị vật che”, kèm một phần lỗi thao tác khi gán nhanh. Luật bổ sung là: còn trong khung thì luôn đặt chấm; chỉ dùng `v=0` khi khớp thật sự nằm ngoài mép ảnh.

Đối chiếu chi tiết và nhật ký review đã được ghi tại `reports/review_partner.md`; chưa tự sửa các file nhãn/gold trong lượt review này để giữ nguyên bằng chứng cần rework.
