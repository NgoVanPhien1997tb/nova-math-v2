# Kế hoạch Triển khai: [FEATURE]

**Nhánh**: `[###-feature-name]` | **Ngày**: [DATE] | **Spec**: [link]
**Đầu vào**: Đặc tả tính năng từ `/specs/[###-feature-name]/spec.md`

**Lưu ý**: Template này được điền bởi lệnh `/speckit.plan`. Xem `.specify/templates/commands/plan.md` để biết quy trình thực thi.

## Tóm tắt

[Trích xuất từ spec tính năng: yêu cầu chính + hướng tiếp cận kỹ thuật từ nghiên cứu]

## Ngữ cảnh Kỹ thuật

<!--
  HÀNH ĐỘNG CẦN THIẾT: Thay thế nội dung trong phần này bằng các chi tiết kỹ thuật
  cho dự án. Cấu trúc ở đây được trình bày với tư cách tham vấn để hướng dẫn
  quá trình lặp lại.
-->

**Ngôn ngữ/Phiên bản**: [ví dụ: Python 3.11, Swift 5.9, Rust 1.75 hoặc CẦN LÀM RÕ]  
**Các Dependencies chính**: [ví dụ: FastAPI, UIKit, LLVM hoặc CẦN LÀM RÕ]  
**Lưu trữ**: [nếu áp dụng, ví dụ: PostgreSQL, CoreData, files hoặc N/A]  
**Kiểm thử**: [ví dụ: pytest, XCTest, cargo test hoặc CẦN LÀM RÕ]  
**Nền tảng Mục tiêu**: [ví dụ: Linux server, iOS 15+, WASM hoặc CẦN LÀM RÕ]
**Loại Dự án**: [single/web/mobile - quyết định cấu trúc nguồn]  
**Mục tiêu Hiệu năng**: [đặc thù domain, ví dụ: 1000 req/s, 10k dòng/giây, 60 fps hoặc CẦN LÀM RÕ]  
**Các Ràng buộc**: [đặc thù domain, ví dụ: <200ms p95, <100MB memory, có khả năng offline hoặc CẦN LÀM RÕ]  
**Quy mô/Phạm vi**: [đặc thù domain, ví dụ: 10k users, 1M LOC, 50 màn hình hoặc CẦN LÀM RÕ]

## Kiểm tra Hiến pháp

*CỔNG (GATE): Phải vượt qua trước khi nghiên cứu Phase 0. Kiểm tra lại sau khi thiết kế Phase 1.*

[Các cổng được xác định dựa trên file hiến pháp]

## Cấu trúc Dự án

### Tài liệu (tính năng này)

```text
specs/[###-feature]/
├── plan.md              # File này (output của lệnh /speckit.plan)
├── research.md          # Output Phase 0 (lệnh /speckit.plan)
├── data-model.md        # Output Phase 1 (lệnh /speckit.plan)
├── quickstart.md        # Output Phase 1 (lệnh /speckit.plan)
├── contracts/           # Output Phase 1 (lệnh /speckit.plan)
└── tasks.md             # Output Phase 2 (lệnh /speckit.tasks - KHÔNG được tạo bởi /speckit.plan)
```

### Mã nguồn (gốc repository)
<!--
  HÀNH ĐỘNG CẦN THIẾT: Thay thế cây placeholder bên dưới bằng bố cục cụ thể
  cho tính năng này. Xóa các tùy chọn không dùng và mở rộng cấu trúc đã chọn với
  các đường dẫn thực (ví dụ: apps/admin, packages/something). Kế hoạch được giao phải
  không bao gồm nhãn Option.
-->

```text
# [XÓA NẾU KHÔNG DÙNG] Tùy chọn 1: Dự án đơn (MẶC ĐỊNH)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [XÓA NẾU KHÔNG DÙNG] Tùy chọn 2: Ứng dụng Web (khi phát hiện "frontend" + "backend")
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [XÓA NẾU KHÔNG DÙNG] Tùy chọn 3: Mobile + API (khi phát hiện "iOS/Android")
api/
└── [tương tự backend ở trên]

ios/ hoặc android/
└── [cấu trúc đặc thù nền tảng: feature modules, UI flows, platform tests]
```

**Quyết định Cấu trúc**: [Ghi lại cấu trúc đã chọn và tham chiếu các thư mục thực tế đã chụp ở trên]

## Theo dõi Độ phức tạp

> **CHỈ điền nếu Kiểm tra Hiến pháp có các vi phạm cần được giải trình**

| Vi phạm | Tại sao Cần thiết | Giải pháp Đơn giản hơn Bị TỪ CHỐI Vì |
|-----------|------------|-------------------------------------|
| [ví dụ: dự án thứ 4] | [nhu cầu hiện tại] | [tại sao 3 dự án là không đủ] |
| [ví dụ: Repository pattern] | [vấn đề cụ thể] | [tại sao truy cập DB trực tiếp là không đủ] |
