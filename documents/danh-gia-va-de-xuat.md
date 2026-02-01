# BÁO CÁO ĐÁNH GIÁ VÀ RÀ SOÁT CHỨC NĂNG HỆ THỐNG

## 1. Tổng quan đánh giá
Hệ thống hiện tại đã đáp ứng được các **nghiệp vụ cốt lõi (Core Business)** đê vận hành một website luyện thi trắc nghiệm. Tuy nhiên, để đảm bảo tính thực tế, trải nghiệm người dùng tốt và khả năng vận hành ổn định, hệ thống còn thiếu một số chức năng bổ trợ và các quy trình xử lý ngoại lệ.

## 2. Các chức năng còn thiếu (Missing Features)

### 2.1. Nhóm Quản lý tài khoản & Bảo mật (Ưu tiên Cao)
Các chức năng này có trong báo cáo gốc (Usecase) nhưng chưa được đặc tả chi tiết:
- [ ] **YC-CN-17: Quên mật khẩu & Khôi phục mật khẩu**: Sử dụng mật khẩu phụ hoặc Email/OTP.
- [ ] **YC-CN-18: Đổi mật khẩu**: Cho phép người dùng thay đổi mật khẩu định kỳ.
- [ ] **YC-CN-19: Cập nhật thông tin cá nhân**: Cập nhật Avatar, Email, SĐT.
- [ ] **YC-CN-20: Lịch sử giao dịch**: Người dùng xem lại lịch sử nạp tiền/mua gói để đối chiếu.

### 2.2. Nhóm Nghiệp vụ Giáo viên (Ưu tiên Trung bình)
Để hệ thống linh hoạt và giáo viên muốn gắn bó lâu dài:
- [ ] **YC-CN-21: Ngân hàng câu hỏi (Question Bank)**: Quản lý kho câu hỏi riêng biệt, không gắn cứng vào đề thi ngay từ đầu. Cho phép tái sử dụng câu hỏi.
- [ ] **YC-CN-22: Xử lý phản hồi**: Giao diện cho giáo viên nhận thông báo báo lỗi từ học sinh và thực hiện sửa lỗi/giải thích.

### 2.3. Nhóm Luyện thi & Kết quả (Ưu tiên Cao)
Tăng giá trị cho người học:
- [ ] **YC-CN-23: Xem lời giải chi tiết**: Hiển thị phương pháp giải cho từng câu hỏi sau khi nộp bài (quan trọng hơn cả đáp án đúng/sai).
- [ ] **YC-CN-24: Bảng xếp hạng (Leaderboard)**: Xếp hạng học sinh theo đề thi hoặc theo tuần/tháng.

### 2.4. Nhóm Admin (Ưu tiên Thấp)
- [ ] **YC-CN-25: Quản lý học sinh**: Khóa/Mở khóa tài khoản học sinh vi phạm.
- [ ] **YC-CN-26: Cấu hình hệ thống**: Quản lý banner, thông báo, tham số hệ thống.

## 3. Phân tích sự liên kết và Lỗ hổng Logic (Integration Gaps)

### 3.1. Luồng xử lý Báo lỗi & Cập nhật điểm
- **Hiện trạng**: Học sinh báo lỗi -> Admin/Giáo viên nhận tin.
- **Lỗ hổng**: Khi câu hỏi được sửa lại đáp án đúng (ví dụ từ A sang B), điểm số của những học sinh đã làm bài trước đó chưa được xử lý.
- **Đề xuất**: Thêm chức năng **"Chấm lại (Re-grade)"** cho một đề thi cụ thể khi có sự thay đổi về đáp án gốc.

### 3.2. Luồng Thanh toán & Kích hoạt
- **Hiện trạng**: Thanh toán thành công -> Kích hoạt gói.
- **Lỗ hổng**: Xử lý trường hợp "Tiền trừ nhưng gói chưa mở" (do lỗi mạng lúc callback).
- **Đề xuất**: Thêm chức năng cho Admin **"Kích hoạt thủ công"** dựa trên mã giao dịch hoặc bằng chứng thanh toán.

### 3.3. Luồng Câu hỏi & Đề thi
- **Hiện trạng**: Thêm câu hỏi trực tiếp vào đề.
- **Bất cập**: Nếu muốn tạo 2 đề thi có 50% câu hỏi giống nhau (đề ôn tập), giáo viên phải nhập liệu 2 lần.
- **Đề xuất**: Tách riêng **Ngân hàng câu hỏi**. Quy trình: Tạo câu hỏi vào ngân hàng -> Tạo đề thi -> Chọn câu hỏi từ ngân hàng đưa vào đề.

## 4. Kế hoạch cập nhật

Dựa trên đánh giá trên, đề xuất bổ sung thêm các file đặc tả yêu cầu chức năng sau:

1. `yeu-cau-chuc-nang-17-quan-ly-tai-khoan-ca-nhan.md` (Gộp Quên MK, Đổi MK, Profile)
2. `yeu-cau-chuc-nang-18-lich-su-giao-dich.md`
3. `yeu-cau-chuc-nang-19-ngan-hang-cau-hoi.md`
4. `yeu-cau-chuc-nang-20-xem-loi-giai-chi-tiet.md`

Bạn có muốn tôi tiến hành tạo các file chức năng bổ sung này không?
