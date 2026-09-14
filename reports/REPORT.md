# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Nguyễn Thanh Đức<br>
**MSSV:** 2A202602279<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: "f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33"
- Bốn mã ảnh: "drive_022", "drive_033", "drive_038", "drive_008"
- Số vật thể thực tế: 42
- Mã SHA-256 của gói YOLO của bạn: "abc9d48bd824e5a26d13d2964c713f515c40b671f3c6c05878cbbe1a987ec2d5"
- Mã SHA-256 của gói CVAT gốc của bạn: "89fe6fdbd535a275bfaa4d7e301953a85a3449bb9a2640c0a918b15bb005a8dd"
- Nguồn đối chiếu: bộ tham chiếu do người hướng dẫn thực hành cấp
- Mã SHA-256 của gói đối chiếu: "c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b"
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu:

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu: Vì trong quá trình dán nhãn trước khi đối chiếu không có sự can thiệp từ bên khác

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_008.jpg — xe tải thùng đỏ ở giữa ảnh | truck (1) | Có cabin phía trước và thùng hàng lớn phía sau, thùng hàng mở/chứa hàng rõ ràng | Xe có thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng được gán truck |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau: Một xe có thể được phân lớp là car (class = 0), nhưng đồng thời có thuộc tính boundary = truncated nếu xe bị mép ảnh cắt.

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| drive_008.jpg — xe tải thùng đỏ bị gán car | lớp | Quan sát thấy xe có cabin và thùng hàng lớn phía sau, không phải ô tô con | Sửa thành truck. Quy tắc: phương tiện có thùng/ben/sàn hàng rõ ràng → truck |

- Số hộp `needs_review` trước và sau khi kiểm: Trước: 2; Sau: 0
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: Có một vật 'needs_review' ở xa nhưng dự theo các nguyên tắc vẫn có thể chắc chắn detect được

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: row: [1, 0.939523, 0.460539, 0.087578, 0.111953]
- Tên lớp và tọa độ điểm ảnh `xyxy`: lớp=1 (truck) | tâm=(0.9395, 0.4605) | kích thước=(0.0876, 0.1120) và pixel xyxy: [573.3, 258.9, 629.3, 330.6]
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học? Vì đúng định dạng YOLO chỉ có nghĩa là annotation hợp lệ về mặt cú pháp

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: "drive_022", "drive_033", "drive_038"
- Mã ảnh thẩm định: "drive_008"
- Mô tả một dự đoán trong `detect_result.jpg`: Xe bus nhìn rõ, không bị cắt bởi ảnh và dễ nhận biết
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào? Kiểm lại các trường hợp xe bị che khuất
- Minh chứng nào có thể bác bỏ nhận định của bạn? Mô hình được huấn luyện với ít epoch (8 epoches)
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế? Vì kết quả trong phép đánh giá sẽ có sai số

## 6. Đối chiếu nhãn

- Số hộp ghép được: 29
- IoU trung bình và trung vị: 0.829613; 0.87435
- Mức đồng thuận lớp: 0.655172
- Số hộp phía bạn không ghép được: 13
- Số hộp phía đối chiếu không ghép được: 21
- Một điểm khác biệt cụ thể: gói xuất CVAT gốc giữ được các thuộc tính còn gói YOLO chỉ giữ hình học và lớp
- Quy tắc hoặc hành động sửa phát sinh: Duy trì quy tắc sát mép biên thực tế
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng? Vì nó chỉ phản ánh tính nhất quán hoặc một định nghĩa dễ hiểu sai.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

Hiện chưa có
