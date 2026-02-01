# Đặc tả Tính năng: [FEATURE NAME]

**Nhánh Tính năng**: `[###-feature-name]`  
**Được tạo**: [DATE]  
**Trạng thái**: Bản nháp (Draft)  
**Đầu vào**: Mô tả của người dùng: "$ARGUMENTS"

## Kịch bản Người dùng & Kiểm thử *(bắt buộc)*

<!--
  QUAN TRỌNG: Các user story nên được ƯU TIÊN như các hành trình người dùng được sắp xếp theo mức độ quan trọng.
  Mỗi user story/hành trình phải CÓ THỂ KIỂM THỬ ĐỘC LẬP - nghĩa là nếu bạn chỉ triển khai MỘT trong số chúng,
  bạn vẫn nên có một MVP (Sản phẩm Khả thi Tối thiểu) mang lại giá trị.
  
  Gán độ ưu tiên (P1, P2, P3, v.v.) cho mỗi story, trong đó P1 là quan trọng nhất.
  Hãy nghĩ về mỗi story như một lát cắt chức năng độc lập có thể:
  - Được phát triển độc lập
  - Được kiểm thử độc lập
  - Được triển khai độc lập
  - Được demo cho người dùng độc lập
-->

### User Story 1 - [Tiêu đề Ngắn gọn] (Ưu tiên: P1)

[Mô tả hành trình người dùng này bằng ngôn ngữ thông thường]

**Tại sao ưu tiên này**: [Giải thích giá trị và tại sao nó có mức ưu tiên này]

**Test Độc lập**: [Mô tả cách cái này có thể được test độc lập - ví dụ: "Có thể được test đầy đủ bởi [hành động cụ thể] và mang lại [giá trị cụ thể]"]

**Kịch bản Chấp nhận (Acceptance Scenarios)**:

1. **Given (Với)** [trạng thái ban đầu], **When (Khi)** [hành động], **Then (Thì)** [kết quả mong đợi]
2. **Given (Với)** [trạng thái ban đầu], **When (Khi)** [hành động], **Then (Thì)** [kết quả mong đợi]

---

### User Story 2 - [Tiêu đề Ngắn gọn] (Ưu tiên: P2)

[Mô tả hành trình người dùng này bằng ngôn ngữ thông thường]

**Tại sao ưu tiên này**: [Giải thích giá trị và tại sao nó có mức ưu tiên này]

**Test Độc lập**: [Mô tả cách cái này có thể được test độc lập]

**Kịch bản Chấp nhận**:

1. **Given (Với)** [trạng thái ban đầu], **When (Khi)** [hành động], **Then (Thì)** [kết quả mong đợi]

---

### User Story 3 - [Tiêu đề Ngắn gọn] (Ưu tiên: P3)

[Mô tả hành trình người dùng này bằng ngôn ngữ thông thường]

**Tại sao ưu tiên này**: [Giải thích giá trị và tại sao nó có mức ưu tiên này]

**Test Độc lập**: [Mô tả cách cái này có thể được test độc lập]

**Kịch bản Chấp nhận**:

1. **Given (Với)** [trạng thái ban đầu], **When (Khi)** [hành động], **Then (Thì)** [kết quả mong đợi]

---

[Thêm các user story khác nếu cần, mỗi cái với một mức ưu tiên được gán]

### Các Trường hợp Ngoài lề (Edge Cases)

<!--
  HÀNH ĐỘNG CẦN THIẾT: Nội dung trong phần này đại diện cho các placeholder.
  Hãy điền vào chúng với các trường hợp ngoài lề (edge cases) đúng.
-->

- Chuyện gì xảy ra khi [điều kiện biên]?
- Hệ thống xử lý [kịch bản lỗi] như thế nào?

## Các Yêu cầu *(bắt buộc)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Nội dung trong phần này đại diện cho các placeholder.
  Hãy điền vào chúng với các yêu cầu chức năng đúng.
-->

### Yêu cầu Chức năng

- **FR-001**: Hệ thống PHẢI [khả năng cụ thể, ví dụ: "cho phép người dùng tạo tài khoản"]
- **FR-002**: Hệ thống PHẢI [khả năng cụ thể, ví dụ: "xác thực địa chỉ email"]  
- **FR-003**: Người dùng PHẢI có thể [tương tác chính, ví dụ: "đặt lại mật khẩu của họ"]
- **FR-004**: Hệ thống PHẢI [yêu cầu dữ liệu, ví dụ: "lưu trữ tùy chọn người dùng"]
- **FR-005**: Hệ thống PHẢI [hành vi, ví dụ: "ghi log tất cả các sự kiện bảo mật"]

*Ví dụ về cách đánh dấu các yêu cầu không rõ ràng:*

- **FR-006**: Hệ thống PHẢI xác thực người dùng qua [CẦN LÀM RÕ: phương thức auth chưa được chỉ định - email/password, SSO, OAuth?]
- **FR-007**: Hệ thống PHẢI lưu giữ dữ liệu người dùng trong [CẦN LÀM RÕ: thời gian lưu giữ chưa được chỉ định]

### Các Thực thể Chính *(bao gồm nếu tính năng liên quan đến dữ liệu)*

- **[Thực thể 1]**: [Nó đại diện cho cái gì, các thuộc tính chính mà không cần implementation]
- **[Thực thể 2]**: [Nó đại diện cho cái gì, mối quan hệ với các thực thể khác]

## Tiêu chí Thành công *(bắt buộc)*

<!--
  HÀNH ĐỘNG CẦN THIẾT: Xác định các tiêu chí thành công có thể đo lường được.
  Những tiêu chí này phải độc lập với công nghệ và có thể đo lường.
-->

### Các Kết quả Đo lường được

- **SC-001**: [Chỉ số đo lường, ví dụ: "Người dùng có thể hoàn tất tạo tài khoản trong dưới 2 phút"]
- **SC-002**: [Chỉ số đo lường, ví dụ: "Hệ thống xử lý 1000 người dùng đồng thời không bị suy giảm"]
- **SC-003**: [Chỉ số hài lòng của người dùng, ví dụ: "90% người dùng hoàn thành tác vụ chính thành công trong lần thử đầu tiên"]
- **SC-004**: [Chỉ số kinh doanh, ví dụ: "Giảm 50% vé hỗ trợ liên quan đến [X]"]
