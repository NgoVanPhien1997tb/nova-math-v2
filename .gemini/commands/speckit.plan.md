
---
description: Thực thi quy trình lập kế hoạch triển khai sử dụng template plan để tạo các tạo phẩm thiết kế.
handoffs: 
  - label: Tạo Tasks
    agent: speckit.tasks
    prompt: Chia nhỏ kế hoạch thành các tasks
    send: true
  - label: Tạo Checklist
    agent: speckit.checklist
    prompt: Tạo checklist cho domain sau...
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Dàn ý

1. **Thiết lập**: Chạy `.specify/scripts/powershell/setup-plan.ps1 -Json` từ repo root và parse JSON cho FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape.

2. **Load ngữ cảnh**: Đọc FEATURE_SPEC và `.specify/memory/constitution.md`. Load template IMPL_PLAN (đã được copy).

3. **Thực thi quy trình plan**: Tuân theo cấu trúc trong template IMPL_PLAN để:
   - Điền Ngữ cảnh Kỹ thuật (đánh dấu những cái chưa biết là "CẦN LÀM RÕ")
   - Điền phần Kiểm tra Hiến pháp từ hiến pháp
   - Đánh giá các cổng (LỖI nếu vi phạm không chính đáng)
   - Phase 0: Tạo research.md (giải quyết tất cả CẦN LÀM RÕ)
   - Phase 1: Tạo data-model.md, contracts/, quickstart.md
   - Phase 1: Cập nhật ngữ cảnh agent bằng cách chạy script agent
   - Đánh giá lại Kiểm tra Hiến pháp sau thiết kế

4. **Dừng và báo cáo**: Lệnh kết thúc sau khi lập kế hoạch Phase 2. Báo cáo nhánh, đường dẫn IMPL_PLAN, và các tạo phẩm đã tạo.

## Các Phase

### Phase 0: Dàn bài & Nghiên cứu

1. **Trích xuất những cái chưa biết từ Ngữ cảnh Kỹ thuật** ở trên:
   - Với mỗi CẦN LÀM RÕ → task nghiên cứu
   - Với mỗi dependency → task best practices
   - Với mỗi tích hợp → task patterns

2. **Tạo và gửi các agent nghiên cứu**:

   ```text
   Cho mỗi cái chưa biết trong Ngữ cảnh Kỹ thuật:
     Task: "Nghiên cứu {unknown} cho {feature context}"
   Cho mỗi lựa chọn công nghệ:
     Task: "Tìm best practices cho {tech} trong {domain}"
   ```

3. **Hợp nhất các phát hiện** trong `research.md` sử dụng định dạng:
   - Quyết định: [cái gì đã chọn]
   - Lý do: [tại sao chọn]
   - Các lựa chọn thay thế đã xem xét: [cái gì khác đã đánh giá]

**Output**: research.md với tất cả CẦN LÀM RÕ đã được giải quyết

### Phase 1: Thiết kế & Contracts

**Điều kiện tiên quyết:** `research.md` hoàn thành

1. **Trích xuất các thực thể từ feature spec** → `data-model.md`:
   - Tên thực thể, các trường, mối quan hệ
   - Quy tắc validation từ yêu cầu
   - Chuyển đổi trạng thái nếu áp dụng

2. **Tạo thư mục contracts và copy templates**:
   ```powershell
   # Tạo thư mục contracts trong specs/[feature]/
   mkdir -p specs/[feature]/contracts
   
   # Copy templates
   cp .specify/templates/contracts-api-template.md specs/[feature]/contracts/api.md
   cp .specify/templates/contracts-models-template.md specs/[feature]/contracts/models.md
   ```

3. **Điền API contracts** (`contracts/api.md`) từ yêu cầu chức năng:
   - **Liệt kê tất cả endpoints** trong bảng tóm tắt
   - **Chi tiết từng API**:
     - Method (GET/POST/PUT/DELETE)
     - URL endpoint
     - Request headers, body, query params
     - Response success (với ví dụ JSON cụ thể)
     - Response error (các trường hợp lỗi)
   - **QUAN TRỌNG**: Backend và Frontend sẽ đọc file này để implement/gọi API

4. **Điền Data Models** (`contracts/models.md`):
   - **Entities**: Định nghĩa các bảng database
   - **DTOs**: Request/Response objects
   - **Enums**: Các giá trị cố định
   - **Mapping**: Cách chuyển đổi Entity ↔ DTO

5. **Cập nhật ngữ cảnh Agent**:
   - Chạy `.specify/scripts/powershell/update-agent-context.ps1 -AgentType gemini`
   - Các script này phát hiện AI agent nào đang được sử dụng
   - Cập nhật file ngữ cảnh đặc thù agent phù hợp
   - Chỉ thêm công nghệ mới từ plan hiện tại
   - Giữ nguyên các bổ sung thủ công giữa các marker

**Output**: data-model.md, /contracts/api.md, /contracts/models.md, quickstart.md, file đặc thù agent

**LƯU Ý QUAN TRỌNG VỀ CONTRACTS**:
- File `contracts/api.md` là **HỢP ĐỒNG** giữa Backend và Frontend
- Backend PHẢI implement API đúng theo contracts
- Frontend PHẢI gọi API đúng theo contracts
- Khi cần thay đổi API, PHẢI cập nhật contracts TRƯỚC, sau đó cả BE và FE cùng cập nhật theo

## Quy tắc chính

- Sử dụng đường dẫn tuyệt đối
- LỖI khi thất bại cổng hoặc còn các làm rõ chưa giải quyết
