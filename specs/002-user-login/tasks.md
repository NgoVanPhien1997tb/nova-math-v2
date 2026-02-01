# Tasks: Đăng nhập (User Login)

**Spec**: [specs/002-user-login/spec.md](spec.md)
**Plan**: [specs/002-user-login/plan.md](plan.md)
**Status**: Generated

## Phase 1: Setup
> Khởi tạo môi trường và cấu trúc dự án.

- [ ] [T001] [Setup] Cập nhật `pom.xml` thêm dependencies: Spring Security, JJWT `backend/pom.xml` [Issue #1 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/1)
- [ ] [T002] [Setup] [P] Cập nhật `application.properties` với JWT secret và expiration `backend/src/main/resources/application.properties` [Issue #2 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/2)
- [ ] [T003] [Setup] [P] Tạo Flyway migration script `V1__init_users_table.sql` cho bảng users và roles `backend/src/main/resources/db/migration/V1__init_users_table.sql` [Issue #3 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/3)
- [ ] [T004] [Setup] [P] Generate Auth Module trong Angular (lazy loaded) `frontend/src/app/features/auth/` [Issue #1 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/1)

## Phase 2: Foundational
> Các thành phần cốt lõi chặn đường cho mọi User Stories.

- [ ] [T005] [Foundation] Tạo Enums `UserRole`, `UserStatus` `backend/src/main/java/com/novamath/entity/UserRole.java` [Issue #4 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/4)
- [ ] [T006] [Foundation] Tạo Entity `User` khớp với data-model.md `backend/src/main/java/com/novamath/entity/User.java` [Issue #5 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/5)
- [ ] [T007] [Foundation] Tạo `UserRepository` `backend/src/main/java/com/novamath/repository/UserRepository.java` [Issue #6 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/6)
- [ ] [T008] [Foundation] Cấu hình `SecurityConfig`: PasswordEncoder (BCrypt), SecurityFilterChain `backend/src/main/java/com/novamath/config/SecurityConfig.java` [Issue #7 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/7)
- [ ] [T009] [Foundation] Implement `JwtUtil` để generate và validate token `backend/src/main/java/com/novamath/security/JwtUtil.java` [Issue #8 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/8)
- [ ] [T010] [Foundation] [P] Tạo Frontend Data Models (Interfaces): `User`, `LoginRequest`, `LoginResponse` `frontend/src/app/features/auth/models/auth.models.ts` [Issue #2 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/2)
- [ ] [T011] [Foundation] [P] Tạo `AuthService` (Angular) với các phương thức cơ bản (login placeholder, token storage) `frontend/src/app/features/auth/services/auth.service.ts` [Issue #3 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/3)

## Phase 3: User Story 1 - Đăng nhập Thành công (P1)
> [US1] Người dùng đăng nhập với thông tin hợp lệ.

**Mục tiêu**: API Login hoạt động, trả về JWT. Frontend gọi được API và redirect.
**Test Độc lập**: Postman gọi POST /auth/login -> 200 OK + Token.

- [ ] [T012] [US1] Backend: Tạo DTOs `LoginRequest`, `LoginResponse` `backend/src/main/java/com/novamath/dto/auth/` [Issue #9 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/9)
- [ ] [T013] [US1] Backend: Implement `AuthService.login()`: verify password, generate token `backend/src/main/java/com/novamath/service/auth/AuthService.java` [Issue #10 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/10)
- [ ] [T014] [US1] Backend: Implement `AuthController.login()` theo contracts/api.md `backend/src/main/java/com/novamath/controller/auth/AuthController.java` [Issue #11 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/11)
- [ ] [T015] [US1] Frontend: Thiết kế UI `LoginComponent` (HTML/CSS) `frontend/src/app/features/auth/login/login.component.html` [Issue #4 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/4)
- [ ] [T016] [US1] Frontend: Bind logic vào `LoginComponent` (Reactive Forms, call Service) `frontend/src/app/features/auth/login/login.component.ts` [Issue #5 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/5)
- [ ] [T017] [US1] Frontend: Handle success response (Lưu token localStorage, Redirect to Home) `frontend/src/app/features/auth/services/auth.service.ts` [Issue #6 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/6)

## Phase 4: User Story 2 - Xử lý Đăng nhập Thất bại (P1)
> [US2] Hệ thống báo lỗi khi nhập sai.

**Mục tiêu**: API trả về 401 khi sai pass. Frontend hiển thị lỗi.

- [ ] [T018] [US2] Backend: Handle `BadCredentialsException` trong Global Exception Handler, trả về format lỗi chuẩn `backend/src/main/java/com/novamath/exception/GlobalExceptionHandler.java` [Issue #12 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/12)
- [ ] [T019] [US2] Frontend: Hiển thị thông báo lỗi từ API lên `LoginComponent` `frontend/src/app/features/auth/login/login.component.html` [Issue #7 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/7)

## Phase 5: User Story 3 - Khóa Tài khoản (P2)
> [US3] Khóa tài khoản sau 5 lần sai.

**Mục tiêu**: Sau 5 lần sai, account status = LOCKED. Login đúng cũng không được vào.

- [ ] [T020] [US3] Backend: Implement logic đếm `failedAttempts` trong `AuthService` `backend/src/main/java/com/novamath/service/auth/AuthService.java` [Issue #13 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/13)
- [ ] [T021] [US3] Backend: Implement logic `lockUser()` khi attempts >= 5 `backend/src/main/java/com/novamath/service/auth/AuthService.java` [Issue #14 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/14)
- [ ] [T022] [US3] Backend: Implement logic check user status trước khi authenticate `backend/src/main/java/com/novamath/service/auth/AuthService.java` [Issue #15 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/15)
- [ ] [T023] [US3] Backend: Implement auto-unlock logic (check `lockTime` + 30m) `backend/src/main/java/com/novamath/service/auth/AuthService.java` [Issue #16 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/16)

## Final Phase: Polish & Review
> Hoàn thiện và kiểm tra lại toàn bộ tính năng.

- [ ] [T024] [Polish] Manual E2E Test: Login Success, Login Fail, Lockout flow `Manual Test` [Issue #17 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/17)
- [ ] [T025] [Polish] Code Review & Cleanup `Source Code` [Issue #18 (BE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-backend/issues/18) [Issue #8 (FE)](https://github.com/NgoVanPhien1997tb/nova-math-v2-frontend/issues/8)

## Dependencies Graph
1. Setup -> Foundational
2. Foundational -> US1
3. US1 -> US2
4. US2 -> US3

## Parallel Execution Opportunities
- T003 (DB Migration) & T004 (FE Setup) chạy song song với Backend Setup.
- T010, T011 (FE Integration) chạy song song với T005-T009 (BE Foundation).
- T015 (FE UI) chạy song song với T012-T014 (BE API).

## Dependencies Graph
1. Setup -> Foundational
2. Foundational -> US1
3. US1 -> US2
4. US2 -> US3

## Parallel Execution Opportunities
- T003 (DB Migration) & T004 (FE Setup) chạy song song với Backend Setup.
- T010, T011 (FE Integration) chạy song song với T005-T009 (BE Foundation).
- T015 (FE UI) chạy song song với T012-T014 (BE API).
