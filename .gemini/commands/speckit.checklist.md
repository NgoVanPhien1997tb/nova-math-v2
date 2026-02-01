
---
description: Tạo checklist tùy chỉnh cho tính năng hiện tại dựa trên yêu cầu của người dùng.
---

## Mục đích Checklist: "Unit Tests cho Tiếng Anh"

**KHÁI NIỆM QUAN TRỌNG**: Checklists là **UNIT TESTS CHO VIỆC VIẾT YÊU CẦU** - chúng xác thực chất lượng, sự rõ ràng, và tính đầy đủ của các yêu cầu trong một domain nhất định.

**KHÔNG dùng cho xác minh/kiểm thử (verification/testing)**:

- ❌ KHÔNG PHẢI "Xác minh nút bấm click chính xác"
- ❌ KHÔNG PHẢI "Test xử lý lỗi hoạt động"
- ❌ KHÔNG PHẢI "Xác nhận API trả về 200"
- ❌ KHÔNG PHẢI kiểm tra xem code/implementation có khớp với spec không

**DÙNG CHO xác thực chất lượng yêu cầu (requirements quality validation)**:

- ✅ "Các yêu cầu phân cấp thị giác có được định nghĩa cho tất cả các loại thẻ không?" (tính đầy đủ)
- ✅ "Việc 'hiển thị nổi bật' có được định lượng với kích thước/vị trí cụ thể không?" (sự rõ ràng)
- ✅ "Các yêu cầu trạng thái hover có nhất quán trên tất cả các phần tử tương tác không?" (tính nhất quán)
- ✅ "Các yêu cầu khả năng truy cập (accessibility) có được định nghĩa cho điều hướng bàn phím không?" (độ bao phủ)
- ✅ "Spec có định nghĩa điều gì xảy ra khi hình ảnh logo không load được không?" (edge cases)

**Ẩn dụ**: Nếu spec của bạn là code được viết bằng Tiếng Anh, thì checklist là bộ unit test suite của nó. Bạn đang test xem các yêu cầu có được viết tốt, đầy đủ, không mơ hồ, và sẵn sàng để triển khai hay không - KHÔNG PHẢI implementation có hoạt động hay không.

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Các Bước Thực thi

