# TEAM — Ngày 12

Mã bộ ảnh: **G07** · Hình thức: nhóm

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Phạm Hữu Minh | 2A202603330 | có | Đoàn Thị Ánh Hậu | 9 |
| Đoàn Thị Ánh Hậu | 2A202603282 | có | Phạm Thị Lương | 9 |
| Phạm Thị Lương | 2A202603302 | có | Phạm Hữu Minh | 7 |

Nhóm 3 người nên xoay vòng A→B→C→A theo RULES §1. Mỗi đứa tự gán đủ 22 ảnh trên
CVAT của mình, không chia ảnh cho nhau.

Ca ba đứa vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- Một cảnh SVIRO có cái túi đặt chồng lên ghế trẻ em. Minh và Hậu gán hai box
  (`child_seat` + `object`) theo §4.8, Lương gán một box `object` vì cho rằng
  cái ghế bị che gần hết. Coach chốt: hai box, và `child_seat.occupied` để
  `unknown` chứ không phải `no`, vì không nhìn rõ được bên trong.
- Một khung D## tài xế đang cầm vật tối áp sát tai. Hậu vẽ `phone`, Minh không
  vẽ. Coach chốt theo §4.7: không thấy cạnh, góc hay màn hình thì không vẽ.
  Đây là lỗi mà cả nhóm đều suýt mắc.

## Cá nhân

### Phạm Hữu Minh - 2A202603330
- Phần mình làm (ngoài 22 ảnh của mình): review chéo cho B, và gom hai ca
  hỏi coach ở trên thành ghi chú chung cho cả nhóm.
- Điều mình học được hôm nay: gọi ghế theo xe chứ không theo ảnh. Nghe thì
  hiển nhiên, nhưng gán được bốn cảnh rồi mình vẫn còn quen tay gõ theo ảnh.

### Đoàn Thị Ánh Hậu - 2A202603282
- Phần mình làm: review chéo cho C. Ghi lại phần `ir_log` cho nhóm đối chiếu.
- Điều mình học được: phân biệt `unknown` với `closed`. Trước mình hay chọn
  `closed` cho nhanh khi không nhìn rõ mắt, mà hai cái đó dạy model hai chuyện
  khác hẳn nhau.

### Phạm Thị Lương - 2A202603302
- Phần mình làm: review chéo cho A. Dựng bảng so ghế in ra dán màn hình cho nhóm.
- Điều mình học được: trên ảnh D## xoay 90°, trái/phải phải xét theo trục y.
  Lần đầu mình gán ngược hết cả hai mắt.
