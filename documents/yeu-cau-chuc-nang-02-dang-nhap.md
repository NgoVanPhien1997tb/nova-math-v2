# YÊU CẦU CHỨC NĂNG 02: ĐĂNG NHẬP

## Mã yêu cầu
**YC-CN-02**

## Tên yêu cầu
Đăng nhập vào hệ thống

## Mô tả
Cho phép người dùng (học sinh, giáo viên, admin) đăng nhập vào hệ thống để sử dụng các chức năng tương ứng với quyền của mình.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)
- Giáo viên
- Admin

## Mục tiêu
Đăng nhập thành công vào hệ thống

## Điều kiện tiên quyết
- Người dùng đã có tài khoản trong hệ thống
- Tài khoản chưa bị khóa
- Người dùng đang truy cập trang đăng nhập

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng truy cập trang đăng nhập | Hệ thống hiển thị form đăng nhập |
| 2 | Người dùng nhập:<br/>- Tên tài khoản<br/>- Mật khẩu | Hệ thống nhận thông tin |
| 3 | Người dùng nhấn nút "Đăng nhập" | Hệ thống xử lý yêu cầu |
| 4 | | Hệ thống kiểm tra tên tài khoản có tồn tại trong CSDL |
| 5 | | Hệ thống kiểm tra tài khoản có bị khóa hay không |
| 6 | | Hệ thống xác nhận tên tài khoản và mật khẩu khớp nhau |
| 7 | | Hệ thống tạo phiên đăng nhập (session) |
| 8 | | Hệ thống chuyển hướng đến trang chủ tương ứng với vai trò |

## Luồng sự kiện thay thế

### Luồng 4a: Tên tài khoản không tồn tại
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 4a.1 | Tên tài khoản không tồn tại trong CSDL | Hệ thống thông báo "Tên tài khoản không tồn tại" |
| 4a.2 | | Hệ thống trả về giao diện đăng nhập |

### Luồng 6a: Tên tài khoản và mật khẩu không khớp
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 6a.1 | Tên tài khoản và mật khẩu không khớp nhau | Hệ thống đếm số lần đăng nhập sai |
| 6a.2 | Số lần đăng nhập sai < 5 | Hệ thống thông báo "Tên tài khoản hoặc mật khẩu không đúng" |
| 6a.3 | | Hệ thống hiển thị số lần đăng nhập sai còn lại |
| 6a.4 | | Hệ thống trả về giao diện đăng nhập |

### Luồng 6b: Đăng nhập sai quá 5 lần
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 6b.1 | Người dùng đăng nhập sai quá 5 lần với cùng 1 tên tài khoản | Hệ thống khóa tài khoản có tên tài khoản vừa đăng nhập |
| 6b.2 | | Hệ thống thông báo "Tài khoản đã bị khóa do đăng nhập sai quá nhiều lần" |
| 6b.3 | | Hệ thống hướng dẫn liên hệ Admin để mở khóa |

### Luồng 5a: Tài khoản bị khóa
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 5a.1 | Tài khoản đã bị khóa trước đó | Hệ thống thông báo "Tài khoản đã bị khóa" |
| 5a.2 | | Hệ thống hướng dẫn liên hệ Admin |

## Điều kiện sau
- Người dùng đăng nhập thành công
- Phiên làm việc (session) được tạo
- Người dùng có quyền truy cập các chức năng theo vai trò

## Quy tắc nghiệp vụ
1. Tài khoản sẽ bị khóa tự động sau 5 lần đăng nhập sai liên tiếp
2. Chỉ Admin mới có quyền mở khóa tài khoản bị khóa
3. Mỗi vai trò (người dùng, giáo viên, admin) sẽ được chuyển đến trang chủ khác nhau
4. Phiên đăng nhập sẽ hết hạn sau một khoảng thời gian nhất định (cấu hình)

## Dữ liệu đầu vào
- Tên tài khoản (String)
- Mật khẩu (String)

## Dữ liệu đầu ra
- Thông báo kết quả đăng nhập
- Session/Token xác thực (nếu thành công)
- Chuyển hướng đến trang phù hợp với vai trò

## Độ ưu tiên
**Cao** - Chức năng cơ bản, bắt buộc phải có

## Ghi chú
- Cần mã hóa mật khẩu khi so sánh với CSDL
- Nên sử dụng CAPTCHA sau một số lần đăng nhập sai để tránh brute force attack
- Cần ghi log các lần đăng nhập (thành công/thất bại) để theo dõi