1. **Thiết lập**: Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json` từ repo root và parse JSON lấy FEATURE_DIR và danh sách AVAILABLE_DOCS.
   - Tất cả các đường dẫn file phải là tuyệt đối.
   - Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape: ví dụ 'I'\\''m Groot' (hoặc ngoặc kép nếu có thể: "I'm Groot").

2. **Làm rõ mục đích (động)**: Suy ra tối đa BA câu hỏi làm rõ ngữ cảnh ban đầu (không dùng danh mục có sẵn). Chúng PHẢI:
   - Được tạo từ cách diễn đạt của người dùng + các tín hiệu trích xuất từ spec/plan/tasks
   - Chỉ hỏi về thông tin làm thay đổi nội dung checklist một cách đáng kể
   - Được bỏ qua riêng lẻ nếu đã rõ ràng trong `$ARGUMENTS`
   - Ưu tiên sự chính xác hơn độ rộng

   Thuật toán tạo:
   1. Trích xuất tín hiệu: từ khóa domain tính năng (ví dụ: auth, latency, UX, API), chỉ báo rủi ro ("critical", "must", "compliance"), gợi ý bên liên quan ("QA", "review", "security team"), và các sản phẩm bàn giao rõ ràng ("a11y", "rollback", "contracts").
   2. Gom cụm các tín hiệu thành các lĩnh vực trọng tâm ứng viên (tối đa 4) xếp hạng theo mức độ liên quan.
   3. Xác định đối tượng & thời điểm có thể (tác giả, người review, QA, phát hành) nếu không rõ ràng.
   4. Phát hiện các chiều còn thiếu: độ rộng phạm vi, độ sâu/nghiêm ngặt, nhấn mạnh rủi ro, ranh giới loại trừ, tiêu chí chấp nhận đo lường được.
   5. Xây dựng câu hỏi chọn từ các nguyên mẫu này:
      - Tinh chỉnh phạm vi (ví dụ: "Cái này có nên bao gồm các điểm chạm tích hợp với X và Y hay giữ giới hạn ở tính đúng đắn của module cục bộ?")
      - Ưu tiên rủi ro (ví dụ: "Khu vực rủi ro tiềm ẩn nào trong số này nên nhận được các kiểm tra gating bắt buộc?")
      - Căn chỉnh độ sâu (ví dụ: "Đây là danh sách kiểm tra nhanh trước khi commit hay một cổng phát hành chính thức?")
      - Khung đối tượng (ví dụ: "Cái này sẽ được sử dụng bởi chỉ tác giả hay đồng nghiệp trong quá trình PR review?")
      - Loại trừ ranh giới (ví dụ: "Chúng ta có nên loại trừ rõ ràng các mục tinh chỉnh hiệu năng trong vòng này không?")
      - Khoảng trống lớp kịch bản (ví dụ: "Không phát hiện luồng phục hồi—các đường dẫn rollback / lỗi một phần có nằm trong phạm vi không?")

   Quy tắc định dạng câu hỏi:
   - Nếu trình bày các lựa chọn, tạo một bảng nhỏ gọn với các cột: Tùy chọn | Ứng viên | Tại sao nó quan trọng
   - Giới hạn tối đa A–E tùy chọn; bỏ bảng nếu câu trả lời tự do rõ ràng hơn
   - Không bao giờ yêu cầu người dùng nhắc lại những gì họ đã nói
   - Tránh các danh mục suy đoán (không ảo giác). Nếu không chắc chắn, hỏi rõ ràng: "Xác nhận xem X có thuộc phạm vi không."

   Mặc định khi tương tác không thể thực hiện:
   - Độ sâu: Tiêu chuẩn
   - Đối tượng: Người review (PR) nếu liên quan đến code; Tác giả nếu khác
   - Trọng tâm: Top 2 cụm liên quan nhất

   Xuất ra các câu hỏi (nhãn Q1/Q2/Q3). Sau câu trả lời: nếu ≥2 lớp kịch bản (Alternate / Exception / Recovery / Non-Functional domain) vẫn không rõ ràng, bạn CÓ THỂ hỏi thêm tối đa HAI câu hỏi tiếp theo có mục tiêu (Q4/Q5) với lý do một dòng mỗi câu (ví dụ: "Rủi ro đường dẫn phục hồi chưa được giải quyết"). Không vượt quá tổng cộng năm câu hỏi. Bỏ qua leo thang nếu người dùng từ chối thêm rõ ràng.

3. **Hiểu yêu cầu người dùng**: Kết hợp `$ARGUMENTS` + câu trả lời làm rõ:
   - Suy ra chủ đề checklist (ví dụ: security, review, deploy, ux)
   - Hợp nhất các mục must-have rõ ràng được đề cập bởi người dùng
   - Map các lựa chọn trọng tâm vào khung danh mục
   - Suy luận bất kỳ ngữ cảnh nào còn thiếu từ spec/plan/tasks (KHÔNG ảo giác)

4. **Load ngữ cảnh tính năng**: Đọc từ FEATURE_DIR:
   - spec.md: Yêu cầu tính năng và phạm vi
   - plan.md (nếu có): Chi tiết kỹ thuật, dependencies
   - tasks.md (nếu có): Các tasks triển khai

   **Chiến lược Load Ngữ cảnh**:
   - Chỉ load các phần cần thiết liên quan đến các lĩnh vực trọng tâm đang hoạt động (tránh dump toàn bộ file)
   - Ưu tiên tóm tắt các phần dài thành các gạch đầu dòng kịch bản/yêu cầu ngắn gọn
   - Sử dụng tiết lộ lũy tiến: thêm truy xuất tiếp theo chỉ khi phát hiện khoảng trống
   - Nếu tài liệu nguồn lớn, tạo các mục tóm tắt tạm thời thay vì nhúng văn bản thô

5. **Tạo checklist** - Tạo "Unit Tests cho Yêu cầu":
   - Tạo thư mục `FEATURE_DIR/checklists/` nếu chưa tồn tại
   - Tạo tên file checklist duy nhất:
     - Sử dụng tên ngắn, mô tả dựa trên domain (ví dụ: `ux.md`, `api.md`, `security.md`)
     - Định dạng: `[domain].md`
     - Nếu file tồn tại, nối thêm vào file hiện có
   - Đánh số các mục tuần tự bắt đầu từ CHK001
   - Mỗi lần chạy `/speckit.checklist` tạo một file MỚI (không bao giờ ghi đè checklist hiện có)

   **NGUYÊN TẮC CỐT LÕI - Test Yêu cầu, Không phải Test Implementation**:
   Mỗi mục checklist PHẢI đánh giá CHÍNH CÁC YÊU CẦU về:
   - **Tính đầy đủ (Completeness)**: Tất cả các yêu cầu cần thiết có hiện diện không?
   - **Sự rõ ràng (Clarity)**: Các yêu cầu có không mơ hồ và cụ thể không?
   - **Tính nhất quán (Consistency)**: Các yêu cầu có phù hợp với nhau không?
   - **Khả năng đo lường (Measurability)**: Các yêu cầu có thể được xác minh khách quan không?
   - **Độ bao phủ (Coverage)**: Tất cả các kịch bản/edge cases có được giải quyết không?

   **Cấu trúc Danh mục** - Nhóm các mục theo các chiều chất lượng yêu cầu:
   - **Tính Đầy đủ Yêu cầu** (Tất cả các yêu cầu cần thiết có được ghi lại không?)
   - **Sự Rõ ràng Yêu cầu** (Các yêu cầu có cụ thể và không mơ hồ không?)
   - **Tính Nhất quán Yêu cầu** (Các yêu cầu có căn chỉnh mà không xung đột không?)
   - **Chất lượng Tiêu chí Chấp nhận** (Các tiêu chí thành công có đo lường được không?)
   - **Bao phủ Kịch bản** (Tất cả các luồng/trường hợp có được giải quyết không?)
   - **Bao phủ Edge Case** (Các điều kiện biên có được định nghĩa không?)
   - **Yêu cầu Phi Chức năng** (Hiệu năng, Bảo mật, Accessibility, t.v.v - chúng có được chỉ định không?)
   - **Dependencies & Giả định** (Chúng có được ghi lại và xác thực không?)
   - **Mơ hồ & Xung đột** (Cái gì cần làm rõ?)

   **CÁCH VIẾT MỤC CHECKLIST - "Unit Tests cho Tiếng Anh"**:

   ❌ **SAI** (Test implementation):
   - "Xác minh trang landing hiển thị 3 thẻ tập phim"
   - "Test trạng thái hover hoạt động trên desktop"
   - "Xác nhận click logo điều hướng về home"

   ✅ **ĐÚNG** (Test chất lượng yêu cầu):
   - "Số lượng và bố cục chính xác của các tập phim nổi bật có được chỉ định không?" [Completeness]
   - "Việc 'hiển thị nổi bật' có được định lượng với kích thước/vị trí cụ thể không?" [Clarity]
   - "Các yêu cầu trạng thái hover có nhất quán trên tất cả các phần tử tương tác không?" [Consistency]
   - "Các yêu cầu điều hướng bàn phím có được định nghĩa cho tất cả UI tương tác không?" [Coverage]
   - "Hành vi fallback có được chỉ định khi hình ảnh logo không load được không?" [Edge Cases]
   - "Các trạng thái loading có được định nghĩa cho dữ liệu tập phim bất đồng bộ không?" [Completeness]
   - "Spec có định nghĩa phân cấp thị giác cho các phần tử UI cạnh tranh không?" [Clarity]

   **CẤU TRÚC MỤC**:
   Mỗi mục nên theo mẫu này:
   - Định dạng câu hỏi hỏi về chất lượng yêu cầu
   - Tập trung vào những gì ĐÃ VIẾT (hoặc không được viết) trong spec/plan
   - Bao gồm chiều chất lượng trong ngoặc [Completeness/Clarity/Consistency/vv.]
   - Tham chiếu phần spec `[Spec §X.Y]` khi kiểm tra các yêu cầu hiện có
   - Sử dụng marker `[Gap]` khi kiểm tra các yêu cầu bị thiếu

   **VÍ DỤ THEO CHIỀU CHẤT LƯỢNG**:

   Completeness:
   - "Các yêu cầu xử lý lỗi có được định nghĩa cho tất cả các chế độ lỗi API không? [Gap]"
   - "Các yêu cầu accessibility có được chỉ định cho tất cả các phần tử tương tác không? [Completeness]"
   - "Các yêu cầu breakpoint mobile có được định nghĩa cho layout phản hồi không? [Gap]"

   Clarity:
   - "Việc 'load nhanh' có được định lượng với các ngưỡng thời gian cụ thể không? [Clarity, Spec §NFR-2]"
   - "Tiêu chí lựa chọn 'tập phim liên quan' có được định nghĩa rõ ràng không? [Clarity, Spec §FR-5]"
   - "'Nổi bật' có được định nghĩa với các thuộc tính hình ảnh đo lường được không? [Ambiguity, Spec §FR-4]"

   Consistency:
   - "Các yêu cầu điều hướng có căn chỉnh trên tất cả các trang không? [Consistency, Spec §FR-10]"
   - "Các yêu cầu component thẻ có nhất quán giữa trang landing và trang chi tiết không? [Consistency]"

   Coverage:
   - "Các yêu cầu có được định nghĩa cho kịch bản zero-state (không có tập phim) không? [Coverage, Edge Case]"
   - "Các kịch bản tương tác người dùng đồng thời có được giải quyết không? [Coverage, Gap]"
   - "Các yêu cầu có được chỉ định cho lỗi load dữ liệu một phần không? [Coverage, Exception Flow]"

   Measurability:
   - "Các yêu cầu phân cấp thị giác có thể đo lường/kiểm thử được không? [Acceptance Criteria, Spec §FR-1]"
   - "'Trọng lượng hình ảnh cân bằng' có thể được xác minh khách quan không? [Measurability, Spec §FR-2]"

   **Phân loại & Bao phủ Kịch bản** (Trọng tâm Chất lượng Yêu cầu):
   - Kiểm tra xem yêu cầu có tồn tại cho: Chính (Primary), Thay thế (Alternate), Ngoại lệ/Lỗi (Exception/Error), Phục hồi (Recovery), Kịch bản Phi chức năng
   - Đối với mỗi lớp kịch bản, hỏi: "Các yêu cầu [loại kịch bản] có đầy đủ, rõ ràng và nhất quán không?"
   - Nếu lớp kịch bản bị thiếu: "Các yêu cầu [loại kịch bản] có bị cố ý loại trừ hay bị thiếu không? [Gap]"
   - Bao gồm khả năng phục hồi/rollback khi xảy ra thay đổi trạng thái: "Các yêu cầu rollback có được định nghĩa cho lỗi di chuyển không? [Gap]"

   **Yêu cầu Truy xuất nguồn gốc**:
   - TỐI THIỂU: ≥80% các mục PHẢI bao gồm ít nhất một tham chiếu truy xuất nguồn gốc
   - Mỗi mục nên tham chiếu: phần spec `[Spec §X.Y]`, hoặc sử dụng marker: `[Gap]`, `[Ambiguity]`, `[Conflict]`, `[Assumption]`
   - Nếu không có hệ thống ID: "Một sơ đồ ID yêu cầu & tiêu chí chấp nhận có được thiết lập không? [Traceability]"

   **Làm nổi bật & Giải quyết Vấn đề** (Các Vấn đề Chất lượng Yêu cầu):
   Hỏi các câu hỏi về chính các yêu cầu:
   - Mơ hồ: "Thuật ngữ 'nhanh' có được định lượng với các chỉ số cụ thể không? [Ambiguity, Spec §NFR-1]"
   - Xung đột: "Các yêu cầu điều hướng có xung đột giữa §FR-10 và §FR-10a không? [Conflict]"
   - Giả định: "Giả định về 'podcast API luôn sẵn sàng' có được xác thực không? [Assumption]"
   - Dependencies: "Các yêu cầu external podcast API có được ghi lại không? [Dependency, Gap]"
   - Các định nghĩa thiếu: "'Phân cấp thị giác' có được định nghĩa với tiêu chí đo lường được không? [Gap]"

   **Hợp nhất Nội dung**:
   - Soft cap: Nếu các mục ứng viên thô > 40, ưu tiên theo rủi ro/tác động
   - Gộp các bản sao gần giống nhau kiểm tra cùng một khía cạnh yêu cầu
   - Nếu >5 edge cases tác động thấp, tạo một mục: "Các edge cases X, Y, Z có được giải quyết trong yêu cầu không? [Coverage]"

   **🚫 TUYỆT ĐỐI BỊ CẤM** - Những cái này biến nó thành test implementation, không phải test yêu cầu:
   - ❌ Bất kỳ mục nào bắt đầu bằng "Verify", "Test", "Confirm", "Check" + hành vi thực hiện
   - ❌ Tham chiếu đến thực thi code, hành động người dùng, hành vi hệ thống
   - ❌ "Hiển thị đúng", "hoạt động đúng", "hoạt động như mong đợi"
   - ❌ "Click", "navigate", "render", "load", "execute"
   - ❌ Test cases, test plans, quy trình QA
   - ❌ Chi tiết triển khai (frameworks, APIs, thuật toán)

   **✅ MẪU YÊU CẦU** - Những cái này test chất lượng yêu cầu:
   - ✅ "[Loại yêu cầu] có được định nghĩa/chỉ định/ghi lại cho [kịch bản] không?"
   - ✅ "[Thuật ngữ mơ hồ] có được định lượng/làm rõ với các tiêu chí cụ thể không?"
   - ✅ "Các yêu cầu có nhất quán giữa [phần A] và [phần B] không?"
   - ✅ "[Yêu cầu] có thể được đo lường/xác minh khách quan không?"
   - ✅ "[Edge cases/kịch bản] có được giải quyết trong yêu cầu không?"
   - ✅ "Spec có định nghĩa [khía cạnh bị thiếu] không?"

6. **Tham chiếu Cấu trúc**: Tạo checklist theo mẫu chuẩn trong `.specify/templates/checklist-template.md` cho tiêu đề, phần meta, tiêu đề danh mục, và định dạng ID. Nếu template không có sẵn, sử dụng: Tiêu đề H1, các dòng meta purpose/created, các phần danh mục `##` chứa các dòng `- [ ] CHK### <mục yêu cầu>` với ID tăng dần toàn cục bắt đầu từ CHK001.

