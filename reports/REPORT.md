# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Thanh Đức   Nhóm: Solo   Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 319 / 145 / 29 |
| Thời gian trung bình mỗi ảnh | 5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear — 59%
2. right_ear — 45%
3. left_wrist — 41% (cùng mức với left_hip 41%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Có, ba khớp này phù hợp với nhận xét thực tế về mức che khuất: tai dễ bị che bởi tóc hoặc mũ, còn cổ tay và hông thường bị che khi người cúi đầu, tay đặt sau thân hoặc quần áo dài che khu vực hông. Đây không phải là “khó xác định giải phẫu” mà là “khó phân biệt bị che nhưng còn trong khung” với “ra ngoài khung”, nên cần quy tắc nhìn thấy + căn cứ vị trí chứ không dựa vào cảm giác. Trong báo cáo visibility, `%v=1` cao ở các điểm này phản ánh độ che tương đối hơn là vị trí sai.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9171 | 0.9373 |
| OKS@0.50 | 0.95 | 1.0000 |
| OKS@0.75 | 0.97 | 1.0000 |
| Lỗi `dao_trai_phai` | 2 | 1 |
| Lỗi `nham_nguoi` | 1 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13.jpg`, người 1, sửa toàn bộ cặp trái/phải để khắc phục lỗi `dao_trai_phai`.
- `train_13.jpg`, người 2, điều chỉnh `left_knee` và `right_knee` để giảm lệch nhẹ trong vùng đầu gối khi có hai người chồng nhau.
- `train_12.jpg`, người 1, điều chỉnh `right_ear`, `left_shoulder`, `left_wrist` để đồng bộ visibility và tránh gán `v=1`/`v=2` sai theo góc nhìn.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải xuất hiện ở `train_13.jpg`, người 1. Ảnh này khó vì có hai người chồng nhau và đường người bị cúi, dẫn đến việc dễ lẫn giữa cặp vai/đùi theo hướng nhìn của ảnh thay vì theo người. Đây là kiểu lỗi “đúng vị trí nhưng nhầm trái-phải”, vì vậy cần xác định theo cơ thể người chứ không dựa vào góc hình ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_ear | 59% | Chưa có dữ liệu so sánh | Không xác định được trong repo | Dễ bị che bởi tóc/mũ; cần thống nhất guideline về “vị trí vẫn còn căn cứ” |
| right_ear | 45% | Chưa có dữ liệu so sánh | Không xác định được trong repo | Có thể nhầm giữa “bị che nhưng còn trong khung” và “ra ngoài khung” |
| left_wrist | 41% | Chưa có dữ liệu so sánh | Không xác định được trong repo | Tay đặt sau thân người / tay lái làm dễ lẫn giữa v=1 và v=0 |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Nếu khớp vẫn còn nằm trong đường viền cơ thể và có thể ước lượng được vị trí dù bị che, chọn `v=1` và giữ chấm; chỉ chọn `v=0` khi khớp đã ra ngoài khung hình hoặc không còn căn cứ để đặt điểm.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng 0.0055 sau fine-tune, từ 0.6853 lên 0.6908. Điều này cho thấy mô hình học thêm được một chút khả năng khớp người từ nhãn của mình, nhưng mức tăng không lớn; vì dataset chỉ 20 ảnh nên tín hiệu học tập bị giới hạn. Vì không giảm, không có bằng chứng rằng nhãn của mình làm hỏng kiến thức COCO ở giai đoạn này.

2. Chênh giữa `box_mAP50-95` và `pose_mAP50-95` là 0.8041 - 0.6908 = 0.1133 ở thời điểm sau fine-tune, và 0.8119 - 0.6853 = 0.1266 ở baseline. Như vậy, nhận diện người dễ hơn nhận diện khớp rõ rệt. Đó là điều hợp lý vì khớp là nhánh khó hơn, phụ thuộc nhiều vào góc nhìn, che khuất và bất đồng về vị trí giải phẫu.

3. Cần xem prediction-level output chi tiết để xác định ảnh nào model đoán sai. Trong repo hiện tại, không có file per-image inference error rõ ràng, nên không thể xác định loại sai theo bốn loại của slide 43 với độ tin cậy đủ. Cần chạy notebook hoặc export prediction detail nếu muốn nêu ảnh cụ thể.

4. Cũng như câu 3, ảnh có OKS thấp nhất của model không có trong output level detail đang có. Chỉ với file `eval_model.json`, không thể xác định ảnh test nào là thấp nhất hay ai “đúng” giữa nhãn và model. Cần thêm output per-image hoặc chạy notebook để đối chiếu từng ảnh.

5. Tương tự, không có dữ liệu per-image để so sánh “ảnh gán tệ nhất” và “ảnh model đoán tệ nhất” trong repo hiện tại. Cần thêm file đánh giá theo ảnh hoặc xem lại notebook nếu muốn trả lời cách định lượng.

## 5. Một rule evidence bạn đã dùng

Trong `train_13.jpg`, người 2, `left_knee` là một ví dụ rõ ràng cho quy tắc `v=1` khi khớp còn trong khung nhưng bị che. Dù đầu gối nằm gần thân và có một phần bị che bởi quần áo hoặc chồng lên chân người khác, vẫn có thể ước lượng vị trí từ đường cong cơ thể và góc khớp. Vì phần đầu gối còn nằm trong hình và không vượt ra khỏi khung ảnh, nên quy tắc đúng là `v=1`, không phải `v=0`. Điều này giúp phân biệt giữa “bị che nhưng còn có căn cứ đặt điểm” với “ra ngoài khung và không đặt điểm được”.
