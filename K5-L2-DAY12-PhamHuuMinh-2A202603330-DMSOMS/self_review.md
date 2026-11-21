# Tự đánh giá — Phạm Hữu Minh · G07

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`,
`lr_flip`, `flag`, `ok`.

Bốn lỗi dưới đây đều bắt được ở hai checkpoint và sửa ngay trong buổi. Lần kiểm
cuối `cabin_check` báo 0 lỗi, và không lỗi nào trong số này xuất hiện lại ở các
ảnh gán sau đó.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S02_rgb | `person` hàng ghế sau | `seat_flip` | Người lớn nằm bên phải ảnh nên em gõ `rear_right` theo thói quen. Camera nhìn về sau, đúng phải là `rear_left` | có |
| S01_rgb | `child_seat` bên trái ảnh | `wrong_class` | Ảnh tối, em tưởng là cái chăn phủ ghế nên định gán `object`. Phóng to mới thấy lưng ghế cao và khoá đai | có |
| D01 | `face13` | `lr_flip` | Em xét trái/phải theo trục x. Ảnh D## xoay 90° nên phải theo trục y: `left_eye_*` có y lớn hơn | có |
| D04 | `face` mắt | `guess` | Ghi `closed` trong khi tương phản thấp, thật ra không phân biệt được. Đổi về `unknown` | có |

Hai lỗi đầu nằm trong phạm vi Checkpoint A, hai lỗi sau nằm trong Checkpoint B.
Sau mỗi checkpoint em soát lại toàn bộ các ảnh đã gán trước đó cùng loại lỗi,
chứ không chỉ sửa đúng cái ảnh script chỉ ra.

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: gán `seat` theo bên trái/phải của **ảnh** thay vì theo **xe**. Xảy ra ở cả
hai cảnh đầu tiên, tức là hai lần liên tiếp trước khi em nhận ra.

Vì sao mình mắc: camera OMS nhìn ngược về phía sau, nên cái em thấy bên trái màn
hình lại là ghế bên phải xe. Em thuộc luật từ đầu buổi, nên đây không phải lỗi
không biết. Mắt đọc ảnh trước, đầu dịch sang hệ của xe sau, và lúc gán nhanh thì
bước dịch bị bỏ qua. Nó là lỗi quen tay, mà lỗi quen tay thì nhắc mình "nhớ cẩn
thận" không chữa được.

Lần sau tránh bằng cách: nói to vị trí ghế theo xe trước khi gõ, đúng như GUIDE
§4.3 bảo. Nghe ngớ ngẩn nhưng nó chèn một nhịp vào đúng khoảng giữa "nhìn thấy"
và "gõ", là chỗ em sai. Từ cảnh thứ ba trở đi em làm vậy và không lặp lại lần
nào nữa; Checkpoint B và lần kiểm cuối đều không còn `seat_flip`.

Một chỗ nữa đáng ghi, vì nó suýt làm em tin nhầm: `cabin_check` **không** bắt lỗi
`modality` ghi lệch so với tên file, và chỉ cảnh báo chứ không báo lỗi khi
`seatbelt` sai trên ảnh SVIRO. Nghĩa là "0 lỗi" chưa đủ để kết luận bài sạch.
Hai chỗ đó em soát tay trên cả 22 ảnh trước khi nộp.

## 3. Ghi chú về disagreement_log

Dòng bạn em đúng và em sai: ca tài xế cầm một vật tối áp sát tai. Em **không**
vẽ `phone`, Hậu thì vẽ. Lúc đầu em tưởng mình đúng, vì §4.7 nói không thấy hình
dạng thì đừng vẽ. Nhưng Hậu phóng to hơn và chỉ ra một góc máy nhô ra khỏi
ngón tay, tức là có thấy hình dạng thật.

Cái em học được không phải "lần sau cứ vẽ `phone`". Luật vẫn y nguyên. Vấn đề là
em đã kết luận "không thấy" trong khi thực tế là "em chưa nhìn đủ kỹ". Hai cái đó
khác nhau, và chỉ cái thứ nhất mới được phép dẫn tới `unknown`. Sau ca này em đặt
thói quen phóng to tối đa trước khi quyết định không vẽ một đối tượng.
