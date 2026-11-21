# Danh sách lớp v0 — Phạm Hữu Minh · G07

Viết tiếp từ slide 54. Mỗi lớp trả lời: model cần nhận ra cái này để làm gì?

Cột cuối mình tách riêng NIR thật (D01–D06, Drive&Act) với IR mô phỏng
(S01–S10, SVIRO), vì GUIDE §3 nói IR-sim không tái hiện đúng hiện tượng của NIR
thật. Gộp hai cái lại là tự lừa mình.

## DMS (8 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Nhìn thấy trên IR? |
|---|---|---|---|---|
| 1 | `face.eye_left`, `eye_right` | open / closed / unknown | Đo thời gian nhắm mắt để cảnh báo buồn ngủ. Gán riêng từng mắt, vì một mắt bị che không nói gì về mắt kia | NIR thật: có, trừ khi đốm phản xạ rơi đúng tròng kính |
| 2 | `face.gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Đếm thời gian mắt rời đường. Ở xe L2–L3 còn dùng để biết tài xế có sẵn sàng nhận lại tay lái không | Có. Kính râm che mất hướng mắt, chỉ còn suy từ hướng đầu nên hay phải ghi `other` |
| 3 | `face.glasses` | none / clear / sun / unknown | Báo cho phần xử lý mắt biết khi nào đừng tin trạng thái mắt. Lớp điều kiện, không phải lớp cảnh báo | Có, nhưng NIR làm kính râm trông như kính trong. Ghi `none` nhầm là dạy model rằng người đeo kính thì không có kính |
| 4 | `phone` + `state` | held / mounted / lap | Phát hiện dùng điện thoại cầm tay. `mounted` là hợp lệ, `held` mới là vi phạm, nên giá trị thuộc tính mới quyết định chứ không phải cái box | Có, nhận bằng hình dạng. NIR không thấy ánh màn hình |
| 5 | `hand` + `on_wheel` | yes / no / unknown | Biết tay có trên vô lăng không, để tính thời gian tiếp quản. Bộ D## là xe tự lái nên `no` là bình thường | Có |
| 6 | `person.seatbelt` | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây. `worn_incorrect` (dây sau lưng, dưới nách) là ca nguy hiểm mà đèn nhắc thắt dây thường bỏ qua | Một phần. Dây sẫm trên áo sẫm là chìm luôn |
| 7 | `face13` | 13 điểm, mỗi điểm có cờ | Suy hướng đầu để biết mắt rời đường, và tính tỉ lệ khép mí. Cờ `occluded`/`outside` mới là phần có giá trị: nó dạy model chỗ nào không có dữ liệu | Có, với mặt rộng từ 60 px |
| 8 | `frame_meta.camera_blocked` | yes / no | Phát hiện camera bị che hoặc bị bịt. Không có lớp này thì "không thấy tài xế" và "camera bị che" lẫn vào nhau, mà hai cái phải xử lý khác nhau | Có |

## OMS (8 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Nhìn thấy trên IR? |
|---|---|---|---|---|
| 1 | `person.seat` | driver / front_passenger / rear_left / rear_middle / rear_right | Biết ghế nào có người, để nhắc thắt dây đúng ghế và bật tắt túi khí đúng chỗ. Gọi theo xe vì hệ thống trên xe chỉ hiểu ghế theo xe | Có |
| 2 | `child_seat` + `occupied` | no / unknown | Tắt túi khí ở ghế có ghế trẻ em. Không có giá trị `yes` vì trẻ ngồi trong đó đã là `person` rồi | Có, nhận bằng hình dạng |
| 3 | `infant_seat` + `facing` | rear / front | Nôi quay lưng về trước thì bắt buộc tắt túi khí ghế đó. `facing` mới là trường quyết định an toàn, không phải cái box | Có, nhận bằng hình dạng |
| 4 | `object` + `type` | bag / box / cup / other | Phân biệt đồ vật với người, để xe không nhắc thắt dây cho một cái ba lô. Lớp chống báo giả | Có, nhận bằng hình dạng và vị trí chứ không bằng màu |
| 5 | `person.seatbelt` (hàng sau) | not_worn / unknown trên SVIRO | Nhắc thắt dây cho cả hàng ghế sau, chỗ người ta hay quên nhất | Một phần, như lớp 6 bên DMS |
| 6 | Ghế trống (không vẽ box) | — | Tổng ghế có người cộng ghế trống phải bằng số ghế của xe. Đây là phép kiểm đếm, nên dù không vẽ box vẫn phải đếm vào ghi chú | — |
| 7 | `frame_meta.modality` | rgb / ir | Cho model biết khung thuộc miền cảm biến nào. Model học trên ảnh màu rơi rất mạnh khi gặp ảnh hồng ngoại, nên phải tách miền ngay từ nhãn | — (siêu dữ liệu) |
| 8 | `frame_meta.light` | day / night / unknown | Điều khiển phơi sáng và quyết định lúc nào bật đèn hồng ngoại. Trên SVIRO luôn `unknown` vì là ảnh dựng | — (siêu dữ liệu) |

## 2 câu hỏi leo thang

1. **Câu hỏi:** Bốn trên tám cảnh SVIRO cốt lõi có trẻ em hoặc em bé **đang
   ngồi** trong ghế, nhưng GUIDE §3 lại nói "Bộ ảnh không có trẻ em ngồi. Thấy
   hình giống trẻ em thì báo coach, không gán." Và schema không diễn đạt được ca
   này: `child_seat.occupied` chỉ có `no` và `unknown`, cố ý bỏ `yes`; còn
   `infant_seat` không có trường nào nói ghế đang có người. Làm đúng §3 thì bỏ
   trống nửa lô. Vậy phải gán thế nào?

   Khung ví dụ: `S01_rgb` (trẻ ngồi ghế trẻ em đỏ), `S03_rgb` và `S05_rgb` (em bé
   nằm trong nôi), `S08_rgb` (trẻ ngồi cạnh người lớn).

   Em tạm gán thế nào và vì sao: vẽ **hai box riêng** — `person` cho trẻ và
   `child_seat`/`infant_seat` cho ghế, `occupied = unknown` vì không có `yes` để
   chọn. Em theo cách này vì §4.8 đã có tiền lệ "vật nằm trong ghế trẻ em thì hai
   box riêng", và vì giữ được dữ liệu còn hơn bỏ trống. Nhưng `unknown` ở đây là
   sai sự thật: em nhìn thấy rõ là có người ngồi.

   Chỗ em nghĩ là gốc của vấn đề: **slide 54 của bài giảng có `occupied (yes · no
   · unknown)`.** Giá trị `yes` bị bỏ ở `labels_cabin_v1.json` của buổi lab chứ
   không phải chưa từng tồn tại. Nên có lẽ chỉ cần khôi phục lại cho khớp slide.

2. **Câu hỏi:** `frame_meta.modality` chỉ có `rgb` và `ir`, nên NIR thật (D##)
   và IR mô phỏng (S##_ir) cùng mang một nhãn `ir`. Nhưng hai thứ đó khác nhau
   thật chứ không phải khác nhau trên lý thuyết. Có nên thêm `ir_sim`, hay thêm
   một thuộc tính `sensor` riêng?

   Khung ví dụ: mở `S03_ir` cạnh `D03`. Slide 22 xếp "ánh sáng giảm theo khoảng
   cách" là hiện tượng số 1 của ảnh IR thật, và ở `D03` thấy rõ: mặt tài xế sáng,
   nền sau chìm hẳn vào đen. Ở `S03_ir` thì cả khoang sáng đều, không có vùng tối
   dần nào. Cùng nhãn `ir`, mà một bên có hiện tượng còn một bên không.

   Em tạm gán thế nào và vì sao: gán `ir` cho cả hai đúng theo bảng 4.1, không tự
   đặt giá trị mới (RULES §4). Phần tách hai loại em ghi ở cột `source` của
   `ir_log.csv`, chỗ duy nhất guideline v1.0 cho phép phân biệt.
