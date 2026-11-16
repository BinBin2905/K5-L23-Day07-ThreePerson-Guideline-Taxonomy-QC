# Guideline gán nhãn cá nhân — bản nháp Pha 1 — 2A202603218 - Phạm Trọng Hiện

- Phiên bản: P1-draft
- Ngày: 2026-11-16
- Người viết và xác nhận quyết định trên ảnh: Phạm Trọng Hiện
- Loại nhãn: bounding box trên ảnh đường phố; công cụ CVAT, xuất COCO 1.0

> **Cần hoàn thiện trước khi nộp Pha 1:** Tự xem đủ 40 ảnh, đối chiếu từng quy tắc dưới đây, sửa những quyết định chưa đúng với quan sát của mình và điền bảng ca khó bằng mã ảnh thật. Bản nháp này chưa ghi nhận bất kỳ kết luận nào về một ảnh cụ thể.

## 1. Mục tiêu

Tạo nhãn để huấn luyện và đánh giá mô hình phát hiện người và phương tiện trên ảnh đường phố. Mỗi box biểu diễn một đối tượng thật, nhìn thấy và thuộc một lớp đã định nghĩa. Nhãn phải nhất quán giữa người gán: cùng một tình huống cần dẫn tới cùng lớp, cùng ranh giới box và cùng giá trị thuộc tính.

Ưu tiên tránh hai lỗi ảnh hưởng trực tiếp tới mô hình: bỏ sót đối tượng thuộc phạm vi và gán nhầm vật không thật thành người hoặc phương tiện. Những trường hợp không xác định được bằng mắt sẽ được ghi để nhóm chốt ở Pha 2, thay vì tự tạo một lớp mới trong file nhãn.

## 2. Lớp và thuộc tính

### 2.1. Lớp

| Lớp             | Định nghĩa đề xuất                                                                         | Ví dụ                                                       | Không tính là lớp này / dễ nhầm                                                |
| --------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------- | ------------------------------------------------------------------------------ |
| `car`           | Ô tô chở người có khoang hành khách là phần chính, gồm xe con và SUV.                      | Sedan, SUV.                                                 | Xe có khoang hàng riêng rõ rệt (`truck`); xe thùng kín chở hàng (`van`).       |
| `truck`         | Xe tải có phần chở hàng riêng hoặc kết cấu chuyên để vận chuyển hàng.                      | Xe tải thùng, xe tải có thùng hàng phía sau cabin.          | SUV hoặc xe gia đình chỉ chở đồ trên nóc (`car`); xe thùng kín cỡ nhỏ (`van`). |
| `van`           | Xe bốn bánh có thân dạng hộp, khoang kín kéo dài và thường dùng chở hàng hoặc nhiều người. | Xe van thùng kín.                                           | Xe tải có thùng hàng tách khỏi cabin (`truck`); SUV (`car`).                   |
| `motorcycle`    | Xe cơ giới hai bánh thuộc nhóm xe máy hoặc mô tô.                                          | Xe máy, mô tô.                                              | Xe đạp; xe đồ chơi; hình xe trên biển quảng cáo.                               |
| `person`        | Người thật hiện diện trong cảnh, kể cả người đi bộ hoặc ngồi trên phương tiện.             | Người đi bộ, người lái/ngồi trên xe.                        | Tượng, ma-nơ-canh, hình người in trên poster hoặc màn hình.                    |
| `ignore_region` | Vùng ảnh được chủ ý loại khỏi yêu cầu gán từng vật; đây không phải đối tượng huấn luyện.   | Cụm vật quá nhỏ hoặc dày đặc mà không thể tách box ổn định. | Không dùng để che một vật lớn, nhận diện được nhưng khó gán lớp.               |

**Vật ngoài taxonomy:** không ép vào lớp gần giống. Nếu đó là một loại đối tượng quan trọng và lặp lại trong bộ ảnh, ghi câu hỏi trong mục 4 để cả nhóm quyết định có bổ sung lớp ở v1.0 hay không. Trong giai đoạn độc lập, không sửa `taxonomy.json`.

**Ranh giới cần nhóm xác nhận:** xe ba bánh, xe bán tải, xe buýt, xe đạp và các phương tiện không điển hình chưa có lớp riêng trong v0. Cần ghi mã ảnh và quyết định ban đầu sau khi tự xem ảnh; không suy từ tên gọi chung.

### 2.2. Thuộc tính

| Thuộc tính       | Giá trị          | Quy tắc đề xuất                                                                                                                     |
| ---------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `occluded_level` | `0`              | Không có phần nào của đối tượng bị vật khác che.                                                                                    |
| `occluded_level` | `1`              | Bị che trên 0% đến tối đa 25% diện tích ước tính của đối tượng.                                                                     |
| `occluded_level` | `2`              | Bị che trên 25% đến tối đa 60%; vẫn nhận ra loại đối tượng.                                                                         |
| `occluded_level` | `3`              | Bị che trên 60%; chỉ gán khi phần còn thấy vẫn đủ căn cứ nhận diện lớp, nếu không thì đánh dấu để nhóm xem xét.                     |
| `truncated`      | `yes` / `no`     | `yes` khi đối tượng kéo dài ra ngoài mép ảnh; `no` trong các trường hợp còn lại. Bị vật khác che ở giữa ảnh không phải `truncated`. |
| `can_review`     | `true` / `false` | `true` khi còn bất định về lớp, phạm vi box hoặc thuộc tính và cần đưa ra họp calibration; ghi mã ảnh cùng lý do trong ghi chú.     |

