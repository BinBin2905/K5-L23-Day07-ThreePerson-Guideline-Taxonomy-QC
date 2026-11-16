# Guideline gán nhãn — <phiên bản> — <03243 - Lưu Nhật Nam| Nhóm Threeperson>

> Copy file này (hoặc `guideline_v0_starter.md`) rồi viết. Một guideline đủ gồm **5 phần**. Câu nào của v0
> làm hai người đọc ra hai cách khác nhau thì phải viết lại cho một cách hiểu duy nhất.

- Phiên bản:
- Ngày: 16/11/2026
- Người viết / duyệt: Lưu Nhật Nam

## 1. Mục tiêu

Nhãn dùng để huấn luyện mô hình phát hiện **người và phương tiện** trên ảnh đường phố
(ví dụ cho camera giao thông hoặc xe tự lái).

## 2. Lớp và thuộc tính

### Lớp (mỗi vật thuộc đúng một lớp)

| Lớp | Định nghĩa |
|---|---|
| `car` | Ô tô con. |
| `truck` | Xe tải. |
| `van` | Xe thùng kín cỡ nhỏ. |
| `motorcycle` | Xe máy, mô tô. |
| `rider` | người đang điều khiển phương tiện giao thông. |
| `person` | Người. |
| `ignore_region` | Vùng không cần gán từng vật. |
| `car3` | xe 3 bánh hoặc tuktuk |
| `goods`| Hàng hóa trên xe chở |
| `cyclo` | xích lô |
| `bicycle` | xe đạp 2 bánh |
| `bus` | Xe buýt và xe khách cỡ lớn, nếu nhóm muốn mô hình nhận diện riêng |
| `replica` | Tượng, ma-nơ-canh, mô hình và đồ chơi ba chiều mô phỏng người hoặc xe |

### Thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
|---|---|---|
| `occluded_level` | `0`, `1`, `2`, `3` | Mức bị che: 0 là thấy rõ, 1 là bị che ít (hiện vật thể >= 50 %), 2 là bị che nhiều ( vật bị che từ khoảng 50% đến dưới 80%), 3 là không rõ.( vật thể mờ hoặc nhìn ko rõ, bị che > 80%) |
| `truncated` | `no`, `yes` | Vật bị cắt ở mép ảnh. |
| `can_review` | tick / không | Tick khi bạn không chắc, để người khác xem lại. |

## 3. Quy tắc vẽ box

1. Gán box cho mọi người và phương tiện trong ảnh.
2. Box ôm sát vật.
3. Chọn đúng lớp và thuộc tính.
4. Đối với label goods: một box cho cả cụm hàng.
5. Chỉ gán nhãn với những vật thể nhìn trực tiếp được.

## 4. Ca khó
| Ảnh | Tình huống | Quyết định | Lý do |
|---|---|---|---|
| img_007 | xe tự chế thêm phần thùng hàng ở sau xe | đánh nhãn truck | xe có phần thùng hàng thuộc label truck và xe đã được độ cố định thùng hàng rồi |
| img_011 | xe môt ko xác định là mô hình hay thật | đánh nhãn mô hình | ko trong phạm vi giao thông và thuộc phần trưng bày sản phẩm |
| img_020 | xe phản chiếu qua gương chiếu hậu | ko đánh nhãn | data chỉ đánh nhãn những tình huống nhìn thấy trực tiếp ko qua gián tiếp |

## 5. QC và phiên bản
Ngưỡng QC áp dụng, ai quyết ca mơ hồ, lịch sử thay đổi:

| Phiên bản | Thay đổi | Lý do (ca nào) |
|---|---|---|
| | | |
