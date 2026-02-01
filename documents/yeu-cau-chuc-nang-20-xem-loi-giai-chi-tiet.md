# YÊU CẦU CHỨC NĂNG 20: XEM LỜI GIẢI CHI TIẾT

## Mã yêu cầu
**YC-CN-20**

## Tên yêu cầu
Xem lời giải chi tiết và phân tích kết quả

## Mô tả
Sau khi nộp bài và nhận điểm, hệ thống cung cấp lời giải chi tiết (văn bản/hình ảnh/video) cho từng câu hỏi để học sinh hiểu được phương pháp giải và sửa lỗi sai.

## Actor (Tác nhân)
- Người dùng (Học sinh)

## Mục tiêu
- Cung cấp giá trị học tập thực sự, không chỉ là điểm số.
- Giúp học sinh tự học và tiến bộ sau mỗi lần làm bài.

## Luồng sự kiện chính

1. Sau khi nộp bài (YC-CN-07), hệ thống hiển thị màn hình kết quả tổng quan.
2. Người dùng nhấn nút "Xem lời giải chi tiết".
3. Hệ thống hiển thị danh sách toàn bộ câu hỏi trong đề:
   - Câu làm đúng: Đánh dấu xanh.
   - Câu làm sai: Đánh dấu đỏ (Hiển thị đáp án đã chọn và đáp án đúng).
   - Câu chưa làm: Đánh dấu xám.
4. Dưới mỗi câu hỏi, hệ thống hiển thị phần "Hướng dẫn giải chi tiết":
   - Các bước giải toán.
   - Công thức áp dụng.
   - Lưu ý/Mẹo làm bài nhanh (nếu có).
5. Người dùng có thể lọc danh sách: "Chỉ xem câu sai" hoặc "Chỉ xem câu chưa làm".

## Quy tắc nghiệp vụ
1. Lời giải chi tiết chỉ được hiển thị **sau khi** người dùng đã nộp bài (để tránh lộ đáp án).
2. Lời giải phải hỗ trợ đầy đủ ký hiệu toán học (LaTeX/MathJax).
3. Hệ thống ghi lại các vùng kiến thức học sinh hay làm sai để gợi ý luyện tập thêm (Tính năng phân tích nâng cao).

## Độ ưu tiên
**Rất cao**
