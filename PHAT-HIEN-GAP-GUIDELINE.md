# Phát hiện trước buổi học — lô G07 có trẻ em ngồi, guideline nói là không có

Ngày 21/11/2026, soát 8 cảnh SVIRO cốt lõi của lô **G07** trước buổi lab.

## Guideline nói gì

GUIDE §3, mục SVIRO:

> **Không ai trong SVIRO thắt dây an toàn.**
> Bộ ảnh không có trẻ em ngồi. Thấy hình giống trẻ em thì báo coach, không gán.

Bảng 4.1 đi theo đúng giả định đó:

- `child_seat.occupied` chỉ có `no` và `unknown` — **cố ý không có `yes`**.
- `infant_seat` chỉ có `facing` (rear / front), **không có trường nào nói ghế có người**.
- §4.2 ghi rõ: "ghế trẻ em **trống**: `child_seat`; ghế em bé **trống**: `infant_seat`".

## Thực tế trong lô G07

| Cảnh | Nội dung | Có trẻ/em bé ngồi? |
|---|---|---|
| S01 | Trẻ ngồi trong ghế trẻ em màu đỏ, chăn kẻ caro bên trái ảnh | **Có** |
| S02 | Người lớn (nữ) bên phải ảnh, ghế trẻ em rỗng bên trái ảnh | Không |
| S03 | Em bé nằm trong nôi xách, ba lô đỏ bên trái ảnh | **Có** |
| S04 | Ghế trẻ em đỏ rỗng bên phải ảnh, ghế hoa văn rỗng bên trái | Không |
| S05 | Em bé nằm trong nôi xách bên trái ảnh | **Có** |
| S06 | Hai ghế trẻ em rỗng, hai bên | Không |
| S07 | Thùng giấy bên trái ảnh, ghế trẻ em rỗng ở giữa | Không |
| S08 | Trẻ ngồi bên trái ảnh, người lớn (nữ) bên phải ảnh | **Có** |

**4 trên 8 cảnh cốt lõi có trẻ em hoặc em bé đang ngồi.** Không phải một cảnh lọt
lưới, mà là một nửa lô.

## Vì sao đây là vấn đề chứ không phải chuyện nhỏ

1. **Làm theo guideline nguyên văn thì mất điểm.** §3 bảo "không gán" và "báo
   coach". Bỏ không gán 4 cảnh thì mục OMS (20 điểm của RUBRIC) không thể đạt,
   vì nó chấm "mỗi ghế có người hoặc vật: một box đúng loại".
2. **Schema không diễn đạt được.** Trẻ ngồi trong ghế trẻ em cần nói hai điều
   cùng lúc: có cái ghế, và ghế đó **đang có người**. `occupied` lại cố tình bỏ
   giá trị `yes`. Nôi có em bé còn tệ hơn: `infant_seat` không có trường nào về
   người ngồi.
3. **Cảnh đầu tiên của buổi đã dính.** Lịch trong GUIDE: 14:30 Task 1a làm
   S01–S02. S01 chính là cảnh có trẻ ngồi. Cả lớp sẽ gặp ngay ở ảnh đầu tiên.

## Ba cách xử, cần coach chốt

| Cách | Làm gì | Hệ quả |
|---|---|---|
| A | Gán trẻ là `person` (kèm `seat`, `seatbelt`) **và** ghế là `child_seat` với `occupied = unknown`, hai box riêng | Giữ được thông tin, nhất quán với luật "vật trong ghế trẻ em thì hai box" ở §4.8. Nhưng §3 nói đừng gán trẻ |
| B | Theo đúng §3: không gán, ghi vào ghi chú và báo coach | Đúng chữ nghĩa, nhưng 4/8 cảnh trống trơn và mục OMS không chấm được |
| C | Coach bổ sung `occupied = yes` vào schema v1.0 | Sạch nhất, nhưng phải sửa `labels_cabin_v1.json` và mọi người tạo lại project |

**Đề xuất: hỏi coach trước 14:00 và xin chốt một cách cho cả lớp.** Nếu không kịp
thì tạm làm theo A, ghi rõ vào ghi chú và vào `class_list_v0.md` như một câu hỏi
leo thang, vì A giữ được dữ liệu còn B thì vứt mất.

## Cách kiểm lại

Ảnh SVIRO rất tối. Chỉnh brightness/contrast trong **Image settings** của CVAT
(biểu tượng bánh răng trên canvas) là nhìn rõ ngay — thao tác đó chỉ đổi hiển
thị, không sửa file, đúng như §4.4-4 cho phép. Mở S01 rồi kéo sáng lên là thấy.

---

## Bổ sung sau khi đọc slide bài giảng (Slide_Day_12_-_DMS_v_OMS.pdf)

**Slide 54** ("Bản nháp danh sách lớp và thuộc tính: chiều nay viết tiếp") in đúng dòng này:

> `child_seat` box — seat · occupied (**yes** · no · unknown)

Nghĩa là **bản nháp của chính bài giảng CÓ giá trị `yes`.** Chỗ bỏ nó đi là
`labels_cabin_v1.json` và GUIDE §4.1 của buổi lab ("không có `yes`").

Vậy vấn đề không phải "xin thêm một giá trị mới vào guideline". Vấn đề là
**bản cài đặt của lab đã bỏ mất một giá trị mà slide bài giảng có, và lô ảnh
thì cần đúng giá trị đó.** Cách C ở bảng trên vì thế là cách rẻ nhất: khôi phục
`yes` cho khớp lại với slide 54.

Một chỗ lệch nữa giữa slide và bản cài đặt, nhỏ hơn nhưng nên hỏi luôn:

| | Slide 54 | `labels_cabin_v1.json` |
|---|---|---|
| `child_seat.occupied` | yes · no · unknown | no · unknown |
| metadata khung hình | modality · light · **camera_id** | modality · light · **camera_blocked** |
| `light` (slide 26) | ngày · đêm · hoàng hôn | day · night · unknown |

`camera_id` biến mất và `camera_blocked` xuất hiện thay chỗ. Hai trường này làm
hai việc khác hẳn nhau: `camera_id` để biết khung hình từ camera nào, còn
`camera_blocked` để biết ống kính có bị che không. Không cái nào thay được cái kia.

Slide 54 còn ghi ở chân: *"Task 4 buổi chiều: mỗi cặp sửa, thêm, bớt và ghi lý
do 'để làm gì' cho từng dòng."* Nên việc nêu mấy chỗ lệch này **chính là** Task 4,
không phải đi ngoài đề.
