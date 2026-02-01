
---
description: Tạo tasks.md có thể hành động, được sắp xếp theo phụ thuộc cho tính năng dựa trên các tạo phẩm thiết kế có sẵn.
handoffs: 
  - label: Phân tích Tính Nhất quán
    agent: speckit.analyze
    prompt: Chạy phân tích dự án cho tính nhất quán
    send: true
  - label: Triển khai Dự án
    agent: speckit.implement
    prompt: Bắt đầu triển khai theo các phase
    send: true
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Dàn ý

1. **Thiết lập**: Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json` từ repo root và parse FEATURE_DIR và danh sách AVAILABLE_DOCS. Tất cả đường dẫn phải là tuyệt đối. Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape.

2. **Load tài liệu thiết kế**: Đọc từ FEATURE_DIR:
   - **Required**: plan.md (stack, libraries, cấu trúc), spec.md (user stories với ưu tiên)
   - **Optional**: data-model.md (entities), contracts/ (endpoints), research.md (quyết định), quickstart.md (kịch bản test)
   - Lưu ý: Không phải dự án nào cũng có đủ tài liệu. Tạo task dựa trên những gì có sẵn.

3. **Thực thi quy trình tạo task**:
   - Load plan.md và trích xuất tech stack, libraries, cấu trúc dự án
   - Load spec.md và trích xuất user stories với ưu tiên (P1, P2, P3...)
   - Nếu có data-model.md: Trích xuất entities và map vào user stories
   - Nếu có contracts/: Map endpoints vào user stories
   - Nếu có research.md: Trích xuất quyết định cho các task setup
   - Tạo các task được tổ chức theo user story (xem Quy tắc Tạo Task bên dưới)
   - Tạo biểu đồ phụ thuộc hiển thị thứ tự hoàn thành user story
   - Tạo các ví dụ thực thi song song cho mỗi user story
   - Validate tính đầy đủ của task (mỗi user story có đủ task cần thiết, test được độc lập)

4. **Tạo tasks.md**: Sử dụng `.specify/templates/tasks-template.md` làm cấu trúc, điền vào:
   - Tên tính năng đúng từ plan.md
   - Phase 1: Task Setup (khởi tạo dự án)
   - Phase 2: Task Foundational (tiền quyết chặn đường cho mọi user stories)
   - Phase 3+: Một phase cho mỗi user story (theo thứ tự ưu tiên từ spec.md)
   - Mỗi phase bao gồm: mục tiêu story, tiêu chí test độc lập, tests (nếu được yêu cầu), tasks triển khai
   - Final Phase: Đánh bóng & các mối quan tâm chung
   - Tất cả các task phải tuân theo định dạng checklist nghiêm ngặt (xem Quy tắc)
   - Đường dẫn file rõ ràng cho mỗi task
   - Phần Dependencies hiển thị thứ tự hoàn thành story
   - Ví dụ thực thi song song mỗi story
   - Phần Chiến lược triển khai (MVP trước, giao hàng tăng dần)

5. **Báo cáo**: Xuất đường dẫn tới tasks.md đã tạo và tóm tắt:
   - Tổng số task
   - Số task mỗi user story
   - Các cơ hội song song được xác định
   - Tiêu chí test độc lập cho mỗi story
   - Phạm vi MVP đề xuất (thường chỉ User Story 1)
   - Validate format: Xác nhận TẤT CẢ các task tuân theo định dạng checklist (checkbox, ID, nhãn, đường dẫn file)

Ngữ cảnh cho tạo task: {{args}}

tasks.md nên có khả năng thực thi ngay lập tức - mỗi task phải đủ cụ thể để LLM có thể hoàn thành mà không cần thêm ngữ cảnh.

## Quy tắc Tạo Task

**QUAN TRỌNG**: Các task PHẢI được tổ chức theo user story để cho phép triển khai và kiểm thử độc lập.

**Tests là TÙY CHỌN**: Chỉ tạo các task test nếu được yêu cầu rõ ràng trong đặc tả tính năng hoặc nếu người dùng yêu cầu tiếp cận TDD.

### Định dạng Checklist (BẮT BUỘC)

Mỗi task PHẢI tuân thủ nghiêm ngặt định dạng này:

```text
- [ ] [TaskID] [P?] [Story?] Mô tả với đường dẫn file
```

**Các thành phần Format**:

1. **Checkbox**: LUÔN bắt đầu với `- [ ]`
2. **Task ID**: Số thứ tự (T001, T002...) theo thứ tự thực thi
3. **[P] marker**: CHỈ bao gồm nếu task có thể song song hóa (file khác nhau, không phụ thuộc task chưa hoàn thành)
4. **[Story] label**: BẮT BUỘC cho các task phase user story
   - Format: [US1], [US2], [US3]... (map tới user stories từ spec.md)
   - Setup phase: KHÔNG có nhãn story
   - Foundational phase: KHÔNG có nhãn story
   - User Story phases: PHẢI có nhãn story
   - Polish phase: KHÔNG có nhãn story
5. **Mô tả**: Hành động rõ ràng với đường dẫn file chính xác

### Tổ chức Task

1. **Từ User Stories (spec.md)** - TỔ CHỨC CHÍNH:
   - Mỗi user story (P1, P2...) có phase riêng
   - Map tất cả component liên quan vào story của chúng: Models, Services, Endpoints/UI, Tests (nếu có)
   - Đánh dấu phụ thuộc story

2. **Từ Contracts**:
   - Map mỗi contract/endpoint → tới user story nó phục vụ
   - Nếu tests được yêu cầu: Mỗi contract → contract test task [P] trước khi triển khai

3. **Từ Data Model**:
   - Map mỗi entity tới user story cần nó
   - Nếu entity phục vụ nhiều story: Đặt vào story sớm nhất hoặc Setup phase
   - Mối quan hệ → task service layer trong phase story phù hợp

4. **Từ Setup/Infrastructure**:
   - Hạ tầng chia sẻ → Setup phase (Phase 1)
   - Task nền tảng/chặn → Foundational phase (Phase 2)
   - Setup đặc thù story → trong phase của story đó

### Cấu trúc Phase

- **Phase 1**: Setup
- **Phase 2**: Foundational (BẮT BUỘC hoàn thành trước user stories)
- **Phase 3+**: User Stories theo thứ tự ưu tiên
- **Final Phase**: Polish & Cross-Cutting Concerns
