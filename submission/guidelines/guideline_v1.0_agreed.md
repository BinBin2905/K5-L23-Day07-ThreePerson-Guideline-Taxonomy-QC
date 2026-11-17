# Guideline gán nhãn — v1.0 — nhóm Threeperson

- Phiên bản: v1.0
- Ngày: 2026-11-16
- Người viết / duyệt: Nhóm Threeperson; tổng hợp từ guideline cá nhân của `2A202603195`, `2A202603218` và `2A202603243`.
- Phạm vi: bounding box trên 40 ảnh đường phố; gán trong CVAT và xuất COCO 1.0.

## 1. Mục tiêu

Nhãn dùng để huấn luyện và đánh giá mô hình phát hiện người, phương tiện và hàng hóa đang được chở trong ảnh đường phố. Mỗi box phải có lớp và ranh giới nhất quán để các thành viên gán cùng một tình huống theo cùng quy tắc. Bỏ sót vật thuộc phạm vi, gán nhầm vật mô phỏng thành vật thật hoặc vẽ box khác nhau sẽ làm giảm chất lượng huấn luyện và độ tin cậy của QC.

## 2. Lớp và thuộc tính

### Lớp

Mỗi người hoặc phương tiện thuộc đúng **một** lớp. `rider` là người đang **điều khiển** phương tiện; `person` là người thật không điều khiển phương tiện, gồm người đi bộ, đứng, ngồi và hành khách. Người và xe có box riêng nếu cả hai đủ điều kiện. Tên lớp dùng chữ thường và dấu gạch dưới để phù hợp schema taxonomy.

| Lớp             | Định nghĩa (một câu)                                                                                                                                    | Ví dụ                                                                                  | Phản ví dụ / dễ nhầm với                                                            |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `person`        | Người thật nhìn trực tiếp, hiện không điều khiển phương tiện, kể cả hành khách trên xe.                                                                 | Người đi bộ; người ngồi sau xe máy.                                                    | Người đang lái xe là `rider`; hình người in hoặc ma-nơ-canh không gán.              |
| `rider`         | Người thật đang điều khiển phương tiện xe hai bánh thuộc phạm vi taxonomy, kể cả xe hai bánh và xe ba bánh.                                             | Người lái xe máy, đạp xích lô hoặc lái ô tô nếu nhìn thấy đủ rõ.                       | Người ngồi sau hoặc đứng cạnh xe là `person`; xe được gán lớp phương tiện riêng.    |
| `car`           | Ô tô con, taxi hoặc SUV có khoang hành khách là phần chính, không có thùng hàng tách riêng.                                                             | Sedan, taxi, SUV.                                                                      | Bán tải có thùng hàng riêng là `truck`; xe khách lớn là `bus`.                      |
| `truck`         | Xe chở hàng có cabin lái tách rõ khỏi thùng hàng, sàn chở hoặc rơ-moóc; gồm xe đầu kéo và bán tải có thùng riêng.                                       | Xe chở ô tô ở `img_002.jpg`; bán tải thùng hở.                                         | Van kín liền thân là `van`; xe ba bánh chở hàng là `rickshaw`.                      |
| `van`           | Xe bốn bánh thân liền, khoang hàng hoặc khoang khách dạng hộp kín nối liền cabin, phân lớp theo kết cấu dù đang chở người hay hàng.                     | Van thùng kín cỡ nhỏ.                                                                  | Xe có thùng rời sau cabin là `truck`; xe buýt lớn là `bus`.                         |
| `bus`           | Xe buýt hoặc xe khách cỡ lớn có thân dài, nhiều hàng ghế và nhiều cửa sổ để chở hành khách.                                                             | Xe khách ở `img_009.jpg`.                                                              | Van nhỏ là `van`; xe tải thùng kín có cabin riêng là `truck`.                       |
| `motorcycle`    | Xe máy hoặc mô tô hai bánh, có hoặc không có người sử dụng.                                                                                             | Xe máy đang chạy hoặc đỗ.                                                              | Xe đạp không động cơ là `bicycle`; xe máy có thuyền bên hông là `sidecar`.          |
| `bicycle`       | Xe hai bánh vận hành chủ yếu bằng bàn đạp, có hoặc không có người sử dụng.                                                                              | Xe đạp đang được đi hoặc đang đỗ.                                                      | Xe máy là `motorcycle`; xe ba bánh đạp chở khách là `rickshaw`.                     |
| `rickshaw`      | Phương tiện ba bánh chở khách hoặc hàng, chạy bằng động cơ hay sức người, có khoang hoặc thùng trước/sau người lái; một box cho toàn xe.                | Tuk-tuk ở `img_004.jpg`, xích lô ở `img_023.jpg`, xe ba bánh chở hàng ở `img_035.jpg`. | Xe máy có thuyền gắn **bên hông** là `sidecar`; xe tải bốn bánh là `truck`.         |
| `sidecar`       | Tổ hợp xe máy và khoang chở gắn cố định ở bên hông; một box bao cả xe máy lẫn khoang.                                                                   | Xe trong `img_022.jpg`.                                                                | Tuk-tuk/xích lô có khoang trước hoặc sau người lái là `rickshaw`.                   |
| `goods`         | Hàng hóa thật đang nằm trên hoặc gắn với phương tiện được chở, gán một box cho mỗi cụm hàng có thể tách ranh giới, không tính hàng đặt bên lề đường.    | Các ô tô được xếp trên xe chở ở `img_002.jpg`; kiện hàng trên xe ba bánh.              | Xe đang tự tham gia giao thông được gán lớp xe; hàng bày bán trên vỉa hè không gán. |
| `ignore_region` | Vùng ảnh chủ ý bỏ qua việc gán từng vật khi các vật thuộc phạm vi quá nhỏ hoặc chồng lấn đến mức không thể tách ổn định; đây không phải vật huấn luyện. | Phần xa quá dày đặc của `img_021.jpg`.                                                 | Không phủ một người, xe hoặc kiện hàng lớn còn nhận dạng và tách được.              |

