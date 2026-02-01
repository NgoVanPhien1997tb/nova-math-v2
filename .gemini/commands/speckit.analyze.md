
---
description: Thực hiện phân tích nhất quán và chất lượng không phá hủy trên spec.md, plan.md, và tasks.md sau khi tạo task.
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Mục tiêu

Xác định các sự không nhất quán, trùng lặp, mơ hồ, và các mục chưa được chỉ định đầy đủ trên ba tạo phẩm cốt lõi (`spec.md`, `plan.md`, `tasks.md`) trước khi triển khai. Lệnh này PHẢI chỉ chạy sau khi `/speckit.tasks` đã tạo ra hoàn chỉnh `tasks.md`.

## Ràng buộc Hoạt động

**TUYỆT ĐỐI CHỈ ĐỌC (READ-ONLY)**: **Không** sửa đổi bất kỳ file nào. Xuất ra một báo cáo phân tích có cấu trúc. Cung cấp một kế hoạch khắc phục tùy chọn (người dùng phải phê duyệt rõ ràng trước khi bất kỳ lệnh chỉnh sửa tiếp theo nào được gọi thủ công).

**Thẩm quyền Hiến pháp**: Hiến pháp dự án (`.specify/memory/constitution.md`) là **không thể thương lượng** trong phạm vi phân tích này. Các xung đột Hiến pháp tự động là CRITICAL (NGHIÊM TRỌNG) và yêu cầu điều chỉnh spec, plan, hoặc tasks—chứ không phải làm loãng, diễn giải lại, hoặc lờ đi nguyên tắc. Nếu bản thân một nguyên tắc cần thay đổi, điều đó phải diễn ra trong một bản cập nhật hiến pháp riêng biệt, rõ ràng bên ngoài `/speckit.analyze`.

## Các Bước Thực thi

