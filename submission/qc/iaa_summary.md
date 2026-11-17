# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 3
- Lượt gán đủ **80/80**.
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 303 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp      | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Cùng blur | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
| -------- | --- | ------------ | ------ | -------- | ------------------- | -------------- | --------- | --------------- | ---------------- | ------- | -------- |
| **TỔNG** | 40  | 303          | 0.878  | 96.0%    | 60.7%               | 92.1%          | 94.7%     | 90/84           | 1                | ĐẠT     |
| 195      | 218 | 13           | 94     | 0.884    | 92.6%               | 74.5%          | 94.7%     | 98.9%           | 21/21            | 1       | CHƯA ĐẠT |
| 195      | 243 | 13           | 133    | 0.883    | 97.7%               | 41.4%          | 88.7%     | 96.2%           | 28/45            | 0       | ĐẠT      |
| 218      | 243 | 14           | 76     | 0.864    | 97.4%               | 77.6%          | 94.7%     | 86.8%           | 41/18            | 0       | ĐẠT      |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 398 ({'sai_thuoc_tinh': 140, 'bo_sot_thua': 162, 'iou_thap': 43, 'co_can_review': 15, 'sai_lop': 12, 'bat_dong_ignore': 26}), trên 36 ảnh cần calibration.
