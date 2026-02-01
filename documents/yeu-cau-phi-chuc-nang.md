# CÁC YÊU CẦU PHI CHỨC NĂNG

## YC-PCN-01: TÍNH CHÍNH XÁC

### Mô tả
Các chức năng phải thao tác theo đúng yêu cầu đặt ra. Các thông tin phải đầy đủ và đúng.

### Chi tiết
- **Dữ liệu**: Mọi dữ liệu nhập vào và xuất ra phải chính xác, đầy đủ
- **Xử lý**: Các nghiệp vụ phải được xử lý đúng logic
- **Kiểm tra**: Các thông tin sẽ được kiểm tra bởi phía người dùng
- **Chấm điểm**: Kết quả chấm điểm phải chính xác 100%
- **Thanh toán**: Số tiền thanh toán phải chính xác, không sai lệch

### Tiêu chí đánh giá
- Tỷ lệ lỗi tính toán: 0%
- Tỷ lệ lỗi dữ liệu: < 0.1%
- Thời gian kiểm tra dữ liệu: < 1 giây

---

## YC-PCN-02: DỄ SỬ DỤNG

### Mô tả
Giao diện bắt mắt, dễ sử dụng, phù hợp với đại đa số người dùng.

### Chi tiết
- **Giao diện**: 
  - Thiết kế hiện đại, trực quan
  - Màu sắc hài hòa, dễ nhìn
  - Font chữ rõ ràng, kích thước phù hợp
  - Hỗ trợ hiển thị công thức toán học chuyên nghiệp
  
- **Trải nghiệm người dùng**:
  - Điều hướng rõ ràng, dễ hiểu
  - Các chức năng đặt ở vị trí hợp lý
  - Thông báo lỗi rõ ràng, hướng dẫn cụ thể
  - Có hướng dẫn sử dụng cho người mới

- **Responsive**: 
  - Hỗ trợ desktop, tablet, mobile
  - Tự động điều chỉnh layout theo kích thước màn hình

### Tiêu chí đánh giá
- Người dùng mới có thể sử dụng được sau < 5 phút làm quen
- Tỷ lệ người dùng đánh giá giao diện "dễ sử dụng": > 85%
- Số bước thực hiện một tác vụ: tối thiểu

---

## YC-PCN-03: HIỆU SUẤT

### Mô tả
Hệ thống hoạt động với hiệu suất tốt, phản hồi của hệ thống phải nhanh.

### Chi tiết

#### Thời gian phản hồi
- Tải trang thông thường: < 2 giây
- Tìm kiếm: < 1 giây
- Load danh sách (phân trang): < 1 giây
- Chấm điểm tự động: < 3 giây
- Xử lý thanh toán: < 5 giây

#### Lượng truy cập
- **Tối đa**: 1000 request đồng thời
- **Băng thông ước tính**:
  - 25KB × 1000 = 25,000KB (~25MB)
  - Website có khoảng 10 trang HTML → 250MB băng thông

#### Tối ưu hóa
- Sử dụng caching cho dữ liệu tĩnh
- Nén dữ liệu truyền tải (gzip)
- Lazy loading cho hình ảnh
- Tối ưu database query
- CDN cho static assets

### Tiêu chí đánh giá
- Thời gian load trang trung bình: < 2s
- Thời gian đến byte đầu tiên (TTFB): < 500ms
- Xử lý được 1000 concurrent users mà không giảm hiệu suất

---

## YC-PCN-04: CÔNG NGHỆ

### Mô tả
Sử dụng công nghệ Web

