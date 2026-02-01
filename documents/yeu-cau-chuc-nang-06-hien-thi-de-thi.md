# YÊU CẦU CHỨC NĂNG 06: HIỂN THỊ ĐỀ THI

## Mã yêu cầu
**YC-CN-06**

## Tên yêu cầu
Hiển thị đề thi khi người dùng muốn làm đề

## Mô tả
Hiển thị nội dung đề thi đầy đủ với các câu hỏi, đáp án và bộ đếm thời gian khi người dùng bắt đầu làm bài thi.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)

## Mục tiêu
Hiển thị đề thi đầy đủ và chính xác để người dùng làm bài

## Điều kiện tiên quyết
- Người dùng đã đăng nhập
- Người dùng đã mua gói đề chứa đề thi này
- Người dùng đã chọn đề thi cụ thể để làm bài

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng click vào nút "Làm bài" của một đề thi | Hệ thống nhận yêu cầu |
| 2 | | Hệ thống kiểm tra quyền truy cập (đã mua gói đề) |
| 3 | | Hệ thống truy vấn CSDL lấy thông tin đề thi |
| 4 | | Hệ thống lấy danh sách tất cả câu hỏi trong đề thi |
| 5 | | Hệ thống hiển thị thông tin đề thi:<br/>- Tên đề thi<br/>- Số câu hỏi<br/>- Thời gian làm bài<br/>- Hướng dẫn làm bài |
| 6 | Người dùng click "Bắt đầu làm bài" | Hệ thống bắt đầu bài thi |
| 7 | | Hệ thống khởi tạo bộ đếm ngược thời gian |
| 8 | | Hệ thống hiển thị giao diện làm bài với:<br/>- Danh sách câu hỏi (có thể phân trang)<br/>- Nội dung từng câu hỏi<br/>- Hình ảnh (nếu có)<br/>- 4 đáp án A, B, C, D<br/>- Bộ đếm thời gian<br/>- Nút "Nộp bài"<br/>- Nút "Lưu tạm" (nếu có) |
| 9 | Người dùng chọn đáp án cho các câu hỏi | Hệ thống ghi nhận đáp án đã chọn |
| 10 | Người dùng có thể di chuyển giữa các câu hỏi | Hệ thống lưu trạng thái làm bài |

## Luồng sự kiện thay thế

### Luồng 2a: Người dùng chưa mua gói đề
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 2a.1 | Người dùng chưa mua gói đề chứa đề thi này | Hệ thống hiển thị thông báo "Bạn cần mua gói đề để làm bài" |
| 2a.2 | | Hệ thống chuyển hướng đến trang mua gói đề |

### Luồng 4a: Không tìm thấy đề thi hoặc câu hỏi
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 4a.1 | Đề thi không tồn tại hoặc chưa có câu hỏi | Hệ thống hiển thị thông báo lỗi |
| 4a.2 | | Hệ thống quay lại danh sách đề thi |

### Luồng 10a: Người dùng đã làm đề này trước đó
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 10a.1 | Người dùng đã có bài làm trước đó (chưa nộp) | Hệ thống hỏi "Bạn có muốn tiếp tục bài làm cũ?" |
| 10a.2 | Người dùng chọn "Có" | Hệ thống khôi phục trạng thái làm bài cũ |
| 10a.3 | Người dùng chọn "Không" | Hệ thống tạo bài làm mới |

## Điều kiện sau
- Đề thi được hiển thị đầy đủ
- Bộ đếm thời gian hoạt động chính xác
- Người dùng có thể chọn đáp án và di chuyển giữa các câu hỏi
- Trạng thái làm bài được lưu liên tục

## Quy tắc nghiệp vụ
1. Mỗi câu hỏi chỉ có 1 đáp án đúng duy nhất
2. Người dùng có thể thay đổi đáp án đã chọn bất kỳ lúc nào trước khi nộp bài
3. Bộ đếm thời gian phải chính xác và không bị ảnh hưởng bởi reload trang
4. Khi hết thời gian, hệ thống tự động nộp bài
5. Hiển thị câu hỏi theo định dạng:
   - Số thứ tự câu hỏi
   - Nội dung câu hỏi (có thể có công thức toán)
   - Hình ảnh minh họa (nếu có)
   - 4 đáp án với radio button
6. Có thanh điều hướng (navigation) hiển thị trạng thái các câu (đã chọn/chưa chọn)
7. Phải hỗ trợ hiển thị công thức toán học (LaTeX/MathML)

## Dữ liệu đầu vào
- ID đề thi
- ID người dùng (từ session)

## Dữ liệu đầu ra
- Thông tin đề thi:
  - Mã đề
  - Tên đề
  - Thời gian làm bài
  - Số câu hỏi
- Danh sách câu hỏi:
  - Mã câu hỏi
  - Nội dung câu hỏi
  - Hình ảnh (nếu có)
  - Đáp án A, B, C, D
  - Vùng kiến thức
  - Mức độ khó
- Trạng thái làm bài:
  - Thời gian còn lại
  - Số câu đã chọn/chưa chọn
  - Đáp án đã chọn cho mỗi câu

## Độ ưu tiên
**Cao** - Chức năng cốt lõi của hệ thống

## Ghi chú
- Cần tối ưu hiệu suất để load nhanh danh sách câu hỏi
- Nên sử dụng AJAX để lưu trạng thái làm bài định kỳ (auto-save)
- Phải hỗ trợ hiển thị công thức toán học chuyên nghiệp (MathJax hoặc KaTeX)
- Cần xử lý trường hợp mất kết nối internet trong quá trình làm bài
- Nên có chức năng đánh dấu câu hỏi cần xem lại
- Giao diện cần thân thiện, dễ sử dụng, giống với đề thi thật
