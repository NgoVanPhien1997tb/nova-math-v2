# YÊU CẦU CHỨC NĂNG 04: THANH TOÁN TRỰC TUYẾN

## Mã yêu cầu
**YC-CN-04**

## Tên yêu cầu
Cung cấp phương thức thanh toán trực tuyến khi người dùng muốn mua các gói đề

## Mô tả
Cho phép người dùng thanh toán trực tuyến thông qua API ngân hàng để mua các gói đề thi, thay vì thanh toán thủ công.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)

## Mục tiêu
Hoàn thành thanh toán gói đề thành công thông qua phương thức trực tuyến

## Điều kiện tiên quyết
- Người dùng đã đăng nhập vào hệ thống
- Người dùng đã chọn gói đề muốn mua
- Người dùng chưa sở hữu gói đề này

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng chọn gói đề muốn mua | Hệ thống hiển thị thông tin gói đề |
| 2 | Người dùng click nút "Mua gói đề" | Hệ thống hiển thị trang xác nhận thanh toán |
| 3 | | Hệ thống hiển thị:<br/>- Thông tin gói đề (tên, số lượng đề, giá)<br/>- Tổng tiền cần thanh toán<br/>- Các phương thức thanh toán có sẵn |
| 4 | Người dùng chọn phương thức thanh toán | Hệ thống ghi nhận lựa chọn |
| 5 | Người dùng click "Thanh toán" | Hệ thống xử lý yêu cầu thanh toán |
| 6 | | Hệ thống kết nối với API ngân hàng/cổng thanh toán |
| 7 | | Hệ thống chuyển hướng người dùng đến trang thanh toán của ngân hàng |
| 8 | Người dùng thực hiện thanh toán trên trang ngân hàng | Ngân hàng xử lý giao dịch |
| 9 | | Ngân hàng gửi kết quả giao dịch về hệ thống |
| 10 | | Hệ thống kiểm tra kết quả thanh toán |
| 11 | | Hệ thống cập nhật gói đề vào danh sách đã mua của người dùng |
| 12 | | Hệ thống gửi email xác nhận thanh toán thành công |
| 13 | | Hệ thống hiển thị thông báo "Thanh toán thành công" |
| 14 | | Hệ thống chuyển hướng đến trang danh sách gói đề đã mua |

## Luồng sự kiện thay thế

### Luồng 3a: Người dùng click "Không đồng ý"
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 3a.1 | Người dùng click vào nút "Không đồng ý" hoặc "Hủy" | Hệ thống hủy yêu cầu thanh toán |
| 3a.2 | | Hệ thống quay lại trang "Gói đề để mua" |

### Luồng 9a: Thanh toán thất bại
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 9a.1 | Ngân hàng trả về kết quả thanh toán thất bại | Hệ thống ghi nhận lỗi |
| 9a.2 | | Hệ thống hiển thị thông báo "Thanh toán thất bại" với lý do cụ thể |
| 9a.3 | | Hệ thống đề xuất người dùng thử lại hoặc chọn phương thức thanh toán khác |

### Luồng 9b: Mất kết nối với cổng thanh toán
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 9b.1 | Mất kết nối với API ngân hàng | Hệ thống ghi nhận lỗi |
| 9b.2 | | Hệ thống thông báo "Có lỗi xảy ra, vui lòng thử lại sau" |
| 9b.3 | | Hệ thống lưu thông tin giao dịch chưa hoàn thành để xử lý sau |

### Luồng 8a: Người dùng hủy giao dịch trên trang ngân hàng
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 8a.1 | Người dùng hủy giao dịch trên trang ngân hàng | Ngân hàng thông báo hủy giao dịch |
| 8a.2 | | Hệ thống nhận thông báo hủy từ ngân hàng |
| 8a.3 | | Hệ thống hiển thị thông báo "Giao dịch đã bị hủy" |
| 8a.4 | | Hệ thống quay lại trang chi tiết gói đề |

## Điều kiện sau
- Giao dịch thanh toán được ghi nhận trong hệ thống
- Gói đề được thêm vào danh sách đã mua của người dùng (nếu thanh toán thành công)
- Lịch sử giao dịch được lưu lại

## Quy tắc nghiệp vụ
1. Mỗi gói đề chỉ cần mua một lần
2. Thanh toán phải được xác nhận từ ngân hàng/cổng thanh toán trước khi cấp quyền truy cập
3. Hệ thống phải lưu lại tất cả thông tin giao dịch để đối chiếu
4. Phương thức thanh toán được hỗ trợ:
   - Thẻ ATM nội địa
   - Thẻ tín dụng/ghi nợ quốc tế
   - Ví điện tử
   - QR Code
5. Thời gian chờ thanh toán tối đa: 15 phút
6. Nếu quá thời gian chờ, giao dịch sẽ tự động hủy

## Dữ liệu đầu vào
- Mã gói đề
- Thông tin người mua (từ session)
- Phương thức thanh toán
- Thông tin thanh toán (theo yêu cầu của cổng thanh toán)

## Dữ liệu đầu ra
- Mã giao dịch
- Trạng thái giao dịch (thành công/thất bại/đang xử lý)
- Thời gian giao dịch
- Số tiền
- Phương thức thanh toán
- Thông tin gói đề đã mua (nếu thành công)

## Độ ưu tiên
**Cao** - Chức năng cốt lõi của hệ thống, ảnh hưởng trực tiếp đến doanh thu

## Ghi chú
- Cần tích hợp với các cổng thanh toán uy tín (VNPay, MoMo, ZaloPay, v.v.)
- Phải đảm bảo bảo mật thông tin thanh toán (tuân thủ PCI DSS nếu xử lý thẻ)
- Cần có cơ chế xử lý giao dịch treo (pending) do lỗi mạng
- Nên có chức năng hoàn tiền trong trường hợp cần thiết
- Lưu log chi tiết tất cả các giao dịch để có thể đối soát với ngân hàng
