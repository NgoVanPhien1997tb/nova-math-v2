
---
description: Thực thi kế hoạch triển khai bằng cách xử lý và thực hiện tất cả các task được định nghĩa trong tasks.md
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Dàn ý

1. Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` từ repo root và parse FEATURE_DIR và danh sách AVAILABLE_DOCS. Tất cả đường dẫn phải là tuyệt đối. Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape.

2. **Kiểm tra trạng thái các checklist** (nếu FEATURE_DIR/checklists/ tồn tại):
   - Quét tất cả các file checklist trong thư mục checklists/
   - Đối với mỗi checklist, đếm: Total, Completed (`[x]`), Incomplete (`[ ]`).
   - Tạo bảng trạng thái (PASS/FAIL).
   - **Nếu bất kỳ checklist nào chưa hoàn thành**: Dừng và hỏi: "Một số checklist chưa hoàn thành. Bạn có muốn tiếp tục triển khai không? (yes/no)". Chờ phản hồi.
   - **Nếu tất cả checklist hoàn thành**: Tự động chuyển sang bước 3.

3. Load và phân tích ngữ cảnh triển khai:
   - **BẮT BUỘC**: Đọc tasks.md cho danh sách task đầy đủ và kế hoạch thực thi.
   - **BẮT BUỘC**: Đọc plan.md cho tech stack, kiến trúc, và cấu trúc file.
   - **NẾU CÓ**: Đọc data-model.md, contracts/, research.md, quickstart.md.

4. **Xác minh Thiết lập Dự án**:
   - **BẮT BUỘC**: Tạo/xác minh các file ignore (gitignore, dockerignore, v.v.) dựa trên thiết lập dự án thực tế.
   - Kiểm tra nếu là git repo -> .gitignore
   - Kiểm tra Docker, ESLint, Prettier, Terraform, v.v. để tạo file ignore tương ứng.
   - **Các mẫu phổ biến theo Công nghệ**: Node.js (`node_modules`), Python (`venv`, `__pycache__`), Java (`target`), v.v. Xem chi tiết trong plan gốc.

5. Parse cấu trúc tasks.md và trích xuất:
   - **Các Phase task**: Setup, Tests, Core, Integration, Polish
   - **Sự phụ thuộc task**: Quy tắc thực thi tuần tự vs song song
   - **Chi tiết task**: ID, mô tả, đường dẫn file, marker song song [P]
   - **Luồng thực thi**: Thứ tự và yêu cầu phụ thuộc

6. Thực thi triển khai theo kế hoạch task:
   - **Thực thi theo từng Phase**: Hoàn thành từng phase trước khi chuyển sang phase tiếp theo
   - **Tôn trọng sự phụ thuộc**: Chạy task tuần tự theo thứ tự, task song song [P] có thể chạy cùng nhau
   - **Tuân thủ cách tiếp cận TDD**: Thực thi các task test trước các task triển khai tương ứng của chúng
   - **Phối hợp dựa trên file**: Các task ảnh hưởng đến cùng một file phải chạy tuần tự
   - **Checkpoint xác thực**: Xác minh hoàn thành mỗi phase trước khi tiếp tục

7. Quy tắc thực thi triển khai:
   - **Setup trước**: Khởi tạo cấu trúc dự án, dependencies, cấu hình
   - **Tests trước code**: Nếu bạn cần viết test cho contracts, entities, và kịch bản tích hợp
   - **Phát triển cốt lõi (Core)**: Triển khai models, services, CLI commands, endpoints
   - **Công việc tích hợp**: Kết nối Database, middleware, logging, dịch vụ bên ngoài
   - **Đánh bóng và xác thực**: Unit tests, tối ưu hiệu năng, tài liệu

8. Theo dõi tiến độ và xử lý lỗi:
   - Báo cáo tiến độ sau mỗi task hoàn thành
   - Dừng thực thi nếu bất kỳ task không song song nào thất bại
   - Đối với task song song [P], tiếp tục với các task thành công, báo cáo các task thất bại
   - Cung cấp thông báo lỗi rõ ràng với ngữ cảnh để debug
   - Đề xuất các bước tiếp theo nếu triển khai không thể tiếp tục
   - **QUAN TRỌNG**: Đối với các task hoàn thành, hãy đảm bảo đánh dấu task là [X] trong file tasks.

9. Xác thực hoàn thành:
   - Xác minh tất cả các task bắt buộc đã hoàn thành
   - Kiểm tra xem các tính năng đã triển khai có khớp với đặc tả ban đầu không
   - Validate rằng tests pass và độ bao phủ đáp ứng yêu cầu
   - Xác nhận triển khai tuân theo kế hoạch kỹ thuật
   - Báo cáo trạng thái cuối cùng với tóm tắt công việc đã hoàn thành

Lưu ý: Lệnh này giả định một phân rã task hoàn chỉnh tồn tại trong tasks.md. Nếu task chưa hoàn thành hoặc thiếu, đề xuất chạy `/speckit.tasks` trước để tạo lại danh sách task.
