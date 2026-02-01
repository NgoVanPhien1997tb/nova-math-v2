# DANH SÁCH TÀI LIỆU PHÂN TÍCH HỆ THỐNG LUYỆN ĐỀ THI TOÁN THPT QUỐC GIA

## Tổng quan

Tài liệu này tổng hợp kết quả phân tích báo cáo Phân tích và Thiết kế Phần mềm cho hệ thống **Website luyện đề thi Toán THPT Quốc Gia**.

---

## 1. Tóm tắt Báo cáo

📄 **File**: [tom-tat-bao-cao.md](./tom-tat-bao-cao.md)

Tóm tắt tổng quan về nghiệp vụ của hệ thống, bao gồm:
- Giới thiệu và mục đích hệ thống
- Các đối tượng sử dụng (Học sinh, Giáo viên, Admin)
- Các vấn đề được giải quyết
- Quy trình hoạt động chính
- Cấu trúc dữ liệu
- Lợi ích của hệ thống

---

## 2. Yêu cầu Chức năng

### 2.1. Dành cho Người dùng (Học sinh)

#### YC-CN-01: Đăng ký
📄 **File**: [yeu-cau-chuc-nang-01-dang-ky.md](./yeu-cau-chuc-nang-01-dang-ky.md)

Cho phép người dùng mới tạo tài khoản trong hệ thống.

**Các trường dữ liệu**:
- Họ tên
- Ngày sinh
- Tên tài khoản (duy nhất)
- Mật khẩu (≥ 8 ký tự)
- Xác nhận mật khẩu
- Mật khẩu phụ (để khôi phục)

---

#### YC-CN-02: Đăng nhập
📄 **File**: [yeu-cau-chuc-nang-02-dang-nhap.md](./yeu-cau-chuc-nang-02-dang-nhap.md)

Xác thực người dùng để truy cập hệ thống.

**Quy tắc**:
- Khóa tài khoản sau 5 lần nhập sai
- Phân quyền theo vai trò

---

#### YC-CN-03: Xem danh sách gói đề
📄 **File**: [yeu-cau-chuc-nang-03-xem-danh-sach-goi-de.md](./yeu-cau-chuc-nang-03-xem-danh-sach-goi-de.md)

Xem tất cả gói đề có trong hệ thống.

**Thông tin hiển thị**:
- Tên gói đề
- Mô tả
- Số lượng đề
- Giá
- Người tạo
- Hình ảnh

---

#### YC-CN-04: Thanh toán trực tuyến
📄 **File**: [yeu-cau-chuc-nang-04-thanh-toan-truc-tuyen.md](./yeu-cau-chuc-nang-04-thanh-toan-truc-tuyen.md)

Thanh toán qua API ngân hàng để mua gói đề.

**Phương thức hỗ trợ**:
- Thẻ ATM
- Thẻ tín dụng/ghi nợ
- Ví điện tử
- QR Code

---

#### YC-CN-05: Xem gói đề đã mua
📄 **File**: [yeu-cau-chuc-nang-05-xem-goi-de-da-mua.md](./yeu-cau-chuc-nang-05-xem-goi-de-da-mua.md)

Xem danh sách gói đề đã mua và các đề thi trong gói.

**Thông tin**:
- Gói đề đã mua
- Danh sách đề thi trong gói
- Trạng thái làm bài (chưa làm/đang làm/đã làm)
- Điểm số

---

#### YC-CN-06: Hiển thị đề thi
📄 **File**: [yeu-cau-chuc-nang-06-hien-thi-de-thi.md](./yeu-cau-chuc-nang-06-hien-thi-de-thi.md)

Hiển thị nội dung đề thi khi làm bài.

**Tính năng**:
- Hiển thị câu hỏi, đáp án
- Bộ đếm thời gian
- Hỗ trợ công thức toán học
- Lưu trạng thái tự động

---

#### YC-CN-07: Lưu và chấm đề thi
📄 **File**: [yeu-cau-chuc-nang-07-luu-va-cham-de-thi.md](./yeu-cau-chuc-nang-07-luu-va-cham-de-thi.md)

Lưu kết quả và tự động chấm điểm.

**Công thức**: Điểm = (Số câu đúng / Tổng số câu) × 10

