# YÊU CẦU CHỨC NĂNG 05: XEM DANH SÁCH GÓI ĐỀ ĐÃ MUA

## Mã yêu cầu
**YC-CN-05**

## Tên yêu cầu
Người làm đề xem được danh sách các gói đề đã mua và danh sách các đề trong một gói đề

## Mô tả
Cho phép người dùng xem danh sách các gói đề mà họ đã mua và xem chi tiết danh sách các đề thi trong từng gói đề.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)

## Mục tiêu
Xem được danh sách gói đề đã mua và các đề thi trong mỗi gói

## Điều kiện tiên quyết
- Người dùng đã đăng nhập vào hệ thống
- Người dùng đã mua ít nhất một gói đề

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng click vào "Gói đề đã mua" hoặc "Thông tin cá nhân" | Hệ thống nhận yêu cầu |
| 2 | | Hệ thống truy vấn CSDL lấy danh sách gói đề đã mua của người dùng |
| 3 | | Hệ thống hiển thị danh sách gói đề đã mua với các thông tin:<br/>- Tên gói đề<br/>- Số lượng đề trong gói<br/>- Ngày mua<br/>- Trạng thái (đã hoàn thành/đang học) |
| 4 | Người dùng click vào một gói đề cụ thể | Hệ thống nhận yêu cầu xem chi tiết |
| 5 | | Hệ thống truy vấn CSDL lấy danh sách đề thi trong gói đề đó |
| 6 | | Hệ thống hiển thị danh sách đề thi với các thông tin:<br/>- Tên/Mã đề thi<br/>- Số lượng câu hỏi<br/>- Thời gian làm bài<br/>- Trạng thái (chưa làm/đã làm/đang làm)<br/>- Điểm số (nếu đã làm) |
| 7 | Người dùng có thể chọn làm đề hoặc xem lại đề đã làm | Hệ thống chuyển đến chức năng tương ứng |

## Luồng sự kiện thay thế

### Luồng 2a: Chưa mua gói đề nào
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 2a.1 | Người dùng chưa mua gói đề nào | Hệ thống hiển thị thông báo "Bạn chưa mua gói đề nào" |
| 2a.2 | | Hệ thống hiển thị nút "Xem danh sách gói đề" để khuyến khích mua |

### Luồng 5a: Gói đề chưa có đề thi nào
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 5a.1 | Gói đề chưa có đề thi nào (trường hợp hiếm) | Hệ thống hiển thị thông báo "Gói đề này đang được cập nhật" |
| 5a.2 | | Hệ thống khuyến nghị người dùng quay lại sau |

## Điều kiện sau
- Người dùng xem được danh sách gói đề đã mua
- Người dùng xem được danh sách đề thi trong từng gói đề
- Người dùng có thể chọn đề để làm bài

## Quy tắc nghiệp vụ
1. Chỉ hiển thị các gói đề mà người dùng đã thanh toán thành công
2. Danh sách đề thi trong gói phải đầy đủ, không giới hạn
3. Hiển thị trạng thái làm bài của từng đề:
   - Chưa làm: màu xám hoặc chưa có điểm
   - Đang làm: màu vàng (có thời gian còn lại)
   - Đã làm: màu xanh (có điểm số)
4. Sắp xếp gói đề theo thứ tự mua mới nhất
5. Sắp xếp đề thi trong gói theo thứ tự từ dễ đến khó hoặc theo số thứ tự

## Dữ liệu đầu vào
- ID người dùng (từ session)
- ID gói đề (khi xem chi tiết)

## Dữ liệu đầu ra
### Danh sách gói đề đã mua:
- Mã gói đề
- Tên gói đề
- Số lượng đề
- Ngày mua
- Giá đã mua
- Tiến độ hoàn thành (X/Y đề đã làm)

### Danh sách đề thi trong gói:
- Mã đề thi
- Tên đề thi
- Số câu hỏi
- Thời gian làm bài
- Trạng thái (chưa làm/đang làm/đã làm)
- Điểm số (nếu đã làm)
- Lần làm gần nhất (nếu có)

## Độ ưu tiên
**Cao** - Chức năng cần thiết để người dùng sử dụng sản phẩm đã mua

## Ghi chú
- Nên có chức năng tìm kiếm đề thi trong gói đề
- Có thể thêm bộ lọc theo trạng thái (chưa làm/đã làm)
- Nên hiển thị tiến độ hoàn thành của từng gói đề (progress bar)
- Có thể thêm chức năng chia sẻ thành tích lên mạng xã hội
