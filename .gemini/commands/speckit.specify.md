
---
description: Tạo hoặc cập nhật đặc tả tính năng (spec) từ mô tả tính năng bằng ngôn ngữ tự nhiên.
handoffs: 
  - label: Xây dựng Kế hoạch Kỹ thuật
    agent: speckit.plan
    prompt: Tạo kế hoạch cho spec này. Tôi đang xây dựng với...
  - label: Làm rõ Yêu cầu Spec
    agent: speckit.clarify
    prompt: Làm rõ các yêu cầu đặc tả (specification)
    send: true
---

## User Input

```text
$ARGUMENTS
```

Bạn **PHẢI** xem xét đầu vào của người dùng trước khi tiếp tục (nếu không trống).

## Dàn ý

Văn bản người dùng nhập sau `/speckit.specify` trong tin nhắn kích hoạt **chính là** mô tả tính năng. Hãy giả định bạn luôn có sẵn nó trong cuộc hội thoại này ngay cả khi `{{args}}` xuất hiện theo nghĩa đen bên dưới. Đừng yêu cầu người dùng lặp lại trừ khi họ cung cấp một lệnh trống.

Với mô tả tính năng đó, hãy làm như sau:

1. **Tạo một cái tên ngắn gọn** (2-4 từ) cho nhánh (branch):
   - Phân tích mô tả tính năng và trích xuất các từ khóa có ý nghĩa nhất
   - Tạo một tên ngắn 2-4 từ nắm bắt được bản chất của tính năng
   - Sử dụng định dạng danh từ-hành động khi có thể (ví dụ: "add-user-auth", "fix-payment-bug")
   - Giữ lại các thuật ngữ kỹ thuật và từ viết tắt (OAuth2, API, JWT, v.v.)
   - Giữ nó ngắn gọn nhưng đủ mô tả để hiểu tính năng trong nháy mắt
   - Ví dụ:
     - "Tôi muốn thêm xác thực người dùng" → "user-auth"
     - "Implement OAuth2 integration cho API" → "oauth2-api-integration"
     - "Tạo dashboard cho analytics" → "analytics-dashboard"
     - "Sửa lỗi timeout xử lý thanh toán" → "fix-payment-timeout"

2. **Kiểm tra các nhánh hiện có trước khi tạo nhánh mới**:

   a. Đầu tiên, fetch tất cả các nhánh remote để đảm bảo chúng ta có thông tin mới nhất:

      ```bash
      git fetch --all --prune
      ```

   b. Tìm số tính năng cao nhất trên tất cả các nguồn cho short-name:
      - Remote branches: `git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
      - Local branches: `git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
      - Specs directories: Kiểm tra các thư mục khớp với `specs/[0-9]+-<short-name>`

   c. Xác định số tiếp theo có sẵn:
      - Trích xuất tất cả các số từ cả ba nguồn
      - Tìm số N cao nhất
      - Sử dụng N+1 cho số nhánh mới

   d. Chạy script `.specify/scripts/powershell/create-new-feature.ps1 -Json "{{args}}"` với số và short-name đã tính toán:
      - Truyền `--number N+1` và `--short-name "your-short-name"` cùng với mô tả tính năng
      - Bash example: `.specify/scripts/powershell/create-new-feature.ps1 -Json "{{args}}" --json --number 5 --short-name "user-auth" "Add user authentication"`
      - PowerShell example: `.specify/scripts/powershell/create-new-feature.ps1 -Json "{{args}}" -Json -Number 5 -ShortName "user-auth" "Add user authentication"`

   **QUAN TRỌNG**:
   - Kiểm tra cả ba nguồn (remote branches, local branches, specs directories) để tìm số cao nhất
   - Chỉ khớp các nhánh/thư mục với mẫu short-name chính xác
   - Nếu không tìm thấy nhánh/thư mục hiện có nào với short-name này, bắt đầu với số 1
   - Bạn chỉ được chạy script này một lần cho mỗi tính năng
   - JSON được cung cấp trong terminal dưới dạng output - luôn tham chiếu đến nó để lấy nội dung thực tế bạn đang tìm kiếm
   - Output JSON sẽ chứa BRANCH_NAME và đường dẫn SPEC_FILE
   - Đối với dấu nháy đơn trong args như "I'm Groot", sử dụng cú pháp escape: ví dụ 'I'\\''m Groot' (hoặc ngoặc kép nếu có thể: "I'm Groot")

3. Load `.specify/templates/spec-template.md` để hiểu các phần bắt buộc.

