---

description: "Mẫu danh sách nhiệm vụ cho việc triển khai tính năng"
---

# Nhiệm vụ (Tasks): [FEATURE NAME]

**Đầu vào**: Tài liệu thiết kế từ `/specs/[###-feature-name]/`
**Điều kiện tiên quyết**: plan.md (bắt buộc), spec.md (bắt buộc cho user stories), research.md, data-model.md, contracts/

**Tests**: Các ví dụ bên dưới bao gồm các task test. Test là TÙY CHỌN - chỉ bao gồm chúng nếu được yêu cầu rõ ràng trong đặc tả tính năng.

**Tổ chức**: Các nhiệm vụ được nhóm theo user story để cho phép triển khai và kiểm thử độc lập từng story.

## Format: `[ID] [P?] [Story] Mô tả`

- **[P]**: Có thể chạy song song (các file khác nhau, không phụ thuộc)
- **[Story]**: Nhiệm vụ này thuộc về user story nào (ví dụ: US1, US2, US3)
- Bao gồm đường dẫn file chính xác trong mô tả

## Quy ước Đường dẫn

- **Dự án đơn (Single project)**: `src/`, `tests/` tại gốc repository
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` hoặc `android/src/`
- Các đường dẫn hiển thị bên dưới giả định dự án đơn - hãy điều chỉnh dựa trên cấu trúc trong plan.md

<!-- 
  ============================================================================
  QUAN TRỌNG: Các task dưới đây là TASK MẪU chỉ cho mục đích minh họa.
  
  Lệnh /speckit.tasks PHẢI thay thế chúng bằng các task thực tế dựa trên:
  - User stories từ spec.md (với độ ưu tiên P1, P2, P3...)
  - Các yêu cầu tính năng từ plan.md
  - Các thực thể từ data-model.md
  - Các endpoint từ contracts/
  
  Các task PHẢI được tổ chức theo user story để mỗi story có thể:
  - Được triển khai độc lập
  - Được kiểm thử độc lập
  - Được bàn giao như một phần MVP (Minimum Viable Product increment)
  
  KHÔNG giữ lại các task mẫu này trong file tasks.md được tạo ra.
  ============================================================================
-->

## Phase 1: Thiết lập (Cơ sở hạ tầng chia sẻ)

**Mục đích**: Khởi tạo dự án và cấu trúc cơ bản

- [ ] T001 Tạo cấu trúc dự án theo kế hoạch triển khai
- [ ] T002 Khởi tạo dự án [ngôn ngữ] với các dependencies [framework]
- [ ] T003 [P] Cấu hình các công cụ linting và formatting

---

## Phase 2: Nền tảng (Điều kiện tiên quyết chặn đường)

**Mục đích**: Cơ sở hạ tầng cốt lõi PHẢI hoàn thành trước khi BẤT KỲ user story nào có thể được triển khai

**⚠️ QUAN TRỌNG**: Không có công việc user story nào có thể bắt đầu cho đến khi phase này hoàn thành

Ví dụ về các task nền tảng (điều chỉnh dựa trên dự án của bạn):

- [ ] T004 Thiết lập schema cơ sở dữ liệu và framework migrations
- [ ] T005 [P] Triển khai framework xác thực/phân quyền (authentication/authorization)
- [ ] T006 [P] Thiết lập cấu trúc API routing và middleware
- [ ] T007 Tạo các base models/entities mà tất cả các story phụ thuộc vào
- [ ] T008 Cấu hình cơ sở hạ tầng xử lý lỗi (error handling) và ghi log (logging)
- [ ] T009 Thiết lập quản lý cấu hình môi trường

**Điểm kiểm tra (Checkpoint)**: Nền tảng đã sẵn sàng - việc triển khai user story bây giờ có thể bắt đầu song song

---

## Phase 3: User Story 1 - [Tiêu đề] (Ưu tiên: P1) 🎯 MVP

**Mục tiêu**: [Mô tả ngắn gọn story này mang lại cái gì]

**Test Độc lập**: [Cách xác minh story này hoạt động độc lập]

### Tests cho User Story 1 (TÙY CHỌN - chỉ nếu tests được yêu cầu) ⚠️

> **LƯU Ý: Viết các test này TRƯỚC, đảm bảo chúng FAIL trước khi implementation**

- [ ] T010 [P] [US1] Contract test cho [endpoint] trong tests/contract/test_[name].py
- [ ] T011 [P] [US1] Integration test cho [hành trình người dùng] trong tests/integration/test_[name].py

### Triển khai (Implementation) cho User Story 1

- [ ] T012 [P] [US1] Tạo model [Entity1] trong src/models/[entity1].py
- [ ] T013 [P] [US1] Tạo model [Entity2] trong src/models/[entity2].py
- [ ] T014 [US1] Triển khai [Service] trong src/services/[service].py (phụ thuộc vào T012, T013)
- [ ] T015 [US1] Triển khai [endpoint/tính năng] trong src/[location]/[file].py
- [ ] T016 [US1] Thêm validation và xử lý lỗi
- [ ] T017 [US1] Thêm logging cho các hoạt động của user story 1

**Điểm kiểm tra (Checkpoint)**: Tại điểm này, User Story 1 nên hoạt động đầy đủ và có thể test độc lập

---

## Phase 4: User Story 2 - [Tiêu đề] (Ưu tiên: P2)

**Mục tiêu**: [Mô tả ngắn gọn story này mang lại cái gì]

**Test Độc lập**: [Cách xác minh story này hoạt động độc lập]

### Tests cho User Story 2 (TÙY CHỌN - chỉ nếu tests được yêu cầu) ⚠️

- [ ] T018 [P] [US2] Contract test cho [endpoint] trong tests/contract/test_[name].py
- [ ] T019 [P] [US2] Integration test cho [hành trình người dùng] trong tests/integration/test_[name].py

### Triển khai (Implementation) cho User Story 2

- [ ] T020 [P] [US2] Tạo model [Entity] trong src/models/[entity].py
- [ ] T021 [US2] Triển khai [Service] trong src/services/[service].py
- [ ] T022 [US2] Triển khai [endpoint/tính năng] trong src/[location]/[file].py
- [ ] T023 [US2] Tích hợp với các thành phần User Story 1 (nếu cần)

**Điểm kiểm tra (Checkpoint)**: Tại điểm này, CẢ User Story 1 VÀ 2 đều nên hoạt động độc lập

---

## Phase 5: User Story 3 - [Tiêu đề] (Ưu tiên: P3)

**Mục tiêu**: [Mô tả ngắn gọn story này mang lại cái gì]

**Test Độc lập**: [Cách xác minh story này hoạt động độc lập]

### Tests cho User Story 3 (TÙY CHỌN - chỉ nếu tests được yêu cầu) ⚠️

- [ ] T024 [P] [US3] Contract test cho [endpoint] trong tests/contract/test_[name].py
- [ ] T025 [P] [US3] Integration test cho [hành trình người dùng] trong tests/integration/test_[name].py

### Triển khai (Implementation) cho User Story 3

- [ ] T026 [P] [US3] Tạo model [Entity] trong src/models/[entity].py
- [ ] T027 [US3] Triển khai [Service] trong src/services/[service].py
- [ ] T028 [US3] Triển khai [endpoint/tính năng] trong src/[location]/[file].py

**Điểm kiểm tra (Checkpoint)**: Tất cả các user story bây giờ nên hoạt động độc lập

---

[Thêm các phase user story khác nếu cần, theo cùng một mẫu]

---

## Phase N: Đánh bóng & Các mối quan tâm chung (Cross-Cutting Concerns)

**Mục đích**: Các cải tiến ảnh hưởng đến nhiều user story

- [ ] TXXX [P] Cập nhật tài liệu trong docs/
- [ ] TXXX Dọn dẹp code và refactoring
- [ ] TXXX Tối ưu hóa hiệu năng trên tất cả các story
- [ ] TXXX [P] Các unit test bổ sung (nếu được yêu cầu) trong tests/unit/
- [ ] TXXX Tăng cường bảo mật (Security hardening)
- [ ] TXXX Chạy validation quickstart.md

---

## Các sự phụ thuộc & Thứ tự thực thi

### Sự phụ thuộc giữa các Phase

- **Thiết lập (Phase 1)**: Không có phụ thuộc - có thể bắt đầu ngay lập tức
- **Nền tảng (Phase 2)**: Phụ thuộc vào hoàn thành Thiết lập - CHẶN (BLOCKS) tất cả các user story
- **User Stories (Phase 3+)**: Tất cả đều phụ thuộc vào hoàn thành phase Nền tảng
  - Các user story sau đó có thể tiến hành song song (nếu có nhân sự)
  - Hoặc tuần tự theo thứ tự ưu tiên (P1 → P2 → P3)
- **Đánh bóng (Phase cuối)**: Phụ thuộc vào việc hoàn thành tất cả các user story mong muốn

### Sự phụ thuộc của User Story

- **User Story 1 (P1)**: Có thể bắt đầu sau Nền tảng (Phase 2) - Không phụ thuộc vào các story khác
- **User Story 2 (P2)**: Có thể bắt đầu sau Nền tảng (Phase 2) - Có thể tích hợp với US1 nhưng nên test được độc lập
- **User Story 3 (P3)**: Có thể bắt đầu sau Nền tảng (Phase 2) - Có thể tích hợp với US1/US2 nhưng nên test được độc lập

### Trong mỗi User Story

- Tests (nếu bao gồm) PHẢI được viết và FAIL trước khi implement
- Models trước services
- Services trước endpoints
- Implementation cốt lõi trước integration
- Story hoàn thành trước khi chuyển sang ưu tiên tiếp theo

### Cơ hội Song song (Parallel Opportunities)

- Tất cả các task Thiết lập được đánh dấu [P] có thể chạy song song
- Tất cả các task Nền tảng được đánh dấu [P] có thể chạy song song (trong Phase 2)
- Khi phase Nền tảng hoàn thành, tất cả các user story có thể bắt đầu song song (nếu năng lực team cho phép)
- Tất cả các test cho một user story được đánh dấu [P] có thể chạy song song
- Các models trong một story được đánh dấu [P] có thể chạy song song
- Các user story khác nhau có thể được thực hiện song song bởi các thành viên team khác nhau

---

## Ví dụ Song song: User Story 1

```bash
# Chạy tất cả các test cho User Story 1 cùng nhau (nếu test được yêu cầu):
Task: "Contract test for [endpoint] in tests/contract/test_[name].py"
Task: "Integration test for [user journey] in tests/integration/test_[name].py"

