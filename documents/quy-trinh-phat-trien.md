# 📋 Quy Trình Phát Triển Nova Math v2

> **Tài liệu này mô tả đầy đủ quy trình phát triển tính năng từ ý tưởng đến hoàn thành.**

---

## 🗺️ Tổng Quan Quy Trình

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│    PHASE 1               PHASE 2                PHASE 3                     │
│   CHUẨN BỊ              LẬP KẾ HOẠCH            TRIỂN KHAI                  │
│   (1 lần)               (mỗi tính năng)         (mỗi tính năng)             │
│                                                                             │
│  ┌─────────┐          ┌─────────────┐         ┌──────────────┐             │
│  │ ① Hiến  │   ──►    │  ② Viết     │   ──►   │  ⑦ Backend   │             │
│  │   Pháp  │          │    Spec     │         │    (Java)    │             │
│  └─────────┘          └─────────────┘         └──────┬───────┘             │
│                              │                       │                     │
│                              ▼                       ▼                     │
│                       ┌─────────────┐         ┌──────────────┐             │
│                       │ ③ Làm rõ &  │         │  ⑧ Frontend  │             │
│                       │   Phân tích │         │  (Angular)   │             │
│                       └─────────────┘         └──────┬───────┘             │
│                              │                       │                     │
│                              ▼                       ▼                     │
│                       ┌─────────────┐         ┌──────────────┐             │
│                       │ ④ Plan +    │         │  ⑨ Test &    │             │
│                       │  Contracts  │◄────────│    Review    │             │
│                       └─────────────┘         └──────────────┘             │
│                              │                                             │
│                              ▼                                             │
│                       ┌─────────────┐                                      │
│                       │  ⑤ Tasks    │                                      │
│                       └──────┬──────┘                                      │
│                              │                                             │
│                              ▼                                             │
│                       ┌─────────────┐                                      │
│                       │ ⑥ GitHub    │                                      │
│                       │   Issues    │                                      │
│                       └─────────────┘                                      │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 📖 Chi Tiết Từng Bước

---

### PHASE 1: CHUẨN BỊ (Làm 1 lần cho cả dự án)

#### Bước ① Tạo Hiến Pháp Dự Án

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-constitution` |
| **Mục đích** | Định nghĩa các nguyên tắc, quy tắc chung cho cả dự án |
| **Output** | `.specify/memory/constitution.md` |

**Nội dung Hiến Pháp bao gồm:**
- Tech stack: Java Spring Boot + Angular
- Database: PostgreSQL
- Nguyên tắc code: TDD, Clean Code
- Chuẩn API: RESTful, JSON
- Quy trình review code

---

### PHASE 2: LẬP KẾ HOẠCH (Làm cho từng tính năng)

#### Bước ② Viết Spec (Đặc tả tính năng)

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-specify` |
| **Input** | Mô tả tính năng bằng ngôn ngữ tự nhiên |
| **Output** | `specs/[tên-tính-năng]/spec.md` |

**Ví dụ:**
```
Input: "Tôi muốn người dùng có thể đăng nhập bằng email và mật khẩu"

Output: specs/dang-nhap/spec.md
- Mô tả user story
- Các yêu cầu chức năng
- Các yêu cầu phi chức năng
- Các trường hợp đặc biệt
```

---

#### Bước ③ Làm Rõ & Phân Tích (Tuỳ chọn nhưng khuyến khích)

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-clarify` + `/spec-driven-dev-analyze` |
| **Mục đích** | Tìm và làm rõ những điểm mơ hồ trong spec |
| **Output** | Spec được cập nhật đầy đủ hơn |

---

#### Bước ④ Lập Plan + Viết Hợp Đồng API

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-plan` |
| **Input** | `spec.md` |
| **Output** | `plan.md` + `contracts/api.md` + `contracts/models.md` |

**🔴 QUAN TRỌNG - Thư mục `contracts/` chứa:**

```
specs/[tên-tính-năng]/
├── spec.md
├── plan.md
└── contracts/                    ← HỢP ĐỒNG API
    ├── api.md                    ← Định nghĩa tất cả API endpoints
    └── models.md                 ← Định nghĩa các đối tượng dữ liệu
```

**File `contracts/api.md` bao gồm:**
- Danh sách tất cả API endpoints
- Chi tiết từng API:
  - URL, Method (GET/POST/PUT/DELETE)
  - Request (headers, body, params)
  - Response success (với ví dụ JSON)
  - Response error (các trường hợp lỗi)

**File `contracts/models.md` bao gồm:**
- Entities (bảng database)
- DTOs (request/response objects)
- Enums (các giá trị cố định)

---

#### Bước ⑤ Chia Tasks

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-tasks` |
| **Input** | `plan.md` + `contracts/` |
| **Output** | `tasks.md` |

**⭐ Tasks được chia riêng cho Backend và Frontend:**

```markdown
# Tasks - Tính năng Đăng nhập

## Backend Tasks
- BE-1: Tạo API đăng nhập (theo contracts/api.md)
- BE-2: Tạo API đăng xuất (theo contracts/api.md)

## Frontend Tasks
- FE-1: Tạo trang đăng nhập (gọi API theo contracts/api.md)
- FE-2: Xử lý lưu token
```

---

#### Bước ⑥ Đẩy Tasks lên GitHub Issues

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-taskstoissues` |
| **Input** | `tasks.md` |
| **Output** | GitHub Issues với labels |

