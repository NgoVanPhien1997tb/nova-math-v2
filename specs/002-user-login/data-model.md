# Data Model: [002-user-login]

## Entities

### User
> Bảng `users`

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| `id` | Long | PK, Auto-increment | Primary Key |
| `username` | String | Unique, Not Null | Tên đăng nhập |
| `password` | String | Not Null | Password Hash (BCrypt) |
| `role` | Enum | Not Null | User Role (STUDENT, TEACHER, ADMIN) |
| `fullName` | String | Not Null | Tên hiển thị |
| `failedAttempts` | Integer | Default 0 | Số lần login sai |
| `lockTime` | DateTime | Nullable | Thời gian bị khóa (nếu có) |
| `status` | Enum | Not Null | ACTIVE, LOCKED |

### Role (Enum)
- STUDENT
- TEACHER
- ADMIN

## Validation Rules
- **Username**: Min 3, Max 50, Alphanumeric.
- **Password**: Min 6 chars (Input), Hash (Storage).

## State Transitions (User Status)
- `ACTIVE` → `LOCKED`: Khi `failedAttempts` >= 5.
- `LOCKED` → `ACTIVE`: Khi `lockTime` + 30 phút < `now` HOẶC Admin unlock.
