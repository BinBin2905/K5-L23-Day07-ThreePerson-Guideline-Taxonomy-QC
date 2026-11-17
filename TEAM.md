# Thông tin nhóm

- Tên nhóm: ThreePerson
- Mã thành viên (ba chữ số):
  - Võ Nguyễn Thành Phat - 2A202603195
  - Phạm Trọng Hiện - 2A202603218
  - Lưu Nhật Nam - 2A202603243
- Người điều phối:Võ Nguyễn Thành Phát
- Người review bản cuối: Lưu Nhật Nam
- Commit chốt bài (điền ở kênh nộp sau khi commit): 9554974b0c3bd39fbae1a6c4e6725dfe599a30ae

## Kết quả tóm tắt (điền cuối bài)

- Lượt gán hoàn tất / được giao: 80

- IAA trước calibration (kèm mẫu số):
  IoU trung bình: 0.878 (tính trên 303 cặp box ghép), đạt ngưỡng yêu cầu (>= 0.85).
  Tỷ lệ đồng thuận lớp (Class agreement): 96.0%, đạt ngưỡng yêu cầu (>= 95%).
  Độ đồng thuận về thuộc tính (occluded_level: 60.7%, truncated: 92.1%, blur: 94.7%).

- Số ảnh phải calibration: 36 / 40 ảnh (chiếm 90%).

- Vòng QC và số ảnh đã duyệt / cả lô: Vòng 1 có 4 / 40 ảnh đạt chuẩn trực tiếp không nằm trong danh sách flagged (36 ảnh cần hiệu chỉnh qua calibration).

- Tỉ lệ lỗi:
  Theo ảnh: 90.0% (36 / 40 ảnh)
  Theo đối tượng: ca. 37.0% (~147 lỗi phân bổ trên tổng số khoảng 398–480 annotation đối tượng).

- **Quyết định** (Release / Hold / Rework) và lý do một dòng:
  Rework do tỷ lệ ảnh cần calibration quá cao (36/40 ảnh) và phát sinh tới 398 vấn đề về gán nhãn, thuộc tính, bounding box.