### Thuộc tính

Ước tính tỷ lệ **phần còn nhìn thấy** so với toàn bộ đối tượng. Đặt `occluded_level` theo vật khác che khuất, không theo độ mờ hoặc phần ra ngoài mép ảnh. Các khoảng dưới đây không chồng lấn:

| Thuộc tính       | Giá trị          | Nghĩa và ngưỡng                                                                                              |
| ---------------- | ---------------- | ------------------------------------------------------------------------------------------------------------ |
| `occluded_level` | `0`              | Nhìn thấy 100% đối tượng trong khung ảnh, không bị vật khác che.                                             |
| `occluded_level` | `1`              | Bị che nhưng còn nhìn thấy từ 50% đến dưới 100%.                                                             |
| `occluded_level` | `2`              | Còn nhìn thấy từ 20% đến dưới 50%.                                                                           |
| `truncated`      | `no` / `yes`     | `yes` khi đối tượng kéo dài ra ngoài mép ảnh; có thể đồng thời bị che bởi vật khác.                          |
| `blur`           | `false` / `true` | `true` khi ảnh của đối tượng bị nhòe do chuyển động hoặc mất nét nhưng vẫn nhận ra lớp(0 ~ dưới 20%);        |
| `can_review`     | `false` / `true` | `true` khi đã có box hợp lệ nhưng còn bất định về lớp, ranh giới hoặc thuộc tính; ghi mã ảnh, vùng và lý do. |

Nếu vật mờ hoặc bị che đến mức không nhận dạng được lớp, không tạo box tạm. Ghi vấn đề cấp ảnh để nhóm xem lại. `truncated=yes` không tự làm tăng `occluded_level`.

## 3. Quy tắc vẽ box

1. **`R-01` — Một vật, một box.** Mỗi người thật, phương tiện hoặc cụm hàng thuộc lớp được duyệt có box riêng khi đủ lớn và nhận dạng được. Người điều khiển là `rider`; người không điều khiển là `person`; phương tiện có box riêng dù đang có người sử dụng.
2. **`R-02` — Ranh giới box khi bị che.** Box bao toàn bộ phần nhìn thấy **và phần bị vật khác che có thể suy ra hợp lý** từ cấu trúc còn thấy. Không mở box tùy ý khi không đủ dấu hiệu để xác định phạm vi; đánh dấu `can_review` nếu box đã vẽ còn bất định.
3. **`R-03` — Cắt mép ảnh.** Box dừng tại mép ảnh cho phần vật kéo dài ra ngoài khung, đặt `truncated=yes`. Vật vừa bị che vừa bị cắt mép có cả `truncated=yes` và mức `occluded_level` tương ứng.
4. **`R-04` — Kích thước tối thiểu.** Không gán box cao dưới `rules.min_box_height_px = 20` px. Vật cao từ 20 px trở lên vẫn phải đủ dấu hiệu để nhận dạng lớp.
5. **`R-05` — Vật không thật và phản chiếu.** Không gán người/xe chỉ xuất hiện trong gương, ảnh in, màn hình; không gán tượng, ma-nơ-canh, mô hình hoặc đồ chơi như vật thật.
6. **`R-06` — Xe và hàng được chở.** Gán box cho phương tiện chở theo lớp xe. Xe hoặc vật được xếp, buộc, đặt trên phương tiện như hàng chở được gán `goods`, không gán thêm lớp xe cho chính hàng đó. Mỗi cụm hàng tách được có một box `goods`; box hàng không gồm thân phương tiện chở.
7. **`R-07` — Đám đông.** Gán từng người và xe còn đủ lớn, nhận dạng và tách box được. Chỉ tạo `ignore_region` cho phần quá nhỏ hoặc quá dày đặc không thể tách nhất quán; không dùng để che lỗi bỏ sót vật rõ ở tiền cảnh.
8. **`R-08` — Vật chưa rõ.** Không ép vật ngoài taxonomy vào lớp gần nhất. Với box đã vẽ nhưng còn phân vân, đặt `can_review=true` và ghi lý do; nếu chưa thể vẽ box hợp lệ, ghi vấn đề cấp ảnh. Không dùng `ignore_region` cho một vật đơn lẻ chỉ vì chưa biết lớp.
9. **`R-09` — Rìa ảnh và ảnh không có vật phù hợp.** Xe của camera hoặc vật ở mép ảnh áp dụng cùng tiêu chí nhận dạng và kích thước. Ảnh không có annotation vẫn phải xuất hiện trong danh sách `images` của COCO.
10. **`R-10` — Xe ba bánh.** `rickshaw` bao xe ba bánh có khoang/thùng trước hoặc sau người lái; `sidecar` bao cả xe máy và thuyền gắn bên hông. Không đồng thời gán `motorcycle` cho phần xe nằm trong `sidecar`.