7. **Báo cáo**: Xuất đường dẫn đầy đủ đến checklist đã tạo, số lượng mục, và nhắc người dùng rằng mỗi lần chạy tạo một file mới. Tóm tắt:
   - Các lĩnh vực trọng tâm đã chọn
   - Mức độ sâu
   - Tác nhân/thời điểm
   - Bất kỳ mục must-have nào người dùng chỉ định rõ ràng đã được kết hợp

**Quan trọng**: Mỗi lệnh gọi `/speckit.checklist` tạo một file checklist sử dụng tên ngắn, mô tả trừ khi file đã tồn tại. Điều này cho phép:

- Nhiều checklist thuộc các loại khác nhau (ví dụ: `ux.md`, `test.md`, `security.md`)
- Tên file đơn giản, dễ nhớ chỉ ra mục đích checklist
- Dễ dàng xác định và điều hướng trong thư mục `checklists/`

Để tránh lộn xộn, sử dụng các loại mô tả và dọn dẹp các checklist lỗi thời khi xong.

## Ví dụ Checklist Types & Sample Items

**Chất lượng Yêu cầu UX:** `ux.md`

Sample items (test các yêu cầu, KHÔNG test implementation):

- "Các yêu cầu phân cấp thị giác có được định nghĩa với tiêu chí đo lường được không? [Clarity, Spec §FR-1]"
- "Số lượng và vị trí của các phần tử UI có được chỉ định rõ ràng không? [Completeness, Spec §FR-1]"
- "Các yêu cầu trạng thái tương tác (hover, focus, active) có được định nghĩa nhất quán không? [Consistency]"
- "Các yêu cầu accessibility có được chỉ định cho tất cả các phần tử tương tác không? [Coverage, Gap]"
- "Hành vi fallback có được định nghĩa khi hình ảnh không load được không? [Edge Case, Gap]"
- "'Hiển thị nổi bật' có thể được đo lường khách quan không? [Measurability, Spec §FR-4]"