4. Tuân theo luồng thực thi này:

    1. Phân tích mô tả người dùng từ Input
       Nếu trống: LỖI "Không có mô tả tính năng nào được cung cấp"
    2. Trích xuất các khái niệm chính từ mô tả
       Xác định: tác nhân (actors), hành động (actions), dữ liệu (data), ràng buộc (constraints)
    3. Đối với các khía cạnh không rõ ràng:
       - Đưa ra phỏng đoán có căn cứ dựa trên ngữ cảnh và tiêu chuẩn ngành
       - Chỉ đánh dấu với [CẦN LÀM RÕ: câu hỏi cụ thể] nếu:
         - Sự lựa chọn ảnh hưởng đáng kể đến phạm vi tính năng hoặc trải nghiệm người dùng
         - Tồn tại nhiều cách hiểu hợp lý với các ý nghĩa khác nhau
         - Không tồn tại mặc định hợp lý
       - **GIỚI HẠN: Tối đa 3 marker [CẦN LÀM RÕ] tổng cộng**
       - Ưu tiên làm rõ theo tác động: phạm vi > bảo mật/quyền riêng tư > trải nghiệm người dùng > chi tiết kỹ thuật
    4. Điền phần Kịch bản Người dùng & Kiểm thử
       Nếu không rõ luồng người dùng: LỖI "Không thể xác định kịch bản người dùng"
    5. Tạo Yêu cầu Chức năng
       Mỗi yêu cầu phải có thể kiểm thử được
       Sử dụng các mặc định hợp lý cho các chi tiết không được chỉ định (tài liệu hóa các giả định trong phần Giả định)
    6. Định nghĩa Tiêu chí Thành công
       Tạo các kết quả đo lường được, không phụ thuộc công nghệ
       Bao gồm cả các chỉ số định lượng (thời gian, hiệu năng, khối lượng) và định tính (sự hài lòng của người dùng, hoàn thành tác vụ)
       Mỗi tiêu chí phải có thể xác minh được mà không cần biết chi tiết triển khai
    7. Xác định các Thực thể Chính (nếu liên quan đến dữ liệu)
    8. Trả về: THÀNH CÔNG (spec đã sẵn sàng để lập kế hoạch)

5. Viết đặc tả vào SPEC_FILE sử dụng cấu trúc template, thay thế các placeholder bằng các chi tiết cụ thể được trích xuất từ mô tả tính năng (arguments) trong khi vẫn giữ nguyên thứ tự các phần và tiêu đề.

