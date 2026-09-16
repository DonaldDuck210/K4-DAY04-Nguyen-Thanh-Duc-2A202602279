# Mini guideline - nhóm: SOLO  |  người gán: Nguyễn Thanh Đức  |  ngày: 16/9/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Nếu hông và vùng xương chậu vẫn còn hình dạng cơ thể nhìn thấy qua quần áo, cho `v = 1` và đặt chấm; chỉ `v = 0` khi hông đã bị cắt hẳn hoặc che hoàn toàn bằng vật thể. | Hông là điểm nằm ở vùng đứt khúc giữa thân và chân, nên phần quần áo dài có thể che một phần nhưng vẫn còn “đường trung tâm cơ thể” để ước lượng. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu mép tai còn thấy được ít nhất 1-2 cạnh hoặc có thể xác định vị trí dựa trên hình dáng đầu và vị trí của mắt/mũi, chọn `v = 1`; nếu chỉ thấy tóc hoặc mũ che kín hoàn toàn, chọn `v = 0`. | Điểm này dễ nhầm với “chỉ có cảm giác” nên cần căn cứ rõ ràng: có mặt hình dáng đầu và vị trí tương đối của tai. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Nếu phần thân còn visible vẫn đủ để xác định vị trí khớp trong khung, giữ `v = 1`; nếu khớp nằm ngoài khung hoặc không có căn cứ đặt điểm, chọn `v = 0`. | Quy tắc gắn với “vẫn còn trong khung hay không”, không phải “trông có giống người hay không”. |
| Cổ tay nằm sau tay lái / sau thân mình | Nếu cổ tay vẫn nằm trên đường cơ thể và có thể đoán được vị trí từ cánh tay hoặc khuỷu tay, `v = 1`; nếu toàn bộ cổ tay bị ẩn hoàn toàn, `v = 0`. | Phần ẩn vì vật cản không đồng nghĩa với khớp đã xoá khỏi hình; cần nhìn vào mốc giải phẫu liền kề. |
| Hai người chồng lên nhau | Dùng torso, hông và hướng đầu để phân biệt người; không dựa vào “người ở trái/phải của ảnh” khi chồng lên nhau. | Khi chồng lớp, việc nhầm người dễ xảy ra hơn nhiều so với lệch vị trí keypoint. |
| Người nhỏ đến mức nào thì không gán nữa | Vẫn gán nếu có đủ 3-4 điểm nhận dạng trên cơ thể và có thể suy ra đường thân/chi; nếu không thể xác định rõ thân thể và khớp nào cũng mơ hồ, bỏ hoặc đặt `v = 0` cho phần không có căn cứ. | Mục tiêu là giữ skeleton có thể học được; không được “xoá khớp” chỉ vì người nhỏ. |

Với mỗi luật, cần có ảnh mẫu trong CVAT. Với các điểm không có bề mặt nhìn thấy trực tiếp, phải căn cứ vào hình dáng cơ thể và mốc tương đối, không chỉ viết “cẩn thận hơn”.

## 3. Ba ca mơ hồ đã gặp

### Ca 1 - ảnh `train_13.jpg`, người thứ `1`, khớp `left_ear` / `right_ear`

- Mơ hồ ở chỗ nào: Tóc và góc nghiêng đầu che một nửa tai, còn mép trên của đầu và hướng của mặt đủ để suy ra tai nhưng không thấy rõ sắc nét.
- Bạn quyết thế nào: Chọn `v = 1` nếu đường đầu và vị trí đối xứng cho phép ước lượng; nếu tai gần như bị phủ hoàn toàn bởi tóc thì gán `v = 0`.
- Vì sao: Khớp tai là khớp dễ “che nhưng vẫn có thể đoán vị trí” và không nên xóa khỏi skeleton chỉ vì có tóc che.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học nhầm vị trí đầu/điều hướng đầu và khiến nhầm lệch chiều trái-phải hoặc nhả khớp trên đầu.

### Ca 2 - ảnh `train_13.jpg`, người thứ `2`, khớp `left_knee` / `right_knee`

- Mơ hồ ở chỗ nào: Đầu gối nằm gần thân, có quần áo và góc chồng người làm mất đường nét rõ của khớp.
- Bạn quyết thế nào: Giữ `v = 1` nếu đầu gối còn nằm trên đường cong chân và có thể ước lượng từ cẳng chân; không đặt `v = 0` nếu khớp vẫn còn trong khung hình.
- Vì sao: Đầu gối là điểm nằm trong quỹ đạo thân-chân, nên khi phần trên còn thấy và mốc hình dáng chân còn rõ thì vẫn có thể ước lượng vị trí.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model có thể gán đầu gối ra ngoài khung hoặc bỏ hẳn điểm đầu gối, làm méo hình dạng chân khi người ngồi/là ngắn.

### Ca 3 - ảnh `train_12.jpg`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay bị che bởi thân người hoặc góc cánh tay gập, khiến phần đầu tay không thấy rõ.
- Bạn quyết thế nào: Nếu có thể thấy đường cánh tay nối tới vị trí cổ tay thì giữ `v = 1`; nếu không còn điểm căn cứ nào để xác định, đặt `v = 0`.
- Vì sao: Cổ tay là khớp dễ bị ẩn khi tay đặt sau thân hoặc bị vật cản che, nhưng vẫn phải là `v = 1` nếu phần tay còn “đang còn nằm trong cơ thể”.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ học nhầm độ cong tay, làm lệch các khớp cánh tay và kỹ thuật đánh giá pose trong ảnh nhiều người.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `59%`; họ chưa có bảng so sánh trong repo)
- Nguyên nhân là **guideline chưa rõ**: tai dễ bị che bởi tóc hoặc mũ, nên ranh giới giữa “vẫn còn căn cứ đặt chấm” và “không còn căn cứ” cần được mô tả rõ ràng.
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
  - Nếu khớp vẫn còn nằm trong đường viền cơ thể và có thể ước lượng được vị trí dù bị che, chọn `v = 1` và giữ chấm.
  - Chỉ chọn `v = 0` khi khớp thật sự nằm ngoài khung hoặc không còn căn cứ nào để định vị.
