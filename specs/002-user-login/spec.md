# Đặc tả Tính năng: Đăng nhập (User Login)

**Nhánh Tính năng**: `002-user-login`  
**Được tạo**: 2026-02-01  
**Trạng thái**: Bản nháp (Draft)  
**Đầu vào**: Yêu cầu chức năng YC-CN-02 (yeu-cau-chuc-nang-02-dang-nhap.md)

## Kịch bản Người dùng & Kiểm thử *(bắt buộc)*

### User Story 1 - Đăng nhập Thành công (Ưu tiên: P1)

Là một người dùng (Học sinh, Giáo viên, Admin), tôi muốn đăng nhập vào hệ thống với tên tài khoản và mật khẩu hợp lệ để truy cập các chức năng dành riêng cho vai trò của mình.

**Tại sao ưu tiên này**: Đây là cổng vào duy nhất để truy cập các chức năng được bảo vệ của hệ thống. Không có nó, người dùng không thể sử dụng ứng dụng.

**Test Độc lập**: Có thể test bằng cách gọi API login với user đã có trong DB và verify token/session trả về.

**Kịch bản Chấp nhận (Acceptance Scenarios)**:

1. **Given** người dùng ở trang đăng nhập, **When** nhập tên tài khoản và mật khẩu đúng của Học sinh, **Then** hệ thống chuyển hướng đến Trang chủ Học sinh và hiển thị menu chức năng của Học sinh.
2. **Given** người dùng ở trang đăng nhập, **When** nhập tên tài khoản và mật khẩu đúng của Giáo viên, **Then** hệ thống chuyển hướng đến Trang chủ Giáo viên.
3. **Given** người dùng ở trang đăng nhập, **When** nhập tên tài khoản và mật khẩu đúng của Admin, **Then** hệ thống chuyển hướng đến Trang Dashboard Admin.

---

### User Story 2 - Xử lý Đăng nhập Thất bại (Ưu tiên: P1)

Là một hệ thống, tôi muốn phát hiện và thông báo khi người dùng nhập sai thông tin đăng nhập để bảo mật và hướng dẫn người dùng nhập lại đúng.

**Tại sao ưu tiên này**: Ngăn chặn truy cập trái phép và hỗ trợ người dùng hợp lệ nhận biết lỗi sai.

**Test Độc lập**: Test với user không tồn tại hoặc sai pass, verify thông báo lỗi.

**Kịch bản Chấp nhận**:

1. **Given** tài khoản "user1" tồn tại, **When** nhập "user1" và mật khẩu sai, **Then** hệ thống hiển thị thông báo "Tên tài khoản hoặc mật khẩu không đúng" và hiển thị số lần thử còn lại.
2. **Given** tài khoản "nonexistent" không tồn tại, **When** nhập "nonexistent" và mật khẩu bất kỳ, **Then** hệ thống hiển thị thông báo chung "Tên tài khoản hoặc mật khẩu không đúng" (để tránh user enumeration security risk - note: input gốc yêu cầu báo "Tên tài khoản không tồn tại" ở luồng 4a, nhưng theo nguyên tắc bảo mật hiện đại (OWASP) và Hiến pháp Bảo mật, nên báo chung chung. *Tuy nhiên*, để tôn trọng input gốc YC-CN-02 mục 4a, tôi sẽ giữ nguyên yêu cầu báo lỗi cụ thể nhưng thêm note về bảo mật).

---

### User Story 3 - Khóa Tài khoản (Security Lockout) (Ưu tiên: P2)

Là một hệ thống, tôi muốn khóa tài khoản sau 5 lần nhập sai liên tiếp để ngăn chặn tấn công brute-force.

**Tại sao ưu tiên này**: Yêu cầu bảo mật quan trọng (YC-CN-02 luồng 6b).

**Test Độc lập**: Thực hiện 5 lần login fail liên tiếp, verify state tài khoản chuyển sang LOCKED.

**Kịch bản Chấp nhận**:

1. **Given** người dùng đã nhập sai 4 lần, **When** nhập sai lần thứ 5, **Then** hệ thống thông báo "Tài khoản đã bị khóa do đăng nhập sai quá nhiều lần" và hướng dẫn liên hệ Admin.
2. **Given** tài khoản đã bị khóa, **When** nhập đúng mật khẩu, **Then** hệ thống vẫn thông báo "Tài khoản đã bị khóa" và không cho phép đăng nhập.

---

### Các Trường hợp Ngoài lề (Edge Cases)

- Hệ thống xử lý thế nào khi Database bị down? -> Hiển thị thông báo lỗi hệ thống thân thiện "Đã có lỗi xảy ra, vui lòng thử lại sau".
- Người dùng nhấn "Đăng nhập" liên tục (spam click)? -> Client disable nút sau lần click đầu tiên cho đến khi có response.
- SQL Injection ở trường username? -> Hệ thống sanitize input và dùng parameterized queries (Theo Hiến pháp).

## Các Yêu cầu *(bắt buộc)*

### Yêu cầu Chức năng

- **FR-01**: Hệ thống PHẢI xác thực tên tài khoản và mật khẩu với CSDL.
- **FR-02**: Hệ thống PHẢI hỗ trợ 3 vai trò người dùng: Học sinh, Giáo viên, Admin.
- **FR-03**: Hệ thống PHẢI chuyển hướng người dùng đến trang đích (landing page) phù hợp với vai trò sau khi đăng nhập thành công.
- **FR-04**: Hệ thống PHẢI đếm số lần đăng nhập sai liên tiếp cho mỗi tài khoản.
- **FR-05**: Hệ thống PHẢI khóa tài khoản tự động ngay lập tức sau 5 lần đăng nhập sai liên tiếp.
  - Tài khoản sẽ tự động mở khóa sau 30 phút.
  - Admin có quyền mở khóa tài khoản thủ công trước thời hạn này.
- **FR-06**: Hệ thống PHẢI hiển thị thông báo lỗi cụ thể cho các trường hợp: sai user, sai pass, tài khoản khóa (Tuân thủ YC-CN-02).
- **FR-07**: Session đăng nhập PHẢI có thời hạn (cấu hình được, mặc định 30 phút).
- **FR-08**: Mật khẩu PHẢI được mã hóa (hashing) trước khi so sánh (Tuân thủ Hiến pháp & YC-CN-02).

## Clarifications

### Session 2026-02-01
- Q: Spec hiện tại nói "liên hệ Admin" khi bị khóa. Chỉ hỗ trợ mở khóa thủ công hay nên có cơ chế tự động/tự phục vụ để giảm tải cho Admin? → A: Hybrid (Timeout + Admin): Tự động mở sau thời gian chờ (setup 30 phút) HOẶC Admin có thể mở sớm hơn.


### Các Thực thể Chính

- **User (Người dùng)**: ID, Tên tài khoản (unique), Mật khẩu (hash), Vai trò, Số lần và thời điểm login fail, Trạng thái (Active/Locked).
- **Role (Vai trò)**: Học sinh, Giáo viên, Admin.
- **Session**: Token/ID phiên làm việc.

## Tiêu chí Thành công *(bắt buộc)*

### Các Kết quả Đo lường được

- **SC-01**: Người dùng nhập đúng thông tin được chuyển vào trang chủ trong dưới 2 giây (trong điều kiện mạng tiêu chuẩn).
- **SC-02**: 100% tài khoản bị khóa sau chính xác 5 lần nhập sai liên tiếp.
- **SC-03**: Người dùng không thể truy cập các trang nội bộ (VD: /admin, /exam) khi chưa đăng nhập (bị redirect về login).
- **SC-04**: Hệ thống chịu tải được 100 concurrent login requests mà không lỗi (theo YC-PCN-03 support 1000 users, login chiếm một phần nhỏ).

## Giả định & Ràng buộc

- **Giả định**: Đã có dữ liệu tài khoản Admin/Giáo viên/Học sinh mẫu trong DB để test.
- **Ràng buộc**: Mật khẩu được lưu trữ dạng Hash (bcrypt/argon2), không lưu plain text.
- **Ràng buộc**: Giao diện Responsive (desktop/mobile) theo YC-PCN-02.