6. **Xác thực Chất lượng Đặc tả**: Sau khi viết spec ban đầu, hãy xác thực nó dựa trên các tiêu chí chất lượng:

   a. **Tạo Checklist Chất lượng Spec**: Tạo file checklist tại `FEATURE_DIR/checklists/requirements.md` sử dụng cấu trúc template checklist với các mục xác thực này:

      ```markdown
      # Checklist Chất lượng Đặc tả: [TÊN TÍNH NĂNG]
      
      **Mục đích**: Xác thực tính đầy đủ và chất lượng của đặc tả trước khi tiến hành lập kế hoạch
      **Được tạo**: [DATE]
      **Tính năng**: [Link đến spec.md]
      
      ## Chất lượng Nội dung
      
      - [ ] Không có chi tiết triển khai (ngôn ngữ, frameworks, APIs)
      - [ ] Tập trung vào giá trị người dùng và nhu cầu kinh doanh
      - [ ] Được viết cho các bên liên quan phi kỹ thuật
      - [ ] Tất cả các phần bắt buộc đã hoàn thành
      
      ## Tính Đầy đủ của Yêu cầu
      
      - [ ] Không còn marker [CẦN LÀM RÕ] nào
      - [ ] Yêu cầu có thể kiểm thử và không mơ hồ
      - [ ] Tiêu chí thành công có thể đo lường được
      - [ ] Tiêu chí thành công không phụ thuộc công nghệ (không có chi tiết triển khai)
      - [ ] Tất cả các kịch bản chấp nhận được định nghĩa
      - [ ] Các edge cases được xác định
      - [ ] Phạm vi được giới hạn rõ ràng
      - [ ] Dependencies và giả định được xác định
      
      ## Tính Sẵn sàng của Tính năng
      
      - [ ] Tất cả các yêu cầu chức năng đều có tiêu chí chấp nhận rõ ràng
      - [ ] Các kịch bản người dùng bao phủ các luồng chính
      - [ ] Tính năng đáp ứng các kết quả đo lường được định nghĩa trong Tiêu chí Thành công
      - [ ] Không có chi tiết triển khai rò rỉ vào đặc tả
      
      ## Ghi chú
      
      - Các mục được đánh dấu chưa hoàn thành yêu cầu cập nhật spec trước `/speckit.clarify` hoặc `/speckit.plan`
      ```

   b. **Chạy Kiểm tra Xác thực**: Review spec dựa trên từng mục checklist:
      - Đối với mỗi mục, xác định xem nó đạt hay không đạt
      - Ghi lại các vấn đề cụ thể được tìm thấy (trích dẫn các phần spec liên quan)

   c. **Xử lý Kết quả Xác thực**:

      - **Nếu tất cả các mục đều đạt**: Đánh dấu checklist hoàn thành và tiến hành bước 6

      - **Nếu có mục không đạt (ngoại trừ [CẦN LÀM RÕ])**:
        1. Liệt kê các mục không đạt và vấn đề cụ thể
        2. Cập nhật spec để giải quyết từng vấn đề
        3. Chạy lại xác thực cho đến khi tất cả các mục đều đạt (tối đa 3 lần lặp)
        4. Nếu vẫn không đạt sau 3 lần lặp, ghi lại các vấn đề còn lại trong ghi chú checklist và cảnh báo người dùng

      - **Nếu còn marker [CẦN LÀM RÕ]**:
        1. Trích xuất tất cả marker [CẦN LÀM RÕ: ...] từ spec
        2. **KIỂM TRA GIỚI HẠN**: Nếu tồn tại hơn 3 marker, chỉ giữ lại 3 cái quan trọng nhất (theo tác động phạm vi/bảo mật/UX) và đưa ra phỏng đoán có căn cứ cho phần còn lại
        3. Đối với mỗi sự làm rõ cần thiết (tối đa 3), trình bày các tùy chọn cho người dùng theo định dạng này:

           ```markdown
           ## Câu hỏi [N]: [Chủ đề]
           
           **Ngữ cảnh**: [Trích dẫn phần spec liên quan]
           
           **Điều chúng ta cần biết**: [Câu hỏi cụ thể từ marker CẦN LÀM RÕ]
           
           **Các câu trả lời gợi ý**:
           
           | Tùy chọn | Câu trả lời | Ý nghĩa |
           |----------|-------------|---------|
           | A        | [Câu trả lời gợi ý 1] | [Điều này có nghĩa gì cho tính năng] |
           | B        | [Câu trả lời gợi ý 2] | [Điều này có nghĩa gì cho tính năng] |
           | C        | [Câu trả lời gợi ý 3] | [Điều này có nghĩa gì cho tính năng] |
           | Custom   | Cung cấp câu trả lời của bạn | [Giải thích cách cung cấp input tùy chỉnh] |
           
           **Lựa chọn của bạn**: _[Chờ người dùng phản hồi]_
           ```

        4. **QUAN TRỌNG - Định dạng Bảng**: Đảm bảo các bảng markdown được định dạng đúng:
           - Sử dụng khoảng cách nhất quán với các dấu gạch đứng (pipes) thẳng hàng
           - Mỗi ô nên có khoảng trắng xung quanh nội dung: `| Content |` không phải `|Content|`
           - Dấu phân cách header phải có ít nhất 3 dấu gạch ngang: `|--------|`
           - Test rằng bảng hiển thị chính xác trong markdown preview
        5. Đánh số câu hỏi tuần tự (Q1, Q2, Q3 - tối đa 3 tổng cộng)
        6. Trình bày tất cả các câu hỏi cùng nhau trước khi chờ phản hồi
        7. Chờ người dùng phản hồi với lựa chọn của họ cho tất cả các câu hỏi (ví dụ: "Q1: A, Q2: Custom - [chi tiết], Q3: B")
        8. Cập nhật spec bằng cách thay thế mỗi marker [CẦN LÀM RÕ] bằng câu trả lời đã chọn hoặc cung cấp của người dùng
        9. Chạy lại xác thực sau khi tất cả các làm rõ được giải quyết

   d. **Cập nhật Checklist**: Sau mỗi lần lặp xác thực, cập nhật file checklist với trạng thái đạt/không đạt hiện tại

7. Báo cáo hoàn thành với tên nhánh, đường dẫn file spec, kết quả checklist, và mức độ sẵn sàng cho giai đoạn tiếp theo (`/speckit.clarify` hoặc `/speckit.plan`).