**Chất lượng Yêu cầu API:** `api.md`

Sample items:

- "Các định dạng phản hồi lỗi có được chỉ định cho tất cả các kịch bản thất bại không? [Completeness]"
- "Các yêu cầu giới hạn tốc độ (rate limiting) có được định lượng với các ngưỡng cụ thể không? [Clarity]"
- "Các yêu cầu xác thực có nhất quán trên tất cả các endpoint không? [Consistency]"
- "Các yêu cầu retry/timeout có được định nghĩa cho các dependencies bên ngoài không? [Coverage, Gap]"
- "Chiến lược phiên bản hóa có được ghi lại trong yêu cầu không? [Gap]"

**Chất lượng Yêu cầu Hiệu năng:** `performance.md`

Sample items:

- "Các yêu cầu hiệu năng có được định lượng với các chỉ số cụ thể không? [Clarity]"
- "Các mục tiêu hiệu năng có được định nghĩa cho tất cả các hành trình người dùng quan trọng không? [Coverage]"
- "Các yêu cầu hiệu năng dưới các điều kiện tải khác nhau có được chỉ định không? [Completeness]"
- "Các yêu cầu hiệu năng có thể được đo lường khách quan không? [Measurability]"
- "Các yêu cầu suy giảm (degradation) có được định nghĩa cho kịch bản tải cao không? [Edge Case, Gap]"

