# Kế hoạch Triển khai: Đăng nhập (User Login)

**Nhánh**: `002-user-login` | **Ngày**: 2026-02-01 | **Spec**: [specs/002-user-login/spec.md](spec.md)
**Đầu vào**: Đặc tả tính năng từ `specs/002-user-login/spec.md`

## Tóm tắt

Tính năng Đăng nhập cho phép Học sinh, Giáo viên và Admin truy cập vào hệ thống. Sử dụng cơ chế xác thực JWT, bảo mật mật khẩu bằng Hash, và khóa tài khoản sau 5 lần sai. Giao diện Responsive trên Angular và Backend Spring Boot.

## Ngữ cảnh Kỹ thuật

**Ngôn ngữ/Phiên bản**: Backend: Java 17+; Frontend: TypeScript 5+, Angular 17+
**Các Dependencies chính**: 
- BE: Spring Boot 3.x, Spring Security 6.x, JJWT (hoặc tương đương), Spring Data JPA
- FE: Angular Router, RxJS, Angular Reactive Forms
**Lưu trữ**: PostgreSQL 15+ (Bảng `users`, `roles`)
**Kiểm thử**: 
- BE: JUnit 5, Mockito
- FE: Jasmine, Karma
**Nền tảng Mục tiêu**: Web App (Respnsive Desktop/Mobile)
**Loại Dự án**: Web Application (Backend API + Frontend SPA)
**Mục tiêu Hiệu năng**: Login < 2s, chịu tải 100 concurrent requests.
**Các Ràng buộc**: 
- Mật khẩu hashed (BCrypt/Argon2)
- JWT expiration 30 phút
- Khóa tài khoản sau 5 lần sai (30 phút hoặc Admin mở)
**Quy mô/Phạm vi**: 3 roles, ~1000 users tiềm năng.

## Kiểm tra Hiến pháp

*CỔNG (GATE): Phải vượt qua trước khi nghiên cứu Phase 0. Kiểm tra lại sau khi thiết kế Phase 1.*

- **[MUST] Hiểu biết Dự án**: Đã đọc INDEX.md và tom-tat-bao-cao.md.
- **[MUST] Kiến trúc Phân tầng**: Tuân thủ Controller-Service-Repository (BE) và Component-Service (FE).
- **[MUST] API-First**: Sẽ tạo API Contracts trước khi code.
- **[MUST] Kiểm thử Đa tầng**: Sẽ viết Unit & Integration Tests.
- **[MUST] Bảo mật**: JWT, Password Hashing, Input Validation.
- **[MUST] Responsive**: Angular Mobile-first design.

## Cấu trúc Dự án

### Tài liệu (tính năng này)

```text
specs/002-user-login/
├── plan.md              # File này
├── research.md          # Output Phase 0: Xác nhận Tech Stack & Security Specs
├── data-model.md        # Output Phase 1: User & Token models
├── quickstart.md        # Output Phase 1: Hướng dẫn chạy feature
├── contracts/           # Output Phase 1: OpenAPI & Models
└── tasks.md             # Output Phase 2: Tasks triển khai
```

### Mã nguồn (gốc repository)

```text
# Tùy chọn 2: Ứng dụng Web

backend/src/main/java/com/novamath/
├── controller/auth/       # LoginController
├── service/auth/          # AuthService, TokenService
├── repository/            # UserRepository
├── entity/                # User, Role
├── dto/auth/              # LoginRequest, LoginResponse
└── security/              # JwtFilter, SecurityConfig

frontend/src/app/
├── features/auth/         # Auth Module (Lazy loaded)
│   ├── login/             # LoginComponent
│   └── services/          # AuthService
├── core/
│   ├── guards/            # AuthGuard
│   └── interceptors/      # JwtInterceptor
```

**Quyết định Cấu trúc**: Sử dụng quy chuẩn "Ứng dụng Web" với Backend Spring Boot và Frontend Angular như đã định nghĩa trong Hiến pháp.

## Theo dõi Độ phức tạp

> Không có vi phạm Hiến pháp nào cần giải trình.

