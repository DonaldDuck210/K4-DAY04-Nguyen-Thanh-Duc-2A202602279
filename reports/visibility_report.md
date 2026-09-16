# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 319 | v=1 145 | v=0 29

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 8 | 0 | 28% |
| 1 | left_eye | 18 | 11 | 0 | 38% |
| 2 | right_eye | 20 | 9 | 0 | 31% |
| 3 | left_ear | 12 | 17 | 0 | 59% |
| 4 | right_ear | 16 | 13 | 0 | 45% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 20 | 9 | 0 | 31% |
| 8 | right_elbow | 24 | 5 | 0 | 17% |
| 9 | left_wrist | 17 | 12 | 0 | 41% |
| 10 | right_wrist | 18 | 10 | 1 | 34% |
| 11 | left_hip | 16 | 12 | 1 | 41% |
| 12 | right_hip | 17 | 11 | 1 | 38% |
| 13 | left_knee | 20 | 5 | 4 | 17% |
| 14 | right_knee | 20 | 5 | 4 | 17% |
| 15 | left_ankle | 15 | 5 | 9 | 17% |
| 16 | right_ankle | 13 | 7 | 9 | 24% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
