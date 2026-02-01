# YÊU CẦU CHỨC NĂNG 17: QUẢN LÝ TÀI KHOẢN CÁ NHÂN

## Mã yêu cầu
**YC-CN-17**

## Tên yêu cầu
Quản lý tài khoản cá nhân (Quên mật khẩu, Đổi mật khẩu, Thông tin cá nhân)

## Mô tả
Cung cấp các công cụ để người dùng tự quản lý bảo mật và thông tin cá nhân của mình trên hệ thống.

## Actor (Tác nhân)
- Người dùng (Học sinh)
- Giáo viên
- Admin

## Mục tiêu
- Đảm bảo người dùng có thể khôi phục quyền truy cập khi quên mật khẩu.
- Cho phép người dùng chủ động bảo mật tài khoản.
- Cập nhật thông tin cá nhân chính xác.

## Luồng sự kiện chính

### Luồng 1: Quên mật khẩu (Sử dụng Mật khẩu phụ)
1. Người dùng chọn "Quên mật khẩu" tại trang đăng nhập.
2. Hệ thống yêu cầu nhập: Tên tài khoản và Mật khẩu phụ (đã tạo lúc đăng ký).
3. Người dùng nhập thông tin và xác nhận.
4. Hệ thống kiểm tra trong CSDL:
   - Nếu khớp: Hệ thống hiển thị form để người dùng nhập Mật khẩu mới.
   - Nếu không khớp: Hệ thống báo lỗi.
5. Người dùng nhập mật khẩu mới và xác nhận.
6. Hệ thống lưu mật khẩu mới và thông báo thành công.

### Luồng 2: Đổi mật khẩu
1. Người dùng đã đăng nhập, truy cập vào phần "Cài đặt tài khoản".
2. Chọn chức năng "Đổi mật khẩu".
3. Người dùng nhập: Mật khẩu cũ, Mật khẩu mới, Xác nhận mật khẩu mới.
4. Hệ thống kiểm tra:
   - Mật khẩu cũ có đúng không.
   - Mật khẩu mới có đủ 8 ký tự và khác mật khẩu cũ không.
   - Mật khẩu mới và xác nhận có khớp nhau không.
5. Nếu mọi thứ hợp lệ, hệ thống cập nhật mật khẩu mới và yêu cầu đăng nhập lại (tùy chọn).

### Luồng 3: Cập nhật thông tin cá nhân
1. Người dùng vào trang "Hồ sơ cá nhân".
2. Hệ thống hiển thị thông tin hiện tại: Họ tên, Ngày sinh, Email, SĐT, Avatar.
3. Người dùng chỉnh sửa các thông tin mong muốn và nhấn "Lưu".
4. Hệ thống kiểm tra tính hợp lệ của dữ liệu (định dạng Email, SĐT).
5. Hệ thống lưu thông tin và thông báo "Cập nhật thành công".

## Quy tắc nghiệp vụ
1. Mật khẩu phụ là yếu tố xác thực thứ 2 duy nhất để khôi phục mật khẩu nếu không dùng Email OTP.
2. Mật khẩu mới phải có tối thiểu 8 ký tự.
3. Tài khoản sẽ bị tạm khóa nếu yêu cầu khôi phục mật khẩu sai quá nhiều lần trong thời gian ngắn (chống brute-force).
4. Các thông tin nhạy cảm như Email/SĐT sau khi đổi có thể yêu cầu xác thực lại.

## Độ ưu tiên
**Rất cao**
