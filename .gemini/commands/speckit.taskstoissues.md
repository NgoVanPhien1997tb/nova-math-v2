
---
description: Chuyển đổi các task hiện có thành các issue GitHub có thể hành động, được sắp xếp theo phụ thuộc cho tính năng dựa trên các tạo phẩm thiết kế có sẵn.
tools: ['github/github-mcp-server/issue_write']
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Dàn ý

1. Chạy `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` từ repo root và parse FEATURE_DIR và danh sách AVAILABLE_DOCS. Tất cả đường dẫn phải là tuyệt đối. Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape.
2. Từ script đã thực thi, trích xuất đường dẫn đến **tasks**.
3. Lấy Git remote bằng cách chạy:

```bash
git config --get remote.origin.url
```

> [!CAUTION]
> CHỈ TIẾP TỤC CÁC BƯỚC TIẾP THEO NẾU REMOTE LÀ MỘT URL GITHUB

4. Với mỗi task trong danh sách, sử dụng GitHub MCP server để tạo một issue mới trong repository đại diện cho Git remote.

> [!CAUTION]
> TRONG MỌI TRƯỜNG HỢP KHÔNG BAO GIỜ TẠO ISSUE TRONG CÁC REPOSITORY KHÔNG KHỚP VỚI REMOTE URL