**Thông tin lưu**:
- Điểm số
- Số câu đúng/sai
- Thời gian làm bài
- Chi tiết từng câu

---

#### YC-CN-08: Phản hồi về đề thi
📄 **File**: [yeu-cau-chuc-nang-08-phan-hoi.md](./yeu-cau-chuc-nang-08-phan-hoi.md)

Gửi phản hồi, báo lỗi về đề thi/câu hỏi.

**Loại phản hồi**:
- Lỗi câu hỏi
- Lỗi đáp án
- Lỗi hệ thống
- Góp ý
- Khác

---

#### YC-CN-17: Quản lý tài khoản cá nhân
📄 **File**: [yeu-cau-chuc-nang-17-quan-ly-tai-khoan-ca-nhan.md](./yeu-cau-chuc-nang-17-quan-ly-tai-khoan-ca-nhan.md)

Quản lý thông tin hồ sơ, đổi mật khẩu và khôi phục tài khoản qua mật khẩu phụ.

---

#### YC-CN-18: Lịch sử giao dịch
📄 **File**: [yeu-cau-chuc-nang-18-lich-su-giao-dich.md](./yeu-cau-chuc-nang-18-lich-su-giao-dich.md)

Xem lại lịch sử mua gói đề và hỗ trợ Admin tra soát thanh toán khi có lỗi.

---

#### YC-CN-19: Ngân hàng câu hỏi
📄 **File**: [yeu-cau-chuc-nang-19-ngan-hang-cau-hoi.md](./yeu-cau-chuc-nang-19-ngan-hang-cau-hoi.md)

Quản lý kho câu hỏi tập trung, cho phép phân loại theo vùng kiến thức và tái sử dụng.

---

#### YC-CN-20: Xem lời giải chi tiết
📄 **File**: [yeu-cau-chuc-nang-20-xem-loi-giai-chi-tiet.md](./yeu-cau-chuc-nang-20-xem-loi-giai-chi-tiet.md)

Cung cấp hướng dẫn giải chi tiết cho từng câu hỏi sau khi hoàn thành bài thi.

---

### 2.2. Dành cho Giáo viên và Admin

📄 **File**: [yeu-cau-chuc-nang-giao-vien-admin.md](./yeu-cau-chuc-nang-giao-vien-admin.md)

Tổng hợp các yêu cầu:

#### YC-CN-09: Tạo gói đề mới
Tạo gói đề với thông tin cơ bản.

#### YC-CN-10: Thêm đề thi vào gói đề
Thêm đề thi mới vào gói đã tạo.

#### YC-CN-11: Sửa đề thi trong gói đề
Chỉnh sửa thông tin đề thi.

#### YC-CN-12: Thêm tài khoản giáo viên (Admin)
Admin tạo tài khoản cho giáo viên mới.

#### YC-CN-13: Khóa tài khoản giáo viên (Admin)
Thay đổi trạng thái tài khoản giáo viên.

#### YC-CN-14: Xem danh sách giáo viên (Admin)
Xem và quản lý tất cả giáo viên.

#### YC-CN-15: Xem tổng doanh thu (Admin)
Thống kê tổng doanh thu hệ thống.

#### YC-CN-16: Thống kê chi tiết từng gói đề (Admin)
Xem doanh thu và số lượng bán của từng gói đề.

**Chức năng bổ sung**:
- Thêm/sửa câu hỏi
- Xem chi tiết câu hỏi/đề thi/gói đề

---

## 3. Yêu cầu Phi chức năng

📄 **File**: [yeu-cau-phi-chuc-nang.md](./yeu-cau-phi-chuc-nang.md)

### YC-PCN-01: Tính chính xác
- Tỷ lệ lỗi: < 0.1%
- Chấm điểm chính xác 100%

### YC-PCN-02: Dễ sử dụng
- Giao diện trực quan, hiện đại
- Responsive (desktop, tablet, mobile)
- Thời gian làm quen: < 5 phút

### YC-PCN-03: Hiệu suất
- Thời gian load trang: < 2 giây
- Hỗ trợ: 1000 concurrent users
- Băng thông: ~250MB

