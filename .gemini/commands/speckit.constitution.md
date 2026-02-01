
---
description: Tạo hoặc cập nhật hiến pháp dự án từ các đầu vào nguyên tắc, đảm bảo tất cả các template phụ thuộc được đồng bộ.
handoffs: 
  - label: Xây dựng Đặc tả
    agent: speckit.specify
    prompt: Triển khai đặc tả tính năng dựa trên hiến pháp cập nhật. Tôi muốn xây dựng...
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Dàn ý

Bạn đang cập nhật hiến pháp dự án tại `.specify/memory/constitution.md`. File này là một TEMPLATE chứa các token giữ chỗ (placeholder) trong ngoặc vuông (ví dụ `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`). Công việc của bạn là (a) thu thập/suy ra các giá trị cụ thể, (b) điền template một cách chính xác, và (c) lan truyền bất kỳ sửa đổi nào qua các tạo phẩm phụ thuộc.

Tuân theo luồng thực thi này:

1. Load template hiến pháp hiện có tại `.specify/memory/constitution.md`.
   - Xác định mọi token giữ chỗ có dạng `[ALL_CAPS_IDENTIFIER]`.
   **QUAN TRỌNG**: Người dùng có thể yêu cầu ít hơn hoặc nhiều hơn các nguyên tắc so với các nguyên tắc được sử dụng trong template. Nếu một số lượng được chỉ định, hãy tôn trọng điều đó - tuân theo template chung. Bạn sẽ cập nhật tài liệu tương ứng.

2. Thu thập/suy ra giá trị cho các placeholder:
   - Nếu đầu vào người dùng (hội thoại) cung cấp giá trị, hãy sử dụng nó.
   - Ngược lại, suy luận từ ngữ cảnh repo hiện có (README, docs, các phiên bản hiến pháp trước đó nếu được nhúng).
   - Đối với các ngày quản trị: `RATIFICATION_DATE` là ngày áp dụng ban đầu (nếu không biết thì hỏi hoặc đánh dấu TODO), `LAST_AMENDED_DATE` là hôm nay nếu có thay đổi, nếu không giữ nguyên.
   - `CONSTITUTION_VERSION` phải tăng theo quy tắc semantic versioning:
     - MAJOR: Loại bỏ hoặc định nghĩa lại quản trị/nguyên tắc không tương thích ngược.
     - MINOR: Thêm nguyên tắc/phần mới hoặc mở rộng hướng dẫn đáng kể.
     - PATCH: Làm rõ, câu chữ, sửa lỗi chính tả, các tinh chỉnh phi ngữ nghĩa.
   - Nếu loại tăng version mơ hồ, đề xuất lý do trước khi chốt.

3. Soạn thảo nội dung hiến pháp cập nhật:
   - Thay thế mọi placeholder bằng văn bản cụ thể (không để lại token ngoặc vuông nào ngoại trừ các vị trí template cố ý giữ lại mà dự án đã chọn chưa định nghĩa—phải giải trình rõ ràng).
   - Giữ nguyên phân cấp tiêu đề và các bình luận có thể bị xóa sau khi thay thế trừ khi chúng vẫn thêm hướng dẫn làm rõ.
   - Đảm bảo mỗi phần Nguyên tắc: dòng tên ngắn gọn, đoạn văn (hoặc danh sách bullet) nắm bắt các quy tắc không thể thương lượng, lý do rõ ràng nếu không hiển nhiên.
   - Đảm bảo phần Quản trị liệt kê quy trình sửa đổi, chính sách phiên bản, và kỳ vọng review tuân thủ.

4. Checklist lan truyền tính nhất quán (chuyển đổi checklist trước đó thành các xác thực hoạt động):
   - Đọc `.specify/templates/plan-template.md` và đảm bảo bất kỳ "Constitution Check" hoặc quy tắc nào phù hợp với các nguyên tắc cập nhật.
   - Đọc `.specify/templates/spec-template.md` cho sự phù hợp phạm vi/yêu cầu—cập nhật nếu hiến pháp thêm/bớt các phần bắt buộc hoặc ràng buộc.
   - Đọc `.specify/templates/tasks-template.md` và đảm bảo phân loại task phản ánh các loại task mới hoặc bị loại bỏ do nguyên tắc điều khiển (ví dụ: khả năng quan sát, phiên bản hóa, kỷ luật testing).
   - Đọc mỗi file lệnh trong `.specify/templates/commands/*.md` (bao gồm file này) để xác minh không còn tham chiếu lỗi thời nào.
   - Đọc bất kỳ tài liệu hướng dẫn runtime nào (ví dụ: `README.md`, `docs/quickstart.md`). Cập nhật tham chiếu đến các nguyên tắc đã thay đổi.

5. Tạo Báo cáo Tác động Đồng bộ (thêm vào đầu file hiến pháp dưới dạng comment HTML sau khi cập nhật):
   - Thay đổi phiên bản: cũ → mới
   - Danh sách các nguyên tắc đã sửa đổi (tiêu đề cũ → tiêu đề mới nếu đổi tên)
   - Các phần đã thêm
   - Các phần đã xóa
   - Các template yêu cầu cập nhật (✅ đã cập nhật / ⚠ đang chờ) với đường dẫn file
   - Các TODO theo dõi nếu có placeholder nào cố tình hoãn lại.

6. Xác thực trước khi output cuối cùng:
   - Không còn token ngoặc vuông không giải thích được.
   - Dòng phiên bản khớp với báo cáo.
   - Ngày định dạng ISO YYYY-MM-DD.
   - Các nguyên tắc mang tính tuyên bố, có thể kiểm thử, và không có ngôn ngữ mơ hồ ("should" → thay thế bằng lý do MUST/SHOULD nếu phù hợp).

7. Viết hiến pháp hoàn chỉnh lại vào `.specify/memory/constitution.md` (ghi đè).

8. Xuất ra tóm tắt cuối cùng cho người dùng với:
   - Phiên bản mới và lý do tăng version.
   - Bất kỳ file nào được cắm cờ để theo dõi thủ công.
   - Tin nhắn commit đề xuất (ví dụ: `docs: amend constitution to vX.Y.Z (principle additions + governance update)`).

Định dạng & Yêu cầu Phong cách:

- Sử dụng Markdown headings chính xác như trong template (không hạ/nâng cấp độ).
- Ngắt dòng dài để dễ đọc (<100 ký tự lý tưởng) nhưng không bắt buộc cứng nhắc.
- Giữ một dòng trống giữa các phần.
- Tránh khoảng trắng thừa ở cuối dòng.

Nếu người dùng cung cấp cập nhật một phần (ví dụ: chỉ sửa đổi một nguyên tắc), vẫn thực hiện các bước xác thực và quyết định phiên bản.

Nếu thông tin quan trọng bị thiếu (ví dụ: ngày thông qua thực sự không biết), chèn `TODO(<FIELD_NAME>): explanation` và bao gồm trong Báo cáo Tác động Đồng bộ dưới các mục bị hoãn.

Không tạo template mới; luôn hoạt động trên file `.specify/memory/constitution.md` hiện có.
