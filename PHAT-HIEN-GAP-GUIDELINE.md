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

---

## Cập nhật 22/11: đã có bản schema sửa, nhưng script kiểm thì chưa sửa theo

Sáng 22/11 xuất hiện một bản `labels_cabin_v1.json` khác, ở `D:\AIA\day12\`,
sửa lúc 00:03 ngày 22/11. So với bản trong repo khóa thì **khác đúng hai dòng**:

```
<           "no"
---
>           "no",
>           "yes"
```

Tức là `child_seat.occupied` được trả lại giá trị `yes`, đúng như slide 54.
Mọi thứ còn lại y hệt: 9 nhãn, mọi thuộc tính khác, cả phần `svg` của `face13`.
Bản trong gói ảnh `G07/` vẫn là bản cũ, không có `yes`.

### Đã áp dụng

- Thêm `yes` vào schema của project trên CVAT.
- Bốn box `child_seat` ở `S01` và `S08` (mỗi cảnh hai modality) đổi từ
  `occupied = unknown` sang `occupied = yes`. Trước đó phải ghi `unknown` dù
  nhìn rõ là có người ngồi, vì không có giá trị nào khác để chọn.
- Theo đúng §3 ("thấy hình giống trẻ em thì báo coach, không gán"), các box
  `person` và `face` của trẻ em và em bé đã được gỡ. Thông tin "ghế này có
  người" giờ nằm ở `occupied = yes` chứ không nằm ở một box `person`.

### Vấn đề còn lại 1 — script kiểm chưa khớp với schema mới

`scripts/cabin_schema.py` dòng 48 vẫn ghi cứng:

```python
"child_seat": ("rectangle", {"seat": SEATS, "occupied": ["unknown", "no"]}),
```

Nên `cabin_check` báo **4 lỗi** cho đúng bốn box vừa sửa:

```
[ERROR] S01_rgb.png: child_seat: occupied='yes' không hợp lệ (cho phép: unknown, no)
```

Hai nửa của bộ công cụ đang nói ngược nhau: file nhãn cho phép `yes`, script
kiểm thì cấm. Làm theo cái nào cũng sai một cái.

**Nhóm chọn giữ `yes` và báo lại**, thay vì sửa `cabin_schema.py` cho dữ liệu
của mình lọt qua. Sửa script để bài mình pass đúng là kiểu việc RULES đã cảnh
báo, và làm vậy thì giấu luôn chỗ lệch này khỏi coach.

Đề nghị: nếu bản JSON có `yes` là bản chính thức thì xin coach phát kèm
`cabin_schema.py` sửa tương ứng. Một dòng là xong.

### Vấn đề còn lại 2 — `infant_seat` vẫn không có chỗ ghi "có người"

`child_seat` được trả lại `yes`, nhưng `infant_seat` thì **không có trường nào**
nói ghế đang có người, ở cả ba bản schema. Nó chỉ có `seat` và `facing`.

Hệ quả cụ thể trên lô này: `S03` và `S05` đều có em bé nằm trong nôi. Sau khi gỡ
box `person` theo §3, hai cảnh đó **không còn ghi lại gì** về việc có em bé
trong xe. Nôi có em bé và nôi rỗng giờ trông y hệt nhau trong nhãn.

Với OMS thì đây đúng là trường hợp quan trọng nhất: quên trẻ trong xe. Nên
`infant_seat` có lẽ cũng cần một trường `occupied` như `child_seat`.