**Chất lượng Yêu cầu Bảo mật:** `security.md`

Sample items:

- "Các yêu cầu xác thực có được chỉ định cho tất cả các tài nguyên được bảo vệ không? [Coverage]"
- "Các yêu cầu bảo vệ dữ liệu có được định nghĩa cho thông tin nhạy cảm không? [Completeness]"
- "Mô hình mối đe dọa có được ghi lại và các yêu cầu có liên kết với nó không? [Traceability]"
- "Các yêu cầu bảo mật có nhất quán với các nghĩa vụ tuân thủ không? [Consistency]"
- "Các yêu cầu phản ứng khi thất bại/vi phạm bảo mật có được định nghĩa không? [Gap, Exception Flow]"

## Phản Ví dụ: Những gì KHÔNG nên làm

**❌ SAI - Những cái này test implementation, không phải yêu cầu:**

```markdown
- [ ] CHK001 - Verify landing page displays 3 episode cards [Spec §FR-001]
- [ ] CHK002 - Test hover states work correctly on desktop [Spec §FR-003]
- [ ] CHK003 - Confirm logo click navigates to home page [Spec §FR-010]
- [ ] CHK004 - Check that related episodes section shows 3-5 items [Spec §FR-005]
```

**✅ ĐÚNG - Những cái này test chất lượng yêu cầu:**

