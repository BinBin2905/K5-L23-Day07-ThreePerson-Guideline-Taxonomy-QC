# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 3
- Lượt gán đủ **80/80**.
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 366 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Cùng blur | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
|---|---|---|---|---|---|---|---|---|---|---|
| **TỔNG** | 40 | 366 | 0.976 | 99.5% | 93.4% | 98.6% | 99.7% | 21/21 | 1 | ĐẠT |
| 195|218 | 13 | 94 | 0.905 | 97.9% | 74.5% | 94.7% | 98.9% | 21/21 | 1 | ĐẠT |
| 195|243 | 13 | 178 | 1.000 | 100.0% | 100.0% | 100.0% | 100.0% | 0/0 | 0 | ĐẠT |
| 218|243 | 14 | 94 | 1.000 | 100.0% | 100.0% | 100.0% | 100.0% | 0/0 | 0 | ĐẠT |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 92 ({'sai_thuoc_tinh': 28, 'co_can_review': 3, 'bo_sot_thua': 36, 'iou_thap': 12, 'bat_dong_ignore': 11, 'sai_lop': 2}), trên 13 ảnh cần calibration.
