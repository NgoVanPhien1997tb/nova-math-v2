# YÊU CẦU CHỨC NĂNG 03: XEM DANH SÁCH THÔNG TIN CÁC GÓI ĐỀ

## Mã yêu cầu
**YC-CN-03**

## Tên yêu cầu
Xem danh sách thông tin các gói đề

## Mô tả
Cho phép người dùng xem danh sách tất cả các gói đề thi có trong hệ thống cùng với các thông tin cơ bản về mỗi gói đề.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)
- Giáo viên
- Admin

## Mục tiêu
Xem được danh sách tất cả các gói đề trong hệ thống với đầy đủ thông tin

## Điều kiện tiên quyết
- Người dùng đã truy cập vào hệ thống (có thể chưa đăng nhập)

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng click vào "Danh sách gói đề" | Hệ thống nhận yêu cầu |
| 2 | | Hệ thống truy vấn CSDL lấy tất cả các gói đề |
| 3 | | Hệ thống hiển thị danh sách gói đề với các thông tin:<br/>- Tên gói đề<br/>- Mô tả/Nội dung gói đề<br/>- Số lượng đề trong gói<br/>- Giá của gói đề<br/>- Người tạo (giáo viên/admin)<br/>- Hình ảnh minh họa (nếu có) |
| 4 | Người dùng có thể xem chi tiết từng gói đề | Hệ thống hiển thị thông tin chi tiết khi được yêu cầu |

## Luồng sự kiện thay thế

### Luồng 2a: Không có gói đề nào trong hệ thống
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 2a.1 | CSDL chưa có gói đề nào | Hệ thống hiển thị thông báo "Hiện chưa có gói đề nào" |
| 2a.2 | | Hệ thống đề xuất người dùng quay lại sau |

## Điều kiện sau
- Người dùng xem được danh sách các gói đề
- Người dùng có thể quyết định chọn gói đề để xem chi tiết hoặc mua

## Quy tắc nghiệp vụ
1. Danh sách gói đề hiển thị cho tất cả người dùng, kể cả chưa đăng nhập
2. Thông tin hiển thị phải đầy đủ để người dùng có cơ sở quyết định mua
3. Gói đề có thể được sắp xếp theo:
   - Mới nhất
   - Giá (tăng/giảm dần)
   - Số lượng đề
   - Độ phổ biến (số lượt mua)
4. Có thể lọc gói đề theo:
   - Mức giá
   - Vùng kiến thức
   - Số lượng đề

## Dữ liệu đầu vào
- Không có (hoặc các tiêu chí lọc/sắp xếp nếu có)

## Dữ liệu đầu ra
- Danh sách các gói đề với thông tin:
  - Mã gói đề
  - Tên gói đề
  - Mô tả/Nội dung
  - Số lượng đề trong gói
  - Giá
  - Người tạo
  - Hình ảnh
  - Ngày tạo
  - Số lượt đã bán

## Độ ưu tiên
**Cao** - Chức năng quan trọng để người dùng tìm hiểu sản phẩm

## Ghi chú
- Chức năng này không yêu cầu đăng nhập để tăng khả năng tiếp cận
- Nên có chức năng tìm kiếm gói đề theo từ khóa
- Có thể hiển thị nhãn "Mới", "Bán chạy", "Khuyến mãi" để thu hút người dùng
- Nên có phân trang khi số lượng gói đề quá nhiều