## 4. Ca khó

| Ảnh           | Tình huống                                                             | Rule ID        | Quyết định                                                                                                                                                                        | Lý do                                                                                         |
| ------------- | ---------------------------------------------------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| `img_002.jpg` | Xe tải chở nhiều ô tô trên hai tầng.                                   | `R-01`, `R-06` | Một box `truck` cho xe chở; các ô tô được xếp trên xe là `goods`, không có box `car` riêng.                                                                                       | Xe trên sàn chở là hàng, không phải xe đang tự tham gia giao thông.                           |
| `img_007.jpg` | Xe có phần thùng hàng gắn phía sau.                                    | `R-01`         | Phân theo kết cấu nhìn thấy: thùng tách cabin là `truck`, thân hộp kín liền cabin là `van`.                                                                                       | Hàng đang chở không tự đổi lớp xe.                                                            |
| `img_009.jpg` | Xe khách thân dài, nhiều cửa kính.                                     | `R-01`         | Gán `bus`, không ép vào `van`.                                                                                                                                                    | Kết cấu chuyên chở nhiều hành khách khác van nhỏ.                                             |
| `img_019.jpg` | Hình người trong trang phục mascot, chưa rõ là người thật hay mô hình. | `R-05`, `R-08` | Chỉ gán `person` khi xác nhận không điều khiển phương tiện và là người thật; nếu đã vẽ box mà còn nghi ngờ, đặt `can_review=true`; nếu không đủ căn cứ để vẽ, ghi vấn đề cấp ảnh. | Không suy từ trang phục rằng đó chắc chắn là người thật.                                      |
| `img_020.jpg` | Xe chỉ hiện trong gương chiếu hậu.                                     | `R-05`         | Không gán box cho xe phản chiếu.                                                                                                                                                  | Chỉ gán đối tượng nhìn trực tiếp trong cảnh.                                                  |
| `img_021.jpg` | Xe và người đứng sát nhau trong đám đông.                              | `R-04`, `R-07` | Box riêng vật đủ lớn và tách được; `ignore_region` chỉ phủ phần không thể tách ổn định.                                                                                           | Không bỏ qua toàn ảnh chỉ vì có một vùng quá dày đặc.                                         |
| `img_022.jpg` | Xe máy có thuyền chở gắn bên hông.                                     | `R-10`         | Một box `sidecar` bao cả xe máy và thuyền.                                                                                                                                        | Khác `rickshaw` có khoang/thùng trước hoặc sau người lái.                                     |
| `img_024.jpg` | Xe vàng đặt trên nóc xe đen, chưa rõ là xe thật hay mô hình.           | `R-06`, `R-08` | Nếu xác định là hàng được chở, gán `goods` cho xe vàng và lớp phương tiện cho xe đen; nếu chưa xác định được, ghi vấn đề cấp ảnh hoặc `can_review=true` trên box đã vẽ hợp lệ.    | Vật được chở không đồng thời là `car`; `ignore_region` không dành cho một vật đơn lẻ chưa rõ. |
| `img_033.jpg` | Ma-nơ-canh sau kính giống người.                                       | `R-05`         | Không gán `person`; không thêm `replica` hoặc `statue`.                                                                                                                           | Là vật trưng bày, không phải người thật.                                                      |
| `img_038.jpg` | Ô tô đồ chơi trong tủ kính.                                            | `R-05`         | Không gán `car` hoặc `goods`.                                                                                                                                                     | Đồ chơi không phải xe thật đang tham gia giao thông hay hàng trên phương tiện.                |

Nếu còn ca không khớp các quy tắc trên, ghi ảnh, vùng, quyết định tạm và lý do để nhóm calibration. `can_review` chỉ đặt trên box đã tạo; vấn đề không có box được ghi cấp ảnh.

## 5. QC và phiên bản

| Phiên bản | Thay đổi                                                                                                                            | Lý do (ca nào)                                                                            |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| v0        | Bản khởi đầu có `car`, `truck`, `van`, `motorcycle`, `person` và `ignore_region`.                                                   | Thiếu lớp và quy tắc cho xe đặc biệt, hàng được chở và ca che khuất.                      |
| v1.0      | Thống nhất thêm `rider`, `bicycle`, `bus`, `rickshaw`, `sidecar`, `goods`; xác định ngưỡng thuộc tính và các quy tắc `R-01`–`R-10`. | `img_002.jpg`, `img_009.jpg`, `img_019.jpg`, `img_021.jpg`, `img_022.jpg`, `img_024.jpg`. |