**LƯU Ý:** Script tạo và checkout nhánh mới và khởi tạo file spec trước khi viết.

## Hướng dẫn Chung

## Hướng dẫn Nhanh

- Tập trung vào **CÁI GÌ** người dùng cần và **TẠI SAO**.
- Tránh CÁCH triển khai (không tech stack, APIs, cấu trúc code).
- Viết cho các bên liên quan kinh doanh, không phải lập trình viên.
- KHÔNG tạo bất kỳ checklist nào được nhúng trong spec. Đó sẽ là một lệnh riêng biệt.

### Yêu cầu về Phần (Section Requirements)

- **Các phần bắt buộc**: Phải được hoàn thành cho mọi tính năng
- **Các phần tùy chọn**: Chỉ bao gồm khi liên quan đến tính năng
- Khi một phần không áp dụng, xóa hoàn toàn nó (đừng để là "N/A")

### Dành cho AI Generation

Khi tạo spec này từ prompt của người dùng:

1. **Đưa ra phỏng đoán có căn cứ**: Sử dụng ngữ cảnh, tiêu chuẩn ngành, và các mẫu phổ biến để lấp đầy khoảng trống
2. **Tài liệu hóa các giả định**: Ghi lại các mặc định hợp lý trong phần Giả định
3. **Hạn chế làm rõ**: Tối đa 3 marker [CẦN LÀM RÕ] - chỉ sử dụng cho các quyết định quan trọng mà:
   - Ảnh hưởng đáng kể đến phạm vi tính năng hoặc trải nghiệm người dùng
   - Có nhiều cách hiểu hợp lý với các ý nghĩa khác nhau
   - Thiếu bất kỳ mặc định hợp lý nào
4. **Ưu tiên làm rõ**: phạm vi > bảo mật/quyền riêng tư > trải nghiệm người dùng > chi tiết kỹ thuật
5. **Suy nghĩ như một tester**: Mọi yêu cầu mơ hồ nên fail mục checklist "có thể kiểm thử và không mơ hồ"
6. **Các khu vực phổ biến cần làm rõ** (chỉ khi không tồn tại mặc định hợp lý):
   - Phạm vi tính năng và ranh giới (bao gồm/loại trừ các trường hợp sử dụng cụ thể)
   - Loại người dùng và quyền hạn (nếu có thể có nhiều cách hiểu xung đột)
   - Yêu cầu bảo mật/tuân thủ (khi quan trọng về mặt pháp lý/tài chính)

**Ví dụ về các mặc định hợp lý** (đừng hỏi về những cái này):

- Lưu giữ dữ liệu: Thực hành tiêu chuẩn ngành cho domain
- Mục tiêu hiệu năng: Kỳ vọng ứng dụng web/mobile tiêu chuẩn trừ khi được chỉ định
- Xử lý lỗi: Thông báo thân thiện với người dùng với các fallback phù hợp
- Phương thức xác thực: Session-based tiêu chuẩn hoặc OAuth2 cho web apps
- Mô hình tích hợp: RESTful APIs trừ khi được chỉ định khác

### Hướng dẫn Tiêu chí Thành công

Tiêu chí thành công phải là:

1. **Đo lường được**: Bao gồm các chỉ số cụ thể (thời gian, tỷ lệ phần trăm, số lượng, tốc độ)
2. **Không phụ thuộc công nghệ**: Không đề cập đến frameworks, ngôn ngữ, cơ sở dữ liệu, hoặc công cụ
3. **Tập trung vào người dùng**: Mô tả kết quả từ góc nhìn người dùng/doanh nghiệp, không phải nội bộ hệ thống
4. **Có thể xác minh**: Có thể được test/validate mà không cần biết chi tiết triển khai

**Ví dụ tốt**:

- "Người dùng có thể hoàn tất thanh toán trong dưới 3 phút"
- "Hệ thống hỗ trợ 10,000 người dùng đồng thời"
- "95% các tìm kiếm trả về kết quả trong dưới 1 giây"
- "Tỷ lệ hoàn thành tác vụ cải thiện 40%"

**Ví dụ xấu** (tập trung vào triển khai):

- "Thời gian phản hồi API dưới 200ms" (quá kỹ thuật, dùng "Người dùng thấy kết quả ngay lập tức")
- "Database có thể xử lý 1000 TPS" (chi tiết triển khai, dùng chỉ số hướng người dùng)
- "Các component React render hiệu quả" (framework-specific)
- "Tỷ lệ cache hit Redis trên 80%" (technology-specific)
