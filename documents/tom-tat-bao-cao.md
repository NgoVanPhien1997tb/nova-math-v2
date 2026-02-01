# TÓM TẮT BÁO CÁO PHÂN TÍCH VÀ THIẾT KẾ PHẦN MỀM

## Giới thiệu

Hệ thống **Website luyện đề thi Toán THPT Quốc Gia** được xây dựng nhằm hỗ trợ học sinh ôn luyện cho kỳ thi THPT Quốc Gia trong bối cảnh hình thức thi đã chuyển từ tự luận sang trắc nghiệm.

## Mục đích hệ thống

Tạo môi trường thi trắc nghiệm trực tuyến giúp học sinh:
- Làm quen với mô hình thi mới
- Luyện tập nhiều dạng đề thi và câu hỏi
- Đạt kết quả cao trong kỳ thi THPT Quốc Gia

## Các đối tượng sử dụng

### 1. Người Dùng (Học sinh)
- Đăng ký, đăng nhập và quản lý hồ sơ cá nhân.
- Xem danh sách và mua các gói đề thi trực tuyến.
- Xem lịch sử giao dịch và gói đề đã sở hữu.
- Làm đề thi, xem kết quả và lời giải chi tiết (YC-CN-20).
- Phản hồi/Báo lỗi về chất lượng đề thi hoặc câu hỏi.

### 2. Giáo Viên
- Tạo và quản lý gói đề thi, đề thi.
- Quản lý Ngân hàng câu hỏi tập trung (YC-CN-19) để tái sử dụng.
- Thêm, sửa câu hỏi và cập nhật lời giải chi tiết.
- Xử lý các phản hồi báo lỗi từ học sinh.

### 3. Admin (Quản trị viên)
- Quản lý tài khoản giáo viên (tạo mới, khóa/mở khóa)
- Quản lý toàn bộ gói đề, đề thi, câu hỏi
- Xem thống kê doanh thu và số lượng gói đề đã bán
- Xem danh sách giáo viên trong hệ thống

## Các vấn đề được giải quyết

1. **Thanh toán trực tuyến**: Hệ thống tích hợp phương thức thanh toán trực tuyến thay vì thanh toán thủ công, giúp việc mua đề được thuận tiện và nhanh chóng.

2. **Cung cấp thông tin đầy đủ**: Người dùng có thể xem chi tiết thông tin về gói đề trước khi quyết định mua, giúp đưa ra quyết định chính xác.

3. **Phản hồi lỗi**: Người làm đề có thể phản hồi về lỗi trong đề thi hoặc câu hỏi đến người quản trị và người tạo đề để kịp thời xử lý.

4. **Quản lý giáo viên linh hoạt**: Hệ thống cho phép quản lý giáo viên làm việc part-time, có thể dễ dàng thêm mới hoặc khóa tài khoản khi cần thiết.

5. **Giá trị giáo dục thực tế**: Không chỉ dừng lại ở điểm số, hệ thống cung cấp lời giải chi tiết giúp học sinh hiểu kiến thức và sửa lỗi sai ngay lập tức.

6. **Tối ưu năng suất soạn đề**: Với Ngân hàng câu hỏi tập trung, giáo viên không cần nhập liệu lặp lại, dễ dàng quản lý chất lượng câu hỏi.

## Quy trình hoạt động chính

### Quy trình mua và làm đề của học sinh
1. Đăng ký/Đăng nhập vào hệ thống
2. Xem danh sách các gói đề có sẵn
3. Chọn gói đề muốn mua
4. Thanh toán trực tuyến
5. Xem danh sách các đề trong gói đã mua
6. Chọn đề và làm bài thi
7. Xem kết quả và phân tích
8. Phản hồi nếu phát hiện lỗi

### Quy trình tạo đề của giáo viên
1. Đăng nhập với tài khoản giáo viên
2. Tạo gói đề mới hoặc chọn gói đề có sẵn
3. Thêm đề thi vào gói đề
4. Thêm câu hỏi vào đề thi
5. Chỉnh sửa/cập nhật câu hỏi khi cần

### Quy trình quản lý của Admin
1. Đăng nhập với tài khoản Admin
2. Tạo và quản lý tài khoản giáo viên
3. Giám sát và quản lý gói đề/đề thi/câu hỏi
4. Xem thống kê doanh thu và số lượng đề đã bán
5. Xử lý phản hồi từ người dùng

## Cấu trúc dữ liệu chính

### Các thực thể chính
- **Người Dùng**: Họ tên, ngày sinh, tên tài khoản, mật khẩu, mật khẩu phụ
- **Giáo Viên**: Họ tên, SĐT, tên tài khoản, mật khẩu, trạng thái tài khoản
- **Admin**: Họ tên, SĐT, tên tài khoản, mật khẩu
- **Gói Đề**: Nội dung, người tạo, số lượng đề
- **Đề Thi**: Mã đề thi, gói đề, nội dung đề thi
- **Câu Hỏi**: Mã câu hỏi, nội dung, hình ảnh, 4 đáp án, đáp án đúng, vùng kiến thức

## Lợi ích của hệ thống

### Đối với học sinh
- Có môi trường ôn luyện giống với đề thi thật
- Tiết kiệm thời gian và chi phí đi lại
- Có thể luyện tập mọi lúc mọi nơi
- Nhận kết quả và phân tích ngay lập tức

### Đối với giáo viên
- Có công cụ tạo đề hiệu quả
- Quản lý kho câu hỏi dễ dàng
- Có thu nhập từ việc tạo đề chất lượng

### Đối với quản trị
- Quản lý tập trung toàn bộ hệ thống
- Theo dõi doanh thu và hiệu quả kinh doanh
- Đảm bảo chất lượng đề thi thông qua thống kê và phản hồi