### YC-PCN-04: Công nghệ
- Frontend: HTML, CSS, JavaScript
- Backend: ASP.NET MVC (C#)
- Database: SQL Server 2014
- Web Server: IIS 10

### YC-PCN-05: Tin cậy
- Uptime: ≥ 99%
- Tỷ lệ lỗi giao dịch: < 0.2%
- Backup hàng ngày

### YC-PCN-06: Bảo mật
- Mã hóa mật khẩu (bcrypt/PBKDF2)
- SSL/TLS cho mọi kết nối
- Phân quyền rõ ràng
- Bảo vệ OWASP Top 10

### YC-PCN-07: Ràng buộc bên ngoài
- Nội dung chất lượng, có nguồn gốc rõ ràng
- Tuân thủ chương trình THPT

### YC-PCN-08: Đạo đức
- Không sử dụng thông tin cá nhân sai mục đích
- Tuân thủ quy định bảo vệ dữ liệu

### YC-PCN-09: Khả năng mở rộng
- Hỗ trợ scale horizontal
- Database replication/sharding

### YC-PCN-10: Bảo trì
- Code theo chuẩn
- Tài liệu đầy đủ
- Automated testing

---

## Cấu trúc File

```
documents/
├── INDEX.md                                    # File index tổng hợp (Hiện tại)
├── tom-tat-bao-cao.md                          # Tóm tắt nghiệp vụ
├── lo-trinh-trien-khai.md                      # Lộ trình phát triển dự án
├── danh-gia-va-de-xuat.md                      # Phân tích lỗ hổng & cải tiến
├── yeu-cau-chuc-nang-01-dang-ky.md            # YC-CN-01
├── yeu-cau-chuc-nang-02-dang-nhap.md          # YC-CN-02
├── yeu-cau-chuc-nang-03-xem-danh-sach-goi-de.md # YC-CN-03
├── yeu-cau-chuc-nang-04-thanh-toan-truc-tuyen.md # YC-CN-04
├── yeu-cau-chuc-nang-05-xem-goi-de-da-mua.md  # YC-CN-05
├── yeu-cau-chuc-nang-06-hien-thi-de-thi.md    # YC-CN-06
├── yeu-cau-chuc-nang-07-luu-va-cham-de-thi.md # YC-CN-07
├── yeu-cau-chuc-nang-08-phan-hoi.md           # YC-CN-08
├── yeu-cau-chuc-nang-17-quan-ly-tai-khoan-ca-nhan.md # YC-CN-17
├── yeu-cau-chuc-nang-18-lich-su-giao-dich.md  # YC-CN-18
├── yeu-cau-chuc-nang-19-ngan-hang-cau-hoi.md  # YC-CN-19
├── yeu-cau-chuc-nang-20-xem-loi-giai-chi-tiet.md # YC-CN-20
├── yeu-cau-chuc-nang-giao-vien-admin.md       # YC-CN-09 đến 16
├── yeu-cau-phi-chuc-nang.md                   # Tất cả YC-PCN
└── INDEX.md                                    # File này
```

---

## Thống kê

- **Tổng số yêu cầu chức năng**: 20
  - Dành cho Người dùng/Học sinh: 12
  - Dành cho Giáo viên: 7
  - Dành cho Admin: 10
  - (Một số yêu cầu dùng chung cho nhiều vai trò)

- **Tổng số yêu cầu phi chức năng**: 10

- **Tổng số file tài liệu**: 15 file

---

## Ghi chú

### Mức độ ưu tiên các yêu cầu

#### Ưu tiên cao (Must have)
- Đăng ký, Đăng nhập
- Xem danh sách gói đề
- Thanh toán trực tuyến
- Hiển thị đề thi
- Lưu và chấm đề thi

#### Ưu tiên trung bình (Should have)
- Phản hồi
- Quản lý giáo viên
- Thống kê doanh thu

#### Ưu tiên thấp (Nice to have)
- Các tính năng nâng cao
- Báo cáo chi tiết

---

## Tác giả

Tài liệu được phân tích và tổng hợp từ báo cáo gốc: **BÁO-CÁO-MÔN-PHÂN-TÍCH-VÀ-THIẾT-KẾ-PHẦN-MỀM.docx**

Ngày tạo: 31/01/2026
