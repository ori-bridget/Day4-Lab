# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 27 skeleton, trung bình 15.04 khớp có v > 0 mỗi người
- Tổng: v=2 340 | v=1 66 | v=0 53

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 3 | 2 | 11% |
| 1 | left_eye | 20 | 3 | 4 | 11% |
| 2 | right_eye | 20 | 3 | 4 | 11% |
| 3 | left_ear | 15 | 7 | 5 | 26% |
| 4 | right_ear | 20 | 5 | 2 | 19% |
| 5 | left_shoulder | 25 | 2 | 0 | 7% |
| 6 | right_shoulder | 26 | 0 | 1 | 0% |
| 7 | left_elbow | 23 | 3 | 1 | 11% |
| 8 | right_elbow | 24 | 2 | 1 | 7% |
| 9 | left_wrist | 20 | 6 | 1 | 22% |
| 10 | right_wrist | 19 | 6 | 2 | 22% |
| 11 | left_hip | 21 | 5 | 1 | 19% |
| 12 | right_hip | 21 | 4 | 2 | 15% |
| 13 | left_knee | 18 | 3 | 6 | 11% |
| 14 | right_knee | 19 | 4 | 4 | 15% |
| 15 | left_ankle | 13 | 4 | 10 | 15% |
| 16 | right_ankle | 14 | 6 | 7 | 22% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