### Chi tiết
- **Frontend**: HTML, CSS, JavaScript
- **Backend**: ASP.NET MVC (C#)
- **Database**: SQL Server 2014
- **Web Server**: IIS 10
- **Browser hỗ trợ**: Chrome, Firefox, Edge, Safari (2 phiên bản mới nhất)

---

## YC-PCN-05: TIN CẬY

### Mô tả
Hệ thống ít lỗi, hoạt động ổn định.

### Chi tiết
- **Tỷ lệ lỗi**: Trong 1000 giao dịch chỉ có 1-2 giao dịch lỗi (< 0.2%)
- **Uptime**: ≥ 99% (downtime < 7.2 giờ/tháng)
- **Khôi phục lỗi**: Tự động retry khi gặp lỗi tạm thời
- **Backup**: Sao lưu dữ liệu hàng ngày
- **Rollback**: Có thể khôi phục dữ liệu khi cần

### Tiêu chí đánh giá
- Tỷ lệ giao dịch thành công: ≥ 99.8%
- Thời gian ngừng hoạt động không kế hoạch: < 4 giờ/tháng
- Thời gian khôi phục khi gặp sự cố: < 1 giờ

---

## YC-PCN-06: BẢO MẬT

### Mô tả
Bảo mật thông tin khách hàng và dữ liệu hệ thống. Phân quyền rõ ràng.

### Chi tiết

#### Bảo mật dữ liệu
- **Mật khẩu**: 
  - Mã hóa bằng bcrypt hoặc PBKDF2
  - Không lưu plain text
  - Salt riêng cho mỗi mật khẩu
  
- **Dữ liệu nhạy cảm**:
  - Mã hóa thông tin thanh toán
  - Tuân thủ PCI DSS nếu xử lý thẻ
  - SSL/TLS cho mọi kết nối

#### Phân quyền
- **Người dùng**: Chỉ xem/làm đề đã mua, quản lý thông tin cá nhân
- **Giáo viên**: Tạo/sửa gói đề, đề thi, câu hỏi của mình
- **Admin**: Toàn quyền quản lý hệ thống

#### Xử lý vi phạm
- **Người dùng/Giáo viên để lộ thông tin**: 
  - Khóa tài khoản
  - Xem xét báo cáo cơ quan chức năng
  
- **Admin để lộ thông tin**:
  - Cho nghỉ việc
  - Xem xét xử lý pháp lý

#### Bảo vệ tấn công
- SQL Injection: Sử dụng parameterized query
- XSS: Sanitize input, escape output
- CSRF: Sử dụng anti-CSRF token
- Brute force: Rate limiting, CAPTCHA
- Session hijacking: Secure cookie, HTTPS only

### Tiêu chí đánh giá
- Vượt qua kiểm tra bảo mật cơ bản (OWASP Top 10)
- Không có lỗ hổng nghiêm trọng
- Audit log đầy đủ cho các thao tác nhạy cảm

---

## YC-PCN-07: RÀNG BUỘC BÊN NGOÀI

### Mô tả
Thông tin gói đề/đề thi/câu hỏi phải đầy đủ, có nguồn gốc rõ ràng, chất lượng.

### Chi tiết
- **Nội dung**: 
  - Câu hỏi phải đúng chính tả, ngữ pháp
  - Đáp án phải chính xác
  - Nội dung phù hợp với chương trình THPT
  
- **Nguồn gốc**:
  - Ghi rõ người tạo
  - Đề tham khảo phải ghi nguồn
  
- **Chất lượng**:
  - Câu hỏi không gây hiểu nhầm
  - Độ khó phù hợp với đối tượng
  - Phân bổ vùng kiến thức hợp lý

---

## YC-PCN-08: ĐẠO ĐỨC

### Mô tả
Nghiêm cấm sử dụng thông tin khách hàng để chuộc lợi.

### Chi tiết
- Không bán/chia sẻ thông tin cá nhân người dùng
- Không sử dụng thông tin thanh toán sai mục đích
- Không spam email/SMS quảng cáo
- Tuân thủ quy định bảo vệ dữ liệu cá nhân
- Minh bạch trong chính sách sử dụng dữ liệu

### Xử lý vi phạm
- Khóa tài khoản vĩnh viễn
- Xử lý kỷ luật nội bộ
- Báo cáo cơ quan chức năng nếu nghiêm trọng

---

## YC-PCN-09: KHẢ NĂNG MỞ RỘNG (SCALABILITY)

### Mô tả
Hệ thống có khả năng mở rộng khi số lượng người dùng tăng.

### Chi tiết
- Kiến trúc cho phép scale horizontal (thêm server)
- Database có khả năng sharding/replication
- Stateless design cho các service
- Load balancing khi cần thiết

---

## YC-PCN-10: BẢO TRÌ (MAINTAINABILITY)

### Mô tả
Code dễ đọc, dễ bảo trì và nâng cấp.

### Chi tiết
- Code theo chuẩn coding convention
- Comment đầy đủ cho logic phức tạp
- Tài liệu kỹ thuật đầy đủ
- Version control (Git)
- Automated testing (Unit test, Integration test)

---

## TỔNG KẾT YÊU CẦU PHI CHỨC NĂNG

| ID | Yêu cầu | Mức độ ưu tiên | Tiêu chí đánh giá |
|----|---------|----------------|-------------------|
| PCN-01 | Tính chính xác | Cao | Tỷ lệ lỗi < 0.1% |
| PCN-02 | Dễ sử dụng | Cao | Người dùng làm quen < 5 phút |
| PCN-03 | Hiệu suất | Cao | Load trang < 2s, 1000 concurrent users |
| PCN-04 | Công nghệ | Bắt buộc | ASP.NET MVC, SQL Server |
| PCN-05 | Tin cậy | Cao | Uptime ≥ 99%, lỗi < 0.2% |
| PCN-06 | Bảo mật | Rất cao | Vượt qua OWASP Top 10 |
| PCN-07 | Ràng buộc bên ngoài | Trung bình | Nội dung chất lượng, nguồn gốc rõ ràng |
| PCN-08 | Đạo đức | Bắt buộc | Không vi phạm quy định |
| PCN-09 | Khả năng mở rộng | Trung bình | Hỗ trợ scale khi cần |
| PCN-10 | Bảo trì | Trung bình | Code quality, documentation |