### 1. Khởi tạo Ngữ cảnh Phân tích

Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` một lần từ repo root và parse JSON để lấy FEATURE_DIR và AVAILABLE_DOCS. Suy ra các đường dẫn tuyệt đối:

- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md

Hủy bỏ với thông báo lỗi nếu bất kỳ file bắt buộc nào bị thiếu (hướng dẫn người dùng chạy lệnh tiên quyết còn thiếu).
Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape: ví dụ 'I'\\''m Groot' (hoặc ngoặc kép nếu có thể: "I'm Groot").

### 2. Load Tài liệu (Tiết lộ Lũy tiến - Progressive Disclosure)

Chỉ load ngữ cảnh tối thiểu cần thiết từ mỗi tài liệu:

**Từ spec.md:**

- Tổng quan/Ngữ cảnh
- Yêu cầu Chức năng
- Yêu cầu Phi Chức năng
- User Stories
- Edge Cases (nếu có)

**Từ plan.md:**

- Các lựa chọn kiến trúc/stack
- Tham chiếu Mô hình Dữ liệu
- Các Phase
- Ràng buộc kỹ thuật

**Từ tasks.md:**

- Task IDs
- Mô tả
- Nhóm Phase
- Marker song song [P]
- Đường dẫn file được tham chiếu

**Từ hiến pháp:**

- Load `.specify/memory/constitution.md` để xác thực nguyên tắc

### 3. Xây dựng Mô hình Ngữ nghĩa

Tạo các biểu diễn nội bộ (không bao gồm tài liệu thô trong output):

- **Kho Yêu cầu**: Mỗi yêu cầu chức năng + phi chức năng với một key ổn định (suy ra slug dựa trên cụm từ mệnh lệnh; ví dụ: "User can upload file" → `user-can-upload-file`)
- **Kho User story/hành động**: Các hành động người dùng rời rạc với tiêu chí chấp nhận
- **Ánh xạ bao phủ Task**: Map mỗi task vào một hoặc nhiều yêu cầu hoặc story (suy luận bằng từ khóa / các mẫu tham chiếu rõ ràng như ID hoặc cụm từ khóa)
- **Bộ quy tắc Hiến pháp**: Trích xuất tên nguyên tắc và các tuyên bố quy chuẩn MUST/SHOULD

### 4. Các Lượt Phát hiện (Phân tích Token-Hiệu quả)

Tập trung vào các phát hiện có tín hiệu cao. Giới hạn tổng cộng 50 phát hiện; gộp phần còn lại vào tóm tắt tràn (overflow summary).

#### A. Phát hiện Trùng lặp

- Xác định các yêu cầu gần như trùng lặp
- Đánh dấu cách diễn đạt chất lượng thấp hơn để hợp nhất

#### B. Phát hiện Mơ hồ

- Cắm cờ các tính từ mơ hồ (nhanh, mở rộng được, an toàn, trực quan, mạnh mẽ) thiếu tiêu chí đo lường được
- Cắm cờ các placeholder chưa được giải quyết (TODO, TKTK, ???, `<placeholder>`, v.v.)

#### C. Chưa được chỉ định đầy đủ (Underspecification)

- Các yêu cầu có động từ nhưng thiếu đối tượng hoặc kết quả đo lường được
- User stories thiếu sự liên kết tiêu chí chấp nhận
- Các task tham chiếu đến file hoặc component không được định nghĩa trong spec/plan

#### D. Liên kết Hiến pháp

- Bất kỳ yêu cầu hoặc phần tử plan nào xung đột với nguyên tắc MUST
- Thiếu các phần bắt buộc hoặc cổng chất lượng từ hiến pháp

#### E. Khoảng trống Bao phủ (Coverage Gaps)

- Các yêu cầu có 0 task liên kết nào
- Các task không map được vào yêu cầu/story nào
- Yêu cầu phi chức năng không được phản ánh trong tasks (ví dụ: hiệu năng, bảo mật)

#### F. Sự không nhất quán

- Trôi dạt thuật ngữ (cùng một khái niệm được gọi tên khác nhau giữa các file)
- Các thực thể dữ liệu được tham chiếu trong plan nhưng vắng mặt trong spec (hoặc ngược lại)
- Mâu thuẫn thứ tự task (ví dụ: task tích hợp trước task thiết lập nền tảng mà không có ghi chú phụ thuộc)
- Yêu cầu xung đột (ví dụ: một yêu cầu Next.js trong khi cái kia chỉ định Vue)

### 5. Gán Mức độ Nghiêm trọng

Sử dụng heuristic này để ưu tiên các phát hiện:

- **CRITICAL (NGHIÊM TRỌNG)**: Vi phạm MUST của hiến pháp, thiếu tài liệu spec cốt lõi, hoặc yêu cầu có độ bao phủ bằng 0 chặn chức năng cơ bản
- **HIGH (CAO)**: Yêu cầu trùng lặp hoặc xung đột, thuộc tính bảo mật/hiệu năng mơ hồ, tiêu chí chấp nhận không thể kiểm thử
- **MEDIUM (TRUNG BÌNH)**: Trôi dạt thuật ngữ, thiếu bao phủ task phi chức năng, edge case chưa được chỉ định đầy đủ
- **LOW (THẤP)**: Cải thiện phong cách/câu chữ, dư thừa nhỏ không ảnh hưởng đến thứ tự thực thi

### 6. Tạo Báo cáo Phân tích Nhỏ gọn

Xuất ra một báo cáo Markdown (không ghi file) với cấu trúc sau:

## Báo cáo Phân tích Đặc tả

| ID | Danh mục | Mức độ | Vị trí | Tóm tắt | Khuyến nghị |
|----|----------|--------|--------|---------|-------------|
| A1 | Trùng lặp | HIGH | spec.md:L120-134 | Hai yêu cầu tương tự ... | Gộp cách diễn đạt; giữ phiên bản rõ hơn |

(Thêm một hàng cho mỗi phát hiện; tạo ID ổn định có tiền tố là chữ cái đầu danh mục.)

**Bảng Tóm tắt Bao phủ:**

| Key Yêu cầu | Có Task? | Task IDs | Ghi chú |
|-------------|----------|----------|---------|

**Các Vấn đề Liên kết Hiến pháp:** (nếu có)

**Các Task Chưa được Map:** (nếu có)

**Các Chỉ số:**

- Tổng số Yêu cầu
- Tổng số Tasks
- % Bao phủ (yêu cầu có >=1 task)
- Số lượng Mơ hồ
- Số lượng Trùng lặp
- Số lượng Vấn đề Nghiêm trọng

### 7. Cung cấp Các Hành động Tiếp theo

Ở cuối báo cáo, xuất ra một khối Hành động Tiếp theo ngắn gọn:

- Nếu tồn tại vấn đề CRITICAL: Khuyến nghị giải quyết trước `/speckit.implement`
- Nếu chỉ có LOW/MEDIUM: Người dùng có thể tiếp tục, nhưng cung cấp gợi ý cải thiện
- Cung cấp gợi ý lệnh rõ ràng: ví dụ: "Chạy /speckit.specify với sự tinh chỉnh", "Chạy /speckit.plan để điều chỉnh kiến trúc", "Chỉnh sửa thủ công tasks.md để thêm bao phủ cho 'performance-metrics'"

### 8. Đề xuất Khắc phục

Hỏi người dùng: "Bạn có muốn tôi gợi ý các chỉnh sửa khắc phục cụ thể cho N vấn đề hàng đầu không?" (KHÔNG tự động áp dụng chúng.)

## Nguyên tắc Hoạt động

### Hiệu quả Ngữ cảnh

- **Token tín hiệu cao tối thiểu**: Tập trung vào các phát hiện có thể hành động, không phải tài liệu đầy đủ
- **Tiết lộ lũy tiến**: Load tài liệu tăng dần; đừng đổ tất cả nội dung vào phân tích
- **Output hiệu quả token**: Giới hạn bảng phát hiện ở 50 hàng; tóm tắt phần tràn
- **Kết quả xác định**: Chạy lại mà không thay đổi sẽ tạo ra ID và số lượng nhất quán

### Hướng dẫn Phân tích

- **KHÔNG BAO GIỜ sửa đổi file** (đây là phân tích chỉ đọc)
- **KHÔNG BAO GIỜ ảo giác các phần bị thiếu** (nếu vắng mặt, báo cáo chính xác)
- **Ưu tiên vi phạm hiến pháp** (chúng luôn là CRITICAL)
- **Sử dụng ví dụ thay vì quy tắc đầy đủ** (trích dẫn các trường hợp cụ thể, không phải mẫu chung chung)
- **Báo cáo 0 vấn đề một cách duyên dáng** (phát ra báo cáo thành công với thống kê bao phủ)

## Ngữ cảnh

{{args}}
