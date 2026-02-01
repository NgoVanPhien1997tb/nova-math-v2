# YÊU CẦU CHỨC NĂNG 19: NGÂN HÀNG CÂU HỎI

## Mã yêu cầu
**YC-CN-19**

## Tên yêu cầu
Quản lý Ngân hàng câu hỏi tập trung

## Mô tả
Tách biệt quản lý câu hỏi khỏi đề thi. Câu hỏi được lưu trữ trong một kho chung (Ngân hàng), cho phép giáo viên tái sử dụng câu hỏi cho nhiều đề thi khác nhau.

## Actor (Tác nhân)
- Giáo viên
- Admin

## Mục tiêu
- Tăng hiệu suất soạn thảo đề thi.
- Dễ dàng quản lý chất lượng câu hỏi, sửa lỗi một nơi và cập nhật mọi nơi.
- Hỗ trợ sinh đề thi tự động.

## Luồng sự kiện chính

### Luồng 1: Thêm câu hỏi vào ngân hàng
1. Giáo viên chọn "Quản lý ngân hàng câu hỏi".
2. Nhập nội dung câu hỏi, đáp án, lời giải chi tiết (YC-CN-20).
3. Gán các nhãn (Tag): 
   - Vùng kiến thức (Hàm số, Hình học...).
   - Mức độ (Nhận biết, Thông hiểu, Vận dụng, Vận dụng cao).
   - Năm thi (ví dụ: 2024, 2025).
4. Hệ thống kiểm tra trùng lặp nội dung câu hỏi.
5. Lưu vào kho chung của giáo viên hoặc kho hệ thống.

### Luồng 2: Tạo đề thi từ ngân hàng
1. Giáo viên tạo đề mới.
2. Chọn chức năng "Thêm câu hỏi từ ngân hàng".
3. Hệ thống hiển thị bộ lọc: Theo vùng kiến thức, mức độ, từ khóa.
4. Giáo viên chọn các câu hỏi mong muốn và nhấn "Thêm vào đề".
5. Hệ thống tự động đánh số thứ tự và tạo đề thi hoàn chỉnh.

## Quy tắc nghiệp vụ
1. Một câu hỏi có thể thuộc về nhiều đề thi khác nhau.
2. Khi sửa nội dung câu hỏi trong ngân hàng, hệ thống phải thông báo các đề thi nào sẽ bị ảnh hưởng.
3. Giáo viên chỉ có thể xem/dùng câu hỏi của mình hoặc câu hỏi "Công khai" từ Admin.
4. Mỗi câu hỏi phải có mã định danh duy nhất (UUID).

## Độ ưu tiên
**Trung bình - Cao**
