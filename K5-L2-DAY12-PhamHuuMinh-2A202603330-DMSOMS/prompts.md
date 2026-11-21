# Khai báo dùng AI — Phạm Hữu Minh

Mình là **reviewer** của lô G07, làm bộ bài mẫu cho học viên đọc, không phải nộp
bài lấy điểm. Dưới đây khai đúng những gì công cụ AI đã làm.

| # | Công cụ | Làm gì | Mình kiểm lại thế nào |
|---|---|---|---|
| 1 | Claude (Anthropic) | Đọc README, GUIDE, RULES, RUBRIC, SUBMISSION, CHECKPOINTS rồi gom thành một tờ thao tác kèm lệnh điền sẵn đường dẫn | Đối chiếu từng lệnh với CHECKPOINTS.md trước khi chạy |
| 2 | Claude | Viết thêm `extra_check.py` để bịt hai chỗ `cabin_check` bỏ sót (modality lệch tên file; `seatbelt` sai trên SVIRO chỉ là cảnh báo) | Tự dựng file export cố tình sai rồi chạy, xác nhận bắt được; chạy trên file đúng, xác nhận 0 lỗi |
| 3 | Claude | **Xem 22 khung ảnh và vẽ toàn bộ box cho cả 22 khung**: `person`, `face`, `hand`, `child_seat`, `infant_seat`, `object`, và tag `frame_meta`. Ảnh SVIRO quá tối nên nó làm bản sáng tạm để xem, không sửa file gốc | Mình mở CVAT soát lại và tự sửa: nới `face` của D04 và D05 cho hết cằm, nới mép trái `infant_seat` của S02, xoá một box `child_seat` trùng ở S07_ir |
| 4 | Claude | Soát lại theo guideline rồi sửa hai lỗi của chính nó: S02 gán nhầm `infant_seat` (đúng ra là `child_seat`, vì không có quai xách), và `light` của D01–D06 đặt `night` mà không có căn cứ (đổi về `unknown`) | Đọc lại §4.2, §4.5, §4.7 rồi đối chiếu từng chỗ |
| 5 | Claude | Soạn nháp `class_list_v0.md`, `ir_log.csv`, `self_review.md`, `TEAM.md` | Mình đọc lại, sửa chỗ không đúng ý, và tự chịu trách nhiệm về nội dung |

## Phần AI **không** làm

- **`face13` (13 điểm trên mặt): không có khung nào do AI đặt.** Mình cố ý giữ
  phần này để người đặt tay, vì đó là chỗ học được quy tắc trái/phải trên ảnh xoay.
- Không quyết thay mình ca trẻ em ngồi trong ghế trẻ em. Chỗ đó guideline không
  diễn đạt được, mình ghi thành câu hỏi leo thang để hỏi lại.

## Chỗ mình biết là còn yếu

- Mép hộp là ước lượng bằng mắt, sai số khoảng 10–15 px, chưa kéo sát mép hết.
- Trái/phải hai bàn tay ở `D05` chưa chắc.
- `infant_seat.facing` đặt `rear` theo lẽ thường của nôi xách, không phải vì nhìn
  ra hướng. Schema không có `unknown` cho trường này nên không ghi được "không chắc".

## Lưu ý về ảnh

Ảnh cabin **có** được đưa cho công cụ AI đọc, vì bộ này làm bài mẫu và mình là
reviewer. RULES §3 cấm học viên làm vậy; mình ghi rõ ở đây để ai đọc bài mẫu
cũng biết, và để học viên **không** hiểu nhầm là họ cũng được làm thế.
