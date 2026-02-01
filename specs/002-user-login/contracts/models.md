# Data Models - Đăng nhập (User Login)

> **Spec**: [../spec.md](../spec.md)  
> **API Contracts**: [api.md](api.md)  

---

## Entities

### Entity: User

> Bảng: `users`

| Field | Type | Constraints | Mô tả |
|-------|------|-------------|-------|
| `id` | Long | PK, Auto-increment | ID user |
| `username` | String | Unique, Not NULL | Tên đăng nhập |
| `password` | String | Not NULL | Mật khẩu (Hashed) |
| `fullName` | String | Not NULL | Tên hiển thị |
| `role` | Enum | Not NULL | UserRole |
| `failedAttempts`| Integer | Default 0 | Số lần sai pass |
| `lockTime` | DateTime | Nullable | Thời gian bị khóa |
| `status` | Enum | Not NULL | UserStatus |

---

## DTOs

### LoginRequest

| Field | Type | Required | Validation |
|-------|------|----------|------------|
| `username` | String | ✅ | NotBlank |
| `password` | String | ✅ | NotBlank |

### LoginResponse

| Field | Type | Mô tả |
|-------|------|-------|
| `accessToken` | String | JWT Token |
| `expiresIn` | Long | Thời gian hết hạn (giây) |
| `user` | UserDTO | Thông tin user |

### UserDTO

| Field | Type | Mô tả |
|-------|------|-------|
| `id` | Long | ID |
| `username` | String | Tên đăng nhập |
| `fullName` | String | Tên hiển thị |
| `role` | UserRole | Role |

---

## Enums

### UserRole
- `STUDENT`
- `TEACHER`
- `ADMIN`

### UserStatus
- `ACTIVE`
- `LOCKED`
- `INACTIVE`
