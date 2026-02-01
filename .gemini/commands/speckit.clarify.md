
---
description: Xác định các khu vực chưa được chỉ định đầy đủ trong spec tính năng hiện tại bằng cách hỏi tối đa 5 câu hỏi làm rõ có mục tiêu cao và mã hóa câu trả lời trở lại vào spec.
handoffs: 
  - label: Xây dựng Kế hoạch Kỹ thuật
    agent: speckit.plan
    prompt: Tạo kế hoạch cho spec này. Tôi đang xây dựng với...
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Dàn ý

Mục tiêu: Phát hiện và giảm thiểu sự mơ hồ hoặc các điểm quyết định còn thiếu trong đặc tả tính năng đang hoạt động và ghi lại các làm rõ trực tiếp vào file spec.

Các bước thực thi:

1. Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json -PathsOnly` từ repo root **một lần**. Parse các trường payload JSON tối thiểu: `FEATURE_DIR`, `FEATURE_SPEC`.
   - Nếu parse JSON thất bại, hủy bỏ và hướng dẫn người dùng chạy lại `/speckit.specify`.
   - Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape.

2. Load file spec hiện tại. Thực hiện quét độ bao phủ & mơ hồ có cấu trúc.
   Các hạng mục quét: Phạm vi Chức năng, Domain & Mô hình Dữ liệu, Tương tác & Luồng UX, Các Thuộc tính Chất lượng Phi chức năng, Tích hợp & Dependencies, Edge Cases, Ràng buộc, Thuật ngữ.

   Đối với mỗi danh mục có trạng thái Một phần hoặc Thiếu, thêm một cơ hội câu hỏi ứng viên trừ khi việc làm rõ không quan trọng lúc này.

3. Tạo (nội bộ) hàng đợi ưu tiên các câu hỏi làm rõ (tối đa 5). KHÔNG xuất tất cả cùng lúc.
    - Tối đa 10 câu hỏi tổng cộng trong phiên.
    - Câu hỏi phải trả lời được bằng trắc nghiệm hoặc câu ngắn (<=5 từ).
    - Chỉ hỏi các câu hỏi có tác động lớn đến kiến trúc, dữ liệu, hoặc UX.

4. Vòng lặp hỏi tuần tự (tương tác):
    - Trình bày CHÍNH XÁC MỘT câu hỏi tại một thời điểm.
    - Với trắc nghiệm: Phân tích và đưa ra **Khuyến nghị** rõ ràng. Sau đó hiển thị bảng các Tùy chọn (Option | Mô tả).
    - Với câu trả lời ngắn: Đưa ra **Gợi ý** dựa trên best practices.
    - Chờ người dùng trả lời. Xác thực câu trả lời.
    - Dừng khi hết câu hỏi hoặc người dùng yêu cầu dừng.

5. Tích hợp sau MỖI câu trả lời được chấp nhận:
    - Thêm vào phần `## Clarifications`, dưới `### Session YYYY-MM-DD`.
    - Format: `- Q: <câu hỏi> → A: <câu trả lời>`.
    - Áp dụng ngay lập tức vào phần spec phù hợp (Yêu cầu chức năng, Data model, v.v.).
    - Lưu file spec SAU MỖI lần tích hợp.

6. Xác thực: Đảm bảo không trùng lặp, Markdown hợp lệ, tính nhất quán thuật ngữ.

7. Viết spec đã cập nhật lại vào `FEATURE_SPEC`.

8. Báo cáo hoàn thành: Số câu hỏi, đường dẫn spec, tóm tắt bao phủ, và đề xuất bước tiếp theo (`/speckit.plan`).

Quy tắc hành vi:
- Nếu không tìm thấy sự mơ hồ quan trọng, báo cáo và đề xuất tiếp tục.
- Không bao giờ vượt quá 5 câu hỏi.
- Tôn trọng tín hiệu chấm dứt của người dùng.

Ngữ cảnh cho sự ưu tiên: {{args}}