## 3. Quy tắc vẽ box

1. **Một vật thật, một box.** Người ngồi trên xe và chiếc xe là hai đối tượng riêng khi cả hai đủ nhìn thấy để nhận diện. Không gộp người với phương tiện vào một box.
2. **Box ôm sát phần nhìn thấy.** Đặt bốn cạnh ở cực trị của phần đối tượng thực sự nhìn thấy; không vẽ thêm phần chỉ suy đoán nằm sau vật che. Nếu vật bị cắt mép, box dừng tại mép ảnh và `truncated=yes`.
3. **Che khuất khác cắt mép.** Vật bị cột, xe khác hoặc người khác che dùng `occluded_level`; vật kéo dài ra ngoài khung ảnh dùng `truncated`. Một vật có thể đồng thời có cả hai đặc điểm.
4. **Vật không thật.** Không gán box đối tượng cho hình in, phản chiếu, màn hình, tượng, ma-nơ-canh hoặc đồ chơi. Nếu một vật trông quá giống vật thật và không thể phân biệt chắc chắn, ghi `can_review` cho box tạm và nêu lý do để nhóm chốt.
5. **Kích thước tối thiểu.** Theo `rules.min_box_height_px`, vật cao dưới 20 px không gán box riêng. Chỉ tạo `ignore_region` khi có một vùng chứa nhiều vật nhỏ/dày đặc cần loại khỏi kiểm lỗi; không vẽ một vùng ignore chỉ để che lỗi bỏ sót.
6. **Vật bị che nhiều hoặc mờ.** Chỉ gán khi đủ dấu hiệu thị giác để nhận ra lớp. Không suy từ bối cảnh rằng chắc chắn có người hay xe phía sau vật che.
7. **Xe chở xe hoặc chở người.** Gán riêng phương tiện chính và từng đối tượng được chở nếu mỗi đối tượng thật, thuộc taxonomy, đủ lớn và có thể tách ranh giới. Phần hàng hóa không thuộc taxonomy thì không ép vào `truck` hoặc `car`.
8. **Vật ở rìa ảnh và xe của camera.** Áp dụng cùng quy tắc nhận diện và kích thước; trường hợp chỉ thấy một phần rất nhỏ, không xác định được đó là vật gì thì ghi câu hỏi cho nhóm. Không tự giả định xe của camera luôn được gán hoặc luôn bỏ qua.
9. **Không có đối tượng phù hợp.** Để ảnh không có annotation; ảnh vẫn phải xuất hiện trong danh sách `images` của COCO.

## 4. Ca khó do cá nhân tự quan sát

Điền bảng này **sau khi tự xem 40 ảnh**. Mỗi dòng phải có một ảnh thật, một quyết định ban đầu cụ thể và lý do quan sát được. Nếu chưa đủ căn cứ, ghi “chưa chốt, cần họp”; không tạo dữ liệu hoặc quyết định giả.

| Ảnh         | Tình huống quan sát được            | V0 chưa trả lời điều gì? | Quyết định ban đầu của tôi                  | Lý do / điểm cần họp |
| ----------- | ----------------------------------- | ------------------------ | ------------------------------------------- | -------------------- |
| img_003.jpg | Có người dắt xe đạp(không ngồi lái) | chưa có class `xe đạp`   | Thêm class xe đạp và mô tả định nghĩa class |                      |

Các nhóm ca cần chủ động tìm: phương tiện ngoài 5 lớp, xe chở vật/xe khác, người trên xe, vật không thật, vật bị che/cắt mép, vật nhỏ hoặc quá dày đặc.

## 5. QC và lịch sử phiên bản

- Trước khi nộp nhãn cá nhân ở Pha 3, chạy `python scripts/validate_labels.py --all` để kiểm cấu trúc COCO, tên lớp, thuộc tính, box trong ảnh và đủ ảnh được giao. Script không xác nhận được box ôm sát vật thật hay phát hiện vật mà mọi người cùng bỏ sót; cần kiểm bằng mắt.
- Mọi ca `can_review=true` phải có mã ảnh và lý do để đưa vào calibration. Quy tắc mới sau họp không sửa ngược bản guideline cá nhân này; ghi ở guideline thống nhất v1.0 hoặc bản calibrated v1.1.
- Không thay đổi các ngưỡng trong `taxonomy.json.rules` để làm số đo QC đạt yêu cầu.

| Phiên bản | Thay đổi                                                                            | Lý do                                      |
| --------- | ----------------------------------------------------------------------------------- | ------------------------------------------ |
| v0        | Bản khởi đầu của đề bài.                                                            | Chưa đủ quy tắc cho các ca khó.            |
| P1-draft  | Đề xuất định nghĩa lớp, ngưỡng thuộc tính và quy tắc box; chờ đối chiếu với 40 ảnh. | Chuẩn bị bản quyết định cá nhân cho Pha 1. |
