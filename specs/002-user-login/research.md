# Research Report: [002-user-login]

## Trạng thái: HOÀN THÀNH

## 1. Các mục Cần Làm Rõ (Unknowns)

| Mục | Ngữ cảnh | Kết luận |
|-----|----------|----------|
| **JWT Library** | Backend Spring Boot 3.x | Sử dụng `io.jsonwebtoken:jjwt-api` (standard). |
| **Password Hashing** | Security Requirement | Sử dụng `BCryptPasswordEncoder` (Spring Security Standard). |
| **FE State** | Angular Auth State | Sử dụng `BehaviorSubject` trong `AuthService` để lưu trạng thái đăng nhập + `localStorage` để persist token. |

## 2. Các Quyết định Kỹ thuật (Tech Decisions)

### 2.1 Backend Security Architecture
- **Quyết định**: Sử dụng Spring Security Chain với JWT Filter.
- **Lý do**: Chuẩn công nghiệp, stateless, phù hợp với kiến trúc phân tán sau này.
- **Lựa chọn thay thế**: Session-based (Bị từ chối vì không stateless, khó scale).

### 2.2 Frontend Auth Flow
- **Quyết định**: Login → Lưu Token (localStorage) → Redirect (Router).
- **Lý do**: UX nhanh, đơn giản.
- **Lựa chọn thay thế**: Cookie HttpOnly cho Token (Bị từ chối vì phức tạp hơn với CORs ở giai đoạn này, có thể nâng cấp sau).

## 3. Best Practices & Patterns
- **User Enumeration Prevention**: Trả về thông báo lỗi chung chung (Generic error messages).
- **Brute Force Protection**: Implement counting login attempts tại Service Layer, lock tại Database.
- **Input Sanitization**: Sử dụng DTO Validation (@Valid, @NotNull).

## 4. Kết luận
Tech stack đã rõ ràng. Không có blocker nào. Chuyển sang Phase 1: Design.
