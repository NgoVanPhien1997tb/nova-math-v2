# YÊU CẦU CHỨC NĂNG 07: LƯU VÀ CHẤM ĐỀ THI

## Mã yêu cầu
**YC-CN-07**

## Tên yêu cầu
Lưu lại thông tin đề thi khi người dùng đã làm và chấm điểm

## Mô tả
Lưu lại kết quả làm bài của người dùng và tự động chấm điểm dựa trên đáp án đúng.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)

## Mục tiêu
- Lưu trữ kết quả làm bài của người dùng
- Chấm điểm tự động và hiển thị kết quả
- Lưu lịch sử làm bài để xem lại

## Điều kiện tiên quyết
- Người dùng đã đăng nhập
- Người dùng đã làm đề thi (chọn đáp án)
- Người dùng đã nộp bài hoặc hết thời gian

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng click nút "Nộp bài" | Hệ thống hiển thị xác nhận |
| 2 | Người dùng xác nhận nộp bài | Hệ thống nhận yêu cầu nộp bài |
| 3 | | Hệ thống lưu tất cả đáp án đã chọn vào CSDL |
| 4 | | Hệ thống lấy đáp án đúng từ CSDL |
| 5 | | Hệ thống so sánh đáp án đã chọn với đáp án đúng |
| 6 | | Hệ thống tính số câu đúng, câu sai |
| 7 | | Hệ thống tính điểm theo công thức: Điểm = (Số câu đúng / Tổng số câu) × 10 |
| 8 | | Hệ thống lưu kết quả vào CSDL với thông tin:<br/>- Điểm số<br/>- Số câu đúng/sai<br/>- Thời gian làm bài<br/>- Thời gian nộp bài<br/>- Chi tiết từng câu trả lời |
| 9 | | Hệ thống hiển thị kết quả:<br/>- Điểm số<br/>- Số câu đúng/sai<br/>- Thời gian làm bài<br/>- Xếp hạng (nếu có) |
| 10 | | Hệ thống cung cấp tùy chọn xem chi tiết bài làm |

## Luồng sự kiện thay thế

### Luồng 1a: Hết thời gian làm bài
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 1a.1 | Thời gian làm đề hết | Hệ thống tự động nộp bài |
| 1a.2 | | Hệ thống tự động tính những câu chưa được chọn là câu sai |
| 1a.3 | | Chuyển sang bước 3 của luồng chính |

### Luồng 2a: Người dùng hủy nộp bài
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 2a.1 | Người dùng click "Hủy" tại màn hình xác nhận | Hệ thống quay lại giao diện làm bài |
| 2a.2 | | Người dùng tiếp tục làm bài |

## Điều kiện sau
- Kết quả làm bài được lưu vào CSDL
- Điểm số được tính toán chính xác
- Người dùng có thể xem lại bài làm và đáp án đúng
- Lịch sử làm bài được cập nhật

## Quy tắc nghiệp vụ
1. Công thức tính điểm: Điểm = (Số câu đúng / Tổng số câu) × 10
2. Làm tròn điểm đến 2 chữ số thập phân
3. Câu hỏi không chọn đáp án = câu sai
4. Lưu lại thời gian thực tế làm bài (từ lúc bắt đầu đến lúc nộp)
5. Cho phép làm lại đề thi nhiều lần
6. Lưu lịch sử tất cả các lần làm bài
7. Hiển thị lần làm bài tốt nhất (điểm cao nhất)

## Dữ liệu đầu vào
- ID đề thi
- ID người dùng
- Danh sách đáp án đã chọn (mảng câu hỏi và đáp án)
- Thời gian bắt đầu làm bài
- Thời gian nộp bài

## Dữ liệu đầu ra
- Kết quả làm bài:
  - Điểm số (0-10)
  - Số câu đúng
  - Số câu sai
  - Số câu bỏ trống
  - Thời gian làm bài
  - Xếp hạng (so với những người khác)
  - Phân tích theo vùng kiến thức
- Chi tiết từng câu:
  - Câu hỏi
  - Đáp án đã chọn
  - Đáp án đúng
  - Kết quả (đúng/sai)

## Độ ưu tiên
**Cao** - Chức năng cốt lõi, không thể thiếu

## Ghi chú
- Cần xử lý đồng thời khi nhiều người nộp bài cùng lúc
- Nên có thống kê phân tích chi tiết (theo vùng kiến thức, mức độ...)
- Có thể hiển thị biểu đồ so sánh điểm của người dùng qua các lần làm
- Cân nhắc thêm tính năng lưu bài làm tạm (draft) khi chưa hoàn thành
