# YÊU CẦU CHỨC NĂNG 18: LỊCH SỬ GIAO DỊCH

## Mã yêu cầu
**YC-CN-18**

## Tên yêu cầu
Xem lịch sử giao dịch và đối soát thanh toán

## Mô tả
Cho phép người dùng xem lại tất cả các giao dịch nạp tiền/mua gói đề đã thực hiện trên hệ thống. Giúp minh bạch tài chính và hỗ trợ đối soát khi có lỗi.

## Actor (Tác nhân)
- Người dùng (Học sinh)
- Admin (Quản trị viên)

## Mục tiêu
- Người dùng kiểm soát được số tiền đã chi tiêu.
- Admin có bằng chứng để xử lý các khiếu nại về thanh toán.

## Luồng sự kiện chính

### Luồng 1: Người dùng xem lịch sử
1. Người dùng vào mục "Lịch sử mua hàng" hoặc "Ví của tôi".
2. Hệ thống hiển thị danh sách các giao dịch được sắp xếp mới nhất lên đầu.
3. Mỗi bản ghi bao gồm:
   - Mã giao dịch (Transaction ID).
   - Tên gói đề đã mua.
   - Số tiền.
   - Phương thức thanh toán (Ngân hàng, Ví điện tử...).
   - Trạng thái (Thành công, Thất bại, Đang xử lý).
   - Ngày giờ giao dịch.
4. Người dùng có thể nhấn vào một giao dịch để xem chi tiết hoặc in hóa đơn điện tử.

### Luồng 2: Admin tra soát và xử lý giao dịch lỗi
1. Admin vào "Quản lý giao dịch".
2. Tìm kiếm giao dịch theo Tên tài khoản hoặc Mã giao dịch từ phía ngân hàng.
3. Hệ thống hiển thị trạng thái thực tế của giao dịch trong database.
4. Đối với các giao dịch "Thành công" nhưng người dùng chưa nhận được gói đề (do lỗi callback), Admin có nút "Kích hoạt thủ công".
5. Hệ thống yêu cầu xác nhận và ghi log hành động của Admin.

## Quy tắc nghiệp vụ
1. Lịch sử giao dịch không được phép xóa (immutable).
2. Trạng thái giao dịch phải được cập nhật đồng bộ với cổng thanh toán.
3. Mỗi giao dịch thành công phải gửi một thông báo (Notify) hoặc Email cho người dùng.
4. Quyền "Kích hoạt thủ công" chỉ dành cho Admin cấp cao và phải ghi lại log chi tiết lý do.

## Độ ưu tiên
**Cao**
