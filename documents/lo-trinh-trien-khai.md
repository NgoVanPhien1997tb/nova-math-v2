# LỘ TRÌNH TRIỂN KHAI HỆ THỐNG LUYỆN THI TOÁN THPT

Tài liệu này phân loại các chức năng theo thứ tự ưu tiên và sự phụ thuộc logic để tối ưu hóa quá trình lập trình.

## GIAI ĐOẠN 1: NỀN TẢNG VÀ QUẢN TRỊ (AUTHENTICATION)
*Mục tiêu: Xây dựng bộ khung bảo mật và quản lý người dùng.*

1. **YC-CN-01**: Đăng ký tài khoản (Học sinh).
2. **YC-CN-02**: Đăng nhập (Dành cho cả 3 vai trò).
3. **YC-CN-12, 13, 14**: Admin quản lý Giáo viên (Tạo nhân sự để chuẩn bị nội dung).
4. **YC-CN-17**: Quản lý tài khoản cá nhân (Đổi mật khẩu, Profile).

---

## GIAI ĐOẠN 2: QUẢN LÝ NỘI DUNG (CONTENT FLOW)
*Mục tiêu: Xây dựng kho dữ liệu đề thi chất lượng.*

1. **YC-CN-19**: **Ngân hàng câu hỏi** (Làm kho chứa câu hỏi trước để tái sử dụng).
2. **YC-CN-09**: Tạo gói đề thi (Gói sản phẩm lớn).
3. **YC-CN-10**: Thêm/Sửa đề thi vào gói (Lấy câu hỏi từ ngân hàng ở bước 1).

---

## GIAI ĐOẠN 3: TRẢI NGHIỆM HỌC SINH (STUDENT CORE)
*Mục tiêu: Tính năng làm bài thi và trả kết quả.*

1. **YC-CN-03**: Xem danh sách gói đề (Cửa hàng trực tuyến).
2. **YC-CN-06**: Hiển thị đề thi (Giao diện thi).
3. **YC-CN-07**: Chấm điểm tự động và Lưu kết quả.
4. **YC-CN-20**: Xem lời giải chi tiết (Tính năng đi kèm sau khi có kết quả).

---

## GIAI ĐOẠN 4: THƯƠNG MẠI HÓA (PAYMENT FLOW)
*Mục tiêu: Tích hợp thanh toán trực tuyến.*

1. **YC-CN-04**: Tích hợp cổng thanh toán trực tuyến (API).
2. **YC-CN-18**: Lịch sử giao dịch (Xác thực thanh toán).
3. **YC-CN-05**: Xem danh sách gói đề đã mua (Quyền truy cập sau thanh toán).

---

## GIAI ĐOẠN 5: VẬN HÀNH VÀ BÁO CÁO (ANALYTICS & FEEDBACK)
*Mục tiêu: Hoàn thiện và theo dõi hệ thống.*

1. **YC-CN-08**: Hệ thống phản hồi/Báo lỗi đề thi (Kết nối giữa học sinh và giáo viên).
2. **YC-CN-15, 16**: Thống kê doanh thu cho Admin (Dựa trên dữ liệu GĐ 4).

---

## CÁC NHÓM CÓ THỂ LÀM SONG SONG

| Team A (Frontend/User Flow) | Team B (Backend/Admin Flow) | Team C (Payment Integration) |
| :--- | :--- | :--- |
| YC-CN-01, 02 (Auth) | YC-CN-12, 13, 14 (Admin) | (Bắt đầu sau khi hoàn thành Auth) |
| YC-CN-03 (Display) | YC-CN-19 (Question Bank) | YC-CN-04 (Payment API) |
| YC-CN-06 (Exam UI) | YC-CN-09, 10 (Exam Management) | YC-CN-18 (Transaction Log) |
| YC-CN-07 (Scoring) | YC-CN-15, 16 (Revenue Stats) | |
