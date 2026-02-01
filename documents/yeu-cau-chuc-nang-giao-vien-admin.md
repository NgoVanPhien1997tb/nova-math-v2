# DANH SÁCH CÁC YÊU CẦU CHỨC NĂNG CỦA GIÁO VIÊN VÀ ADMIN

## YC-CN-09: TẠO GÓI ĐỀ MỚI

**Actor**: Giáo viên, Admin

**Mô tả**: Tạo một gói đề thi mới với các thông tin cơ bản.

**Dữ liệu đầu vào**:
- Tên gói đề
- Mô tả/Nội dung gói đề  
- Giá
- Hình ảnh minh họa

**Quy tắc nghiệp vụ**:
- Mã gói đề tự động sinh
- Tất cả thông tin bắt buộc nhập trừ hình ảnh
- Giá phải > 0

---

## YC-CN-10: THÊM ĐỀ THI VÀO GÓI ĐỀ

**Actor**: Giáo viên, Admin

**Mô tả**: Thêm một đề thi mới vào gói đề đã tạo.

**Dữ liệu đầu vào**:
- Mã gói đề đích
- Tên đề thi
- Thời gian làm bài (phút)
- Mô tả đề thi

**Quy tắc nghiệp vụ**:
- Mã đề thi tự động sinh duy nhất
- Thời gian làm bài: 30-180 phút
- Một gói đề có thể chứa nhiều đề thi

---

## YC-CN-11: SỬA ĐỀ THI TRONG GÓI ĐỀ

**Actor**: Giáo viên, Admin

**Mô tả**: Chỉnh sửa thông tin của một đề thi đã tồn tại.

**Dữ liệu đầu vào**:
- Mã đề thi cần sửa
- Các thông tin cần cập nhật (tên, thời gian, mô tả...)

**Quy tắc nghiệp vụ**:
- Không cho phép thay đổi mã đề thi
- Nếu đã có người làm bài, cảnh báo trước khi sửa
- Lưu lịch sử thay đổi

---

## YC-CN-12: THÊM TÀI KHOẢN GIÁO VIÊN

**Actor**: Admin

**Mô tả**: Admin tạo tài khoản mới cho giáo viên.

**Dữ liệu đầu vào**:
- Họ tên
- Số điện thoại
- Tên tài khoản (duy nhất)
- Mật khẩu (tối thiểu 8 ký tự)

**Quy tắc nghiệp vụ**:
- Chỉ Admin mới có quyền tạo tài khoản giáo viên
- Tên tài khoản không trùng lặp
- Tài khoản mới mặc định ở trạng thái "Hoạt động"

---

## YC-CN-13: KHÓA TÀI KHOẢN GIÁO VIÊN

**Actor**: Admin

**Mô tả**: Admin thay đổi trạng thái tài khoản giáo viên (khóa/mở khóa).

**Dữ liệu đầu vào**:
- Mã tài khoản giáo viên
- Trạng thái mới (Hoạt động/Khóa)
- Lý do khóa (nếu khóa tài khoản)

**Quy tắc nghiệp vụ**:
- Tài khoản bị khóa không thể đăng nhập
- Gói đề/đề thi/câu hỏi của giáo viên bị khóa vẫn hiển thị
- Có thể mở khóa lại tài khoản bất kỳ lúc nào

---

## YC-CN-14: XEM DANH SÁCH GIÁO VIÊN

**Actor**: Admin

**Mô tả**: Admin xem danh sách tất cả giáo viên trong hệ thống.

**Dữ liệu đầu ra**:
- Danh sách giáo viên với:
  - Họ tên
  - Tên tài khoản
  - SĐT
  - Trạng thái (Hoạt động/Khóa)
  - Số gói đề đã tạo
  - Ngày tạo tài khoản

**Quy tắc nghiệp vụ**:
- Có phân trang nếu danh sách quá dài
- Có bộ lọc theo trạng thái
- Có chức năng tìm kiếm theo tên, SĐT

---

## YC-CN-15: XEM TỔNG DOANH THU

**Actor**: Admin

**Mô tả**: Xem tổng số lượng và tổng doanh thu các gói đề đã bán.

**Dữ liệu đầu ra**:
- Tổng số gói đề đã bán
- Tổng doanh thu
- Biểu đồ doanh thu theo thời gian
- Top gói đề bán chạy

**Quy tắc nghiệp vụ**:
- Lọc theo khoảng thời gian (ngày/tháng/năm)
- Xuất báo cáo ra Excel/PDF
- Hiển thị biểu đồ trực quan

---

## YC-CN-16: THỐNG KÊ CHI TIẾT TỪNG GÓI ĐỀ

**Actor**: Admin

**Mô tả**: Thống kê số lượng đã bán và doanh thu của từng gói đề cụ thể.

**Dữ liệu đầu ra**:
Cho mỗi gói đề:
- Tên gói đề
- Giá bán
- Số lượng đã bán
- Tổng doanh thu
- Số lượt làm bài
- Điểm trung bình
- Đánh giá của người dùng

**Quy tắc nghiệp vụ**:
- Sắp xếp theo doanh thu/số lượng bán
- Lọc theo khoảng thời gian
- Xuất báo cáo chi tiết

---

## YÊU CẦU BỔ SUNG CHO GIÁO VIÊN/ADMIN

### Thêm câu hỏi vào đề thi
- Nhập nội dung câu hỏi (hỗ trợ công thức toán)
- Upload hình ảnh minh họa (tùy chọn)
- Nhập 4 đáp án A, B, C, D
- Chọn đáp án đúng
- Chọn vùng kiến thức
- Chọn mức độ khó (dễ/trung bình/khó)

### Sửa câu hỏi
- Tìm câu hỏi theo mã
- Hiển thị thông tin chi tiết câu hỏi
- Cho phép chỉnh sửa mọi thông tin
- Lưu lại lịch sử chỉnh sửa
- Cảnh báo nếu câu hỏi đã được sử dụng trong đề thi

### Xem chi tiết câu hỏi/đề thi/gói đề
- Hiển thị đầy đủ thông tin
- Hiển thị thống kê liên quan
- Cho phép chuyển nhanh sang chức năng sửa/xóa
