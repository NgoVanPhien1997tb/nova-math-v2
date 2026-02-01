# YÊU CẦU CHỨC NĂNG 01: ĐĂNG KÝ

## Mã yêu cầu
**YC-CN-01**

## Tên yêu cầu
Đăng ký tài khoản người dùng

## Mô tả
Cho phép người dùng mới tạo tài khoản trong hệ thống để có thể sử dụng các chức năng của website luyện đề thi Toán THPT Quốc Gia.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)

## Mục tiêu
Đăng ký tài khoản thành công

## Điều kiện tiên quyết
- Người dùng chưa có tài khoản trong hệ thống
- Người dùng đang truy cập trang đăng ký

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng chọn chức năng "Đăng ký" | Hệ thống hiển thị form đăng ký |
| 2 | Người dùng nhập các thông tin:<br/>- Họ tên<br/>- Ngày tháng năm sinh<br/>- Tên tài khoản<br/>- Mật khẩu<br/>- Xác nhận mật khẩu<br/>- Mật khẩu phụ | Hệ thống nhận thông tin |
| 3 | Người dùng xác nhận đăng ký | Hệ thống kiểm tra tính hợp lệ của thông tin |
| 4 | | Hệ thống kiểm tra tên tài khoản có trùng trong CSDL hay không |
| 5 | | Hệ thống kiểm tra mật khẩu có đủ số ký tự (tối thiểu 8 ký tự) |
| 6 | | Hệ thống kiểm tra mật khẩu và xác nhận mật khẩu có khớp nhau |
| 7 | | Hệ thống lưu thông tin tài khoản vào CSDL |
| 8 | | Hệ thống thông báo đăng ký thành công |
| 9 | | Hệ thống chuyển về trang đăng nhập |

## Luồng sự kiện thay thế

### Luồng 4a: Tên tài khoản đã tồn tại
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 4a.1 | Tên tài khoản bị trùng với tên tài khoản trong CSDL | Hệ thống thông báo "Tên tài khoản đã tồn tại" |
| 4a.2 | | Hệ thống trả về form đăng ký cho người dùng nhập lại |

### Luồng 5a: Mật khẩu chưa đủ số ký tự
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 5a.1 | Mật khẩu nhập chưa đủ 8 ký tự | Hệ thống thông báo "Mật khẩu phải có ít nhất 8 ký tự" |
| 5a.2 | | Hệ thống trả về form đăng ký cho người dùng nhập lại |

### Luồng 6a: Mật khẩu và xác nhận mật khẩu không khớp
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 6a.1 | Chuỗi ký tự ở textbox "Mật khẩu" không trùng với chuỗi ký tự trong textbox "Xác nhận mật khẩu" | Hệ thống thông báo "Mật khẩu xác nhận không khớp" |
| 6a.2 | | Hệ thống trả về form đăng ký cho người dùng nhập lại |

## Điều kiện sau
- Tài khoản mới được tạo và lưu trong CSDL
- Người dùng có thể đăng nhập bằng tài khoản vừa tạo

## Quy tắc nghiệp vụ
1. Tên tài khoản phải là duy nhất trong hệ thống
2. Mật khẩu phải có ít nhất 8 ký tự
3. Mật khẩu và xác nhận mật khẩu phải giống nhau
4. Tất cả các trường thông tin đều bắt buộc nhập
5. Mật khẩu phụ được sử dụng để khôi phục mật khẩu khi quên

## Dữ liệu đầu vào
- Họ tên người dùng (String)
- Ngày tháng năm sinh (Date)
- Tên tài khoản (String, duy nhất)
- Mật khẩu (String, tối thiểu 8 ký tự)
- Xác nhận mật khẩu (String)
- Mật khẩu phụ (String)

## Dữ liệu đầu ra
- Thông báo kết quả đăng ký (thành công/thất bại)
- Tài khoản mới được tạo trong CSDL (nếu thành công)

## Độ ưu tiên
**Cao** - Chức năng cơ bản, bắt buộc phải có

## Ghi chú
- Mật khẩu phụ rất quan trọng để khôi phục tài khoản khi quên mật khẩu
- Cần mã hóa mật khẩu trước khi lưu vào CSDL (yêu cầu phi chức năng về bảo mật)
