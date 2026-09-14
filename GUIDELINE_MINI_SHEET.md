# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Nguyễn Thanh Đức<br>
**MSSV:** 2A202602279<br>
**Hình thức:** Cá nhân<br>
**Mã cặp:** SOLO

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: drive_008.jpg — xe màu trắng dạng thân hộp ở khu vực giữa bên trái ảnh, phía trước/chéo dưới xe buýt màu đỏ.
- Dấu hiệu nhìn thấy: 
Phương tiện có kích thước nhỏ hơn rõ rệt so với các xe buýt trong ảnh.
Thân xe dạng hộp nhỏ và kín.
Không có đặc điểm của thân xe khách dài, nhiều cửa sổ như xe buýt.
Không thấy cấu trúc thùng/ben hàng hóa rõ ràng như xe tải.
- Quy tắc áp dụng: Xe có thân hộp nhỏ, kín, dùng chở người hoặc hàng được gán van; xe buýt phải có thân xe khách dài, nhiều cửa sổ hoặc hàng ghế.
- Quyết định: van — class 3.
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Không đoán. Đánh dấu needs_review và ghi rõ phần đặc trưng nhận dạng không đủ rõ vào nhật ký quyết định. Quy tắc của bài yêu cầu không suy đoán khi vật thể quá nhỏ/mờ để phân lớp có căn cứ.

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: drive_008.jpg — xe ô tô màu đen ở sát mép phải ảnh, phần thân xe đi ra ngoài khung hình.
- Dấu hiệu nhìn thấy: 
Phía sau cabin có sàn/thiết bị công vụ và cần/thiết bị nâng.
Kết cấu phía sau không phải thân hộp kín kiểu van.
Phương tiện có cấu hình chuyên dụng rõ ràng, không phải sedan/SUV thông thường.
- Quy tắc áp dụng: Phương tiện có thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng được gán truck. Xe van phải là thân hộp nhỏ, kín; ô tô con phải thuộc các dạng sedan, hatchback, SUV, taxi hoặc pickup dùng như xe con.
- Quyết định: truck — class 1.
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Không dựa vào kích thước xe để đoán. Nếu không thể xác định có cấu trúc hàng hóa/thiết bị công vụ, chuyển review_state thành needs_review và ghi lý do. Trong trường hợp này, thiết bị công vụ phía sau cabin nhìn thấy đủ rõ, nên quyết định truck là có căn cứ.

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: drive_008.jpg — xe ô tô màu đen ở sát mép phải ảnh, phần thân xe đi ra ngoài khung hình.
- Dấu hiệu nhìn thấy khi phóng 100%: Phần xe nằm ở mép phải khung hình.
Một phần thân xe không nằm trong ảnh.
Phần phương tiện còn nhìn thấy đủ để nhận biết đây là một ô tô con.
Đây là trường hợp bị mép ảnh cắt, không phải phần bị xe khác che.
- Giá trị `visibility`: clear — phần phương tiện nằm trong ảnh đủ rõ để nhận biết.
- Giá trị `boundary`: truncated — phương tiện bị giới hạn bởi mép ảnh.
- Trạng thái `review_state`: confident — bằng chứng phần nhìn thấy đủ để đưa ra quyết định.
- Lý do: visibility và boundary phải được đánh giá độc lập. Phương tiện có thể nhìn rõ nhưng vẫn có boundary = truncated nếu bị mép ảnh cắt. Guideline cũng quy định phương tiện chạm mép ảnh vẫn được gán khi có đủ bằng chứng phân lớp.

## 6. Xác nhận tự kiểm tra

- [x] Đã rà đủ bốn ảnh.
- [x] Đã kiểm vật thể thiếu và trùng.
- [x] Đã kiểm lớp và hình học từng hộp.
- [x] Mỗi hộp có đủ ba thuộc tính.
- [x] Đã xử lý mọi hộp `needs_review`.
- [x] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [x] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [x] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu
- [x] Số vật thể thực tế: 42 — 40–60 là mục tiêu khối lượng, không phải điểm cắt.
