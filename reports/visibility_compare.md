# Visibility report

> Ghi chú: cột “đối chiếu” ở đây dùng protected gold reference
> (`gold/labels/train/`) để review độc lập. Đây không phải là bảng nhãn của một bạn học khác.

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 15.04 khớp có v > 0 mỗi người
- Tổng: v=2 340 | v=1 66 | v=0 53

So sánh với `gold\labels\train` (29 skeleton) - protected gold reference.
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 26% | 3% | 22 |
| 9 | left_wrist | 22% | 7% | 15 |
| 2 | right_eye | 11% | 0% | 11 |
| 13 | left_knee | 11% | 21% | 10 |
| 11 | left_hip | 19% | 28% | 9 |
| 0 | nose | 11% | 3% | 8 |
| 1 | left_eye | 11% | 3% | 8 |
| 12 | right_hip | 15% | 21% | 6 |
| 4 | right_ear | 19% | 14% | 5 |
| 7 | left_elbow | 11% | 7% | 4 |
| 8 | right_elbow | 7% | 3% | 4 |
| 6 | right_shoulder | 0% | 3% | 3 |
| 15 | left_ankle | 15% | 17% | 2 |
| 16 | right_ankle | 22% | 24% | 2 |
| 10 | right_wrist | 22% | 21% | 2 |
| 14 | right_knee | 15% | 14% | 1 |
| 5 | left_shoulder | 7% | 7% | 1 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
