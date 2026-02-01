# Data Models - [TÊN TÍNH NĂNG]

> **Spec**: [link đến spec.md]  
> **API Contracts**: [link đến contracts/api.md]  
> **Phiên bản**: 1.0.0  
> **Cập nhật lần cuối**: [NGÀY]

---

## Mục Lục

1. [Tổng Quan](#tổng-quan)
2. [Entities (Database Models)](#entities)
3. [DTOs (Data Transfer Objects)](#dtos)
4. [Enums](#enums)
5. [Mapping](#mapping)

---

## Tổng Quan

File này định nghĩa các đối tượng dữ liệu được sử dụng trong tính năng, bao gồm:
- **Entities**: Các bảng trong database
- **DTOs**: Các đối tượng gửi/nhận qua API
- **Enums**: Các giá trị cố định

---

## Entities

### Entity: [TÊN_ENTITY]

> Bảng trong database: `ten_bang`

| Field | Type | Constraints | Mô tả |
|-------|------|-------------|-------|
| `id` | Long | PK, Auto-increment | ID tự động tăng |
| `field1` | String | NOT NULL, Max 255 | Mô tả field 1 |
| `field2` | Integer | NOT NULL, Default 0 | Mô tả field 2 |
| `status` | Enum | NOT NULL | Trạng thái (xem Enums) |
| `createdAt` | DateTime | NOT NULL | Thời gian tạo |
| `updatedAt` | DateTime | NOT NULL | Thời gian cập nhật |
| `createdBy` | Long | FK → users.id | Người tạo |

**Relationships:**
- `createdBy` → Many-to-One với `User`
- `items` → One-to-Many với `Item`

**Indexes:**
- `idx_field1` trên `field1` (để tìm kiếm nhanh)
- `idx_status_created` trên `(status, createdAt)` (để lọc và sắp xếp)

**Ví dụ dữ liệu:**
```json
{
  "id": 1,
  "field1": "Giá trị mẫu",
  "field2": 100,
  "status": "ACTIVE",
  "createdAt": "2026-02-01T10:00:00Z",
  "updatedAt": "2026-02-01T10:00:00Z",
  "createdBy": 5
}
```

---

### Entity: [TÊN_ENTITY_2]

> Bảng trong database: `ten_bang_2`

| Field | Type | Constraints | Mô tả |
|-------|------|-------------|-------|
| ... | ... | ... | ... |

---

## DTOs

### Request DTOs

#### CreateExampleRequest

> Sử dụng khi: Tạo mới item (POST /example)

| Field | Type | Required | Validation | Mô tả |
|-------|------|----------|------------|-------|
| `field1` | string | ✅ | min: 1, max: 255 | Tên của item |
| `field2` | number | ❌ | min: 0, default: 0 | Số lượng |
| `field3` | object | ❌ | - | Thông tin bổ sung |

**Ví dụ:**
```json
{
  "field1": "Tên mới",
  "field2": 50,
  "field3": {
    "key": "value"
  }
}
```

**Validation Rules:**
- `field1`: Không được để trống, tối đa 255 ký tự
- `field2`: Phải là số không âm

---

#### UpdateExampleRequest

> Sử dụng khi: Cập nhật item (PUT /example/{id})

| Field | Type | Required | Validation | Mô tả |
|-------|------|----------|------------|-------|
| `field1` | string | ❌ | max: 255 | Tên mới (nếu cần đổi) |
| `field2` | number | ❌ | min: 0 | Số lượng mới |

---

### Response DTOs

#### ExampleDTO

> Sử dụng khi: Trả về thông tin item

| Field | Type | Nullable | Mô tả |
|-------|------|----------|-------|
| `id` | number | ❌ | ID của item |
| `field1` | string | ❌ | Tên |
| `field2` | number | ❌ | Số lượng |
| `status` | string | ❌ | Trạng thái (enum value) |
| `createdAt` | string | ❌ | ISO 8601 datetime |
| `updatedAt` | string | ❌ | ISO 8601 datetime |
| `createdBy` | UserSummaryDTO | ✅ | Thông tin người tạo |

**Ví dụ:**
```json
{
  "id": 1,
  "field1": "Tên item",
  "field2": 100,
  "status": "ACTIVE",
  "createdAt": "2026-02-01T10:00:00Z",
  "updatedAt": "2026-02-01T10:00:00Z",
  "createdBy": {
    "id": 5,
    "name": "Nguyễn Văn A"
  }
}
```

---

#### ExampleListItemDTO

> Sử dụng khi: Trả về danh sách (phiên bản gọn)

| Field | Type | Nullable | Mô tả |
|-------|------|----------|-------|
| `id` | number | ❌ | ID |
| `field1` | string | ❌ | Tên |
| `status` | string | ❌ | Trạng thái |
| `createdAt` | string | ❌ | Thời gian tạo |

---

#### UserSummaryDTO

> Thông tin tóm tắt của user (dùng để embed trong response khác)

| Field | Type | Nullable | Mô tả |
|-------|------|----------|-------|
| `id` | number | ❌ | ID user |
| `name` | string | ❌ | Tên hiển thị |
| `avatarUrl` | string | ✅ | URL avatar |

---

## Enums

### ExampleStatus

> Trạng thái của Example

| Value | Display Name | Mô tả |
|-------|--------------|-------|
| `DRAFT` | Nháp | Item đang soạn thảo |
| `ACTIVE` | Hoạt động | Item đã được công bố |
| `INACTIVE` | Ngừng hoạt động | Item đã bị ẩn |
| `DELETED` | Đã xóa | Item đã bị xóa (soft delete) |

**Backend (Java):**
```java
public enum ExampleStatus {
    DRAFT,
    ACTIVE,
    INACTIVE,
    DELETED
}
```

**Frontend (TypeScript):**
```typescript
export enum ExampleStatus {
  DRAFT = 'DRAFT',
  ACTIVE = 'ACTIVE',
  INACTIVE = 'INACTIVE',
  DELETED = 'DELETED'
}

export const ExampleStatusDisplay: Record<ExampleStatus, string> = {
  [ExampleStatus.DRAFT]: 'Nháp',
  [ExampleStatus.ACTIVE]: 'Hoạt động',
  [ExampleStatus.INACTIVE]: 'Ngừng hoạt động',
  [ExampleStatus.DELETED]: 'Đã xóa'
};
```

---

### [TÊN_ENUM_KHÁC]

| Value | Display Name | Mô tả |
|-------|--------------|-------|
| ... | ... | ... |

---

## Mapping

### Entity ↔ DTO Mapping

| Entity Field | DTO Field | Transformation |
|--------------|-----------|----------------|
| `id` | `id` | Trực tiếp |
| `field1` | `field1` | Trực tiếp |
| `createdAt` | `createdAt` | Format ISO 8601 |
| `createdBy` (User entity) | `createdBy` (UserSummaryDTO) | Chỉ lấy id, name, avatarUrl |

---

## Ghi Chú Cho Developer

### Backend (Java)

```java
// Entity
@Entity
@Table(name = "examples")
public class Example {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, length = 255)
    private String field1;
    
    @Enumerated(EnumType.STRING)
    private ExampleStatus status;
    
    // ... getters, setters
}

// DTO
public record ExampleDTO(
    Long id,
    String field1,
    Integer field2,
    ExampleStatus status,
    Instant createdAt,
    UserSummaryDTO createdBy
) {}
```

### Frontend (TypeScript)

```typescript
// Interface
export interface Example {
  id: number;
  field1: string;
  field2: number;
  status: ExampleStatus;
  createdAt: string;
  updatedAt: string;
  createdBy?: UserSummary;
}

export interface CreateExampleRequest {
  field1: string;
  field2?: number;
  field3?: Record<string, unknown>;
}

export interface UserSummary {
  id: number;
  name: string;
  avatarUrl?: string;
}
```

---

## Lịch Sử Thay Đổi

| Phiên bản | Ngày | Người thay đổi | Mô tả |
|-----------|------|----------------|-------|
| 1.0.0 | [NGÀY] | [TÊN] | Khởi tạo data models |
