# YÊU CẦU CHỨC NĂNG 08: PHẢN HỒI VỀ ĐỀ THI

## Mã yêu cầu
**YC-CN-08**

## Tên yêu cầu
Cung cấp phương thức phản hồi cho người dùng đến Người quản trị hệ thống và người tạo đề thi

## Mô tả
Cho phép người dùng gửi phản hồi, báo lỗi, hoặc góp ý về đề thi, câu hỏi đến người quản trị và người tạo đề.

## Actor (Tác nhân)
- Người dùng (Người làm đề / Học sinh)

## Mục tiêu
Gửi phản hồi thành công đến người quản trị hoặc người tạo đề thi

## Điều kiện tiên quyết
- Người dùng đã đăng nhập
- Người dùng đang xem hoặc đã làm một đề thi cụ thể

## Luồng sự kiện chính

| Bước | Hành động của người dùng | Phản hồi của hệ thống |
|------|-------------------------|----------------------|
| 1 | Người dùng click nút "Báo lỗi" hoặc "Phản hồi" | Hệ thống hiển thị form phản hồi |
| 2 | | Hệ thống hiển thị form với các trường:<br/>- Loại phản hồi (Lỗi câu hỏi/Lỗi đáp án/Góp ý/Khác)<br/>- Câu hỏi liên quan (tự động điền nếu phản hồi từ câu hỏi cụ thể)<br/>- Mô tả chi tiết<br/>- Ảnh chụp màn hình (tùy chọn) |
| 3 | Người dùng chọn loại phản hồi | Hệ thống ghi nhận |
| 4 | Người dùng nhập mô tả chi tiết | Hệ thống ghi nhận |
| 5 | Người dùng có thể đính kèm ảnh chụp màn hình | Hệ thống upload và lưu ảnh |
| 6 | Người dùng click "Gửi phản hồi" | Hệ thống xử lý |
| 7 | | Hệ thống kiểm tra tính hợp lệ của dữ liệu |
| 8 | | Hệ thống lưu phản hồi vào CSDL với thông tin:<br/>- Mã phản hồi<br/>- Người gửi<br/>- Loại phản hồi<br/>- Nội dung<br/>- Câu hỏi liên quan<br/>- Đề thi liên quan<br/>- Ảnh đính kèm (nếu có)<br/>- Thời gian gửi<br/>- Trạng thái (mới/đang xử lý/đã xử lý) |
| 9 | | Hệ thống gửi thông báo đến:<br/>- Admin<br/>- Giáo viên tạo đề (nếu có) |
| 10 | | Hệ thống hiển thị thông báo "Phản hồi đã được gửi thành công" |
| 11 | | Hệ thống tạo mã số phản hồi để người dùng theo dõi |

## Luồng sự kiện thay thế

### Luồng 7a: Dữ liệu không hợp lệ
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 7a.1 | Người dùng chưa nhập đầy đủ thông tin bắt buộc | Hệ thống thông báo "Vui lòng nhập đầy đủ thông tin" |
| 7a.2 | | Hệ thống highlight các trường còn thiếu |
| 7a.3 | | Trở lại bước 3 |

### Luồng 5a: Upload ảnh thất bại
| Bước | Điều kiện | Hành động |
|------|-----------|----------|
| 5a.1 | File ảnh quá lớn hoặc sai định dạng | Hệ thống thông báo lỗi cụ thể |
| 5a.2 | | Cho phép người dùng chọn file khác |

## Điều kiện sau
- Phản hồi được lưu vào hệ thống
- Admin và giáo viên tạo đề nhận được thông báo
- Người dùng nhận được mã số để theo dõi phản hồi

## Quy tắc nghiệp vụ
1. Các loại phản hồi:
   - Lỗi câu hỏi (sai chính tả, sai công thức)
   - Lỗi đáp án (đáp án đúng bị sai, nhiều đáp án đúng...)
   - Lỗi hệ thống (không hiển thị đúng, lỗi tính điểm...)
   - Góp ý (cải thiện câu hỏi, đề thi...)
   - Khác
2. Trường "Mô tả chi tiết" là bắt buộc
3. Giới hạn kích thước ảnh: tối đa 5MB
4. Định dạng ảnh được phép: JPG, PNG, GIF
5. Admin và giáo viên liên quan sẽ nhận thông báo ngay lập tức
6. Người dùng có thể xem trạng thái xử lý phản hồi của mình

## Dữ liệu đầu vào
- ID người gửi (từ session)
- Loại phản hồi (enum)
- Mô tả chi tiết (text)
- ID câu hỏi liên quan (nếu có)
- ID đề thi liên quan (nếu có)
- File ảnh (tùy chọn, max 5MB)

## Dữ liệu đầu ra
- Mã phản hồi (để theo dõi)
- Thông báo thành công
- Thời gian dự kiến xử lý

## Độ ưu tiên
**Trung bình** - Quan trọng cho việc cải thiện chất lượng nhưng không cấp bách

## Ghi chú
- Nên có trang cho người dùng xem danh sách phản hồi đã gửi và trạng thái xử lý
- Admin/Giáo viên cần có trang quản lý phản hồi để trả lời, xử lý
- Có thể thêm tính năng đánh giá câu hỏi (sao, like/dislike)
- Cân nhắc khen thưởng cho người dùng tích cực báo lỗi