# Chạy tất cả các models cho User Story 1 cùng nhau:
Task: "Create [Entity1] model in src/models/[entity1].py"
Task: "Create [Entity2] model in src/models/[entity2].py"
```

---

## Chiến lược Triển khai

### MVP Trước (Chỉ User Story 1)

1. Hoàn thành Phase 1: Thiết lập
2. Hoàn thành Phase 2: Nền tảng (QUAN TRỌNG - chặn tất cả các story)
3. Hoàn thành Phase 3: User Story 1
4. **DỪNG và VALIDATE**: Test User Story 1 độc lập
5. Deploy/demo nếu sẵn sàng

### Giao hàng Tăng dần (Incremental Delivery)

1. Hoàn thành Thiết lập + Nền tảng → Nền tảng sẵn sàng
2. Thêm User Story 1 → Test độc lập → Deploy/Demo (MVP!)
3. Thêm User Story 2 → Test độc lập → Deploy/Demo
4. Thêm User Story 3 → Test độc lập → Deploy/Demo
5. Mỗi story thêm giá trị mà không phá vỡ các story trước đó

### Chiến lược Team Song song

Với nhiều lập trình viên:

1. Team hoàn thành Thiết lập + Nền tảng cùng nhau
2. Khi Nền tảng xong:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
3. Các story hoàn thành và tích hợp độc lập

---

## Ghi chú

- [P] tasks = các file khác nhau, không phụ thuộc
- [Story] nhãn map task vào user story cụ thể để truy xuất nguồn gốc
- Mỗi user story nên có thể hoàn thành và test độc lập
- Xác minh tests fail trước khi implement
- Commit sau mỗi task hoặc nhóm logic
- Dừng tại bất kỳ checkpoint nào để validate story độc lập
- Tránh: các task mơ hồ, xung đột cùng file, phụ thuộc chéo giữa các story làm phá vỡ tính độc lập