```markdown
- [ ] CHK001 - Số lượng và bố cục của các tập phim nổi bật có được chỉ định rõ ràng không? [Completeness, Spec §FR-001]
- [ ] CHK002 - Các yêu cầu trạng thái hover có được định nghĩa nhất quán cho tất cả các phần tử tương tác không? [Consistency, Spec §FR-003]
- [ ] CHK003 - Các yêu cầu điều hướng có rõ ràng cho tất cả các phần tử thương hiệu có thể click không? [Clarity, Spec §FR-010]
- [ ] CHK004 - Tiêu chí lựa chọn cho các tập phim liên quan có được ghi lại không? [Gap, Spec §FR-005]
- [ ] CHK005 - Các yêu cầu trạng thái loading có được định nghĩa cho dữ liệu tập phim bất đồng bộ không? [Gap]
- [ ] CHK006 - Các yêu cầu "phân cấp thị giác" có thể được đo lường khách quan không? [Measurability, Spec §FR-001]
```

**Sự Khác biệt Chính:**

- Sai: Test xem hệ thống có hoạt động đúng không
- Đúng: Test xem các yêu cầu có được viết đúng không
- Sai: Xác minh hành vi
- Đúng: Xác thực chất lượng yêu cầu
- Sai: "Nó có làm X không?"
- Đúng: "X có được chỉ định rõ ràng không?"