**Labels được thêm:**
- `backend` - cho các task BE
- `frontend` - cho các task FE
- `feature/[tên]` - nhóm theo tính năng

---

### PHASE 3: TRIỂN KHAI

#### Bước ⑦ Triển Khai Backend TRƯỚC

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-implement` |
| **Nguồn tham khảo** | GitHub Issues (label: backend) + `contracts/api.md` |

**Dev Backend làm:**
1. Mở GitHub Issue có label `backend`
2. **Đọc `contracts/api.md`** để biết API cần implement như thế nào
3. **Đọc `contracts/models.md`** để biết cấu trúc Entity/DTO
4. Viết code theo đúng hợp đồng
5. Test API
6. Đánh dấu Issue là Done

---

#### Bước ⑧ Triển Khai Frontend SAU

| Thông tin | Chi tiết |
|-----------|----------|
| **Workflow** | `/spec-driven-dev-implement` |
| **Nguồn tham khảo** | GitHub Issues (label: frontend) + `contracts/api.md` |

**Dev Frontend làm:**
1. Mở GitHub Issue có label `frontend`
2. **Đọc `contracts/api.md`** để biết:
   - Gọi URL nào
   - Gửi data gì
   - Nhận về data có format như thế nào
3. **Đọc `contracts/models.md`** để tạo TypeScript interfaces
4. Viết code Angular gọi API đúng theo hợp đồng
5. Đánh dấu Issue là Done

---

#### Bước ⑨ Test Tích Hợp & Review

**Kiểm tra:**
- [ ] Frontend gọi API Backend thành công
- [ ] Dữ liệu trả về đúng format trong `contracts/api.md`
- [ ] Các trường hợp lỗi được xử lý đúng

---

## 📁 Cấu Trúc Thư Mục

```
nova-math-v2/
│
├── .specify/
│   ├── memory/
│   │   └── constitution.md          ← ① Hiến pháp dự án
│   └── templates/
│       ├── contracts-api-template.md     ← Template cho API contracts
│       └── contracts-models-template.md  ← Template cho data models
│
├── specs/                            ← Tất cả tính năng
│   │
│   ├── dang-nhap/                    ← Tính năng 1
│   │   ├── spec.md                   ← ② Đặc tả
│   │   ├── plan.md                   ← ④ Kế hoạch
│   │   ├── tasks.md                  ← ⑤ Danh sách tasks
│   │   └── contracts/                ← ④ HỢP ĐỒNG API
│   │       ├── api.md                ← Định nghĩa API
│   │       └── models.md             ← Định nghĩa Data Models
│   │
│   └── quan-ly-bai-toan/             ← Tính năng 2
│       └── ...
│
├── backend/                          ← ⑦ Code Java Spring Boot
│   └── src/
│
└── frontend/                         ← ⑧ Code Angular
    └── src/
```

---

## ✅ Checklist Trước Khi Code

### Cho mỗi tính năng, đảm bảo đã có:

- [ ] `constitution.md` với các nguyên tắc dự án *(chỉ cần 1 lần)*
- [ ] `spec.md` mô tả đầy đủ tính năng
- [ ] `plan.md` với kế hoạch kỹ thuật
- [ ] **`contracts/api.md`** với đầy đủ thông tin API
- [ ] **`contracts/models.md`** với đầy đủ data models
- [ ] `tasks.md` chia riêng Backend/Frontend
- [ ] GitHub Issues đã được tạo với labels phù hợp

---

## 🔄 Khi Cần Thay Đổi API

Nếu trong quá trình phát triển cần thay đổi API:

1. **CẬP NHẬT `contracts/api.md` TRƯỚC**
2. Thông báo cho cả team (BE + FE)
3. Backend cập nhật code theo contracts mới
4. Frontend cập nhật code theo contracts mới

**KHÔNG ĐƯỢC** tự ý thay đổi API mà không cập nhật contracts!

---

## 📚 Các Workflow Có Sẵn

| Workflow | Mô tả |
|----------|-------|
| `/spec-driven-dev-constitution` | Tạo hiến pháp dự án |
| `/spec-driven-dev-specify` | Viết spec tính năng |
| `/spec-driven-dev-clarify` | Làm rõ các điểm mơ hồ |
| `/spec-driven-dev-analyze` | Phân tích tính nhất quán |
| `/spec-driven-dev-plan` | Lập kế hoạch + tạo contracts |
| `/spec-driven-dev-tasks` | Chia tasks |
| `/spec-driven-dev-taskstoissues` | Đẩy lên GitHub Issues |
| `/spec-driven-dev-implement` | Triển khai từng task |

---

## 💡 Mẹo

1. **Contracts là nguồn chân lý duy nhất** - Cả BE và FE đều phải tuân theo
2. **Viết contracts chi tiết** - Càng chi tiết, càng ít hiểu lầm
3. **Thêm ví dụ JSON cụ thể** - Giúp dev dễ hiểu hơn
4. **Review contracts trước khi code** - Phát hiện vấn đề sớm, sửa rẻ hơn

---

*Cập nhật lần cuối: 2026-02-01*
