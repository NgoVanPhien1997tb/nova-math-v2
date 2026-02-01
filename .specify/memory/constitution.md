<!--
=============================================================================
BÁO CÁO TÁC ĐỘNG ĐỒNG BỘ (Sync Impact Report)
=============================================================================
Thay đổi phiên bản: 1.0.0 → 1.1.0 (Thêm nguyên tắc Hiểu biết Dự án)

Các nguyên tắc đã sửa đổi:
  - Không có

Các nguyên tắc đã thêm:
  0. Hiểu biết Dự án (Project Understanding) - ĐIỀU KIỆN TIÊN QUYẾT

Các phần đã thêm:
  - Không có

Các phần đã xóa:
  - Không có

Các template yêu cầu cập nhật:
  ✅ .specify/templates/plan-template.md (không cần cập nhật)
  ✅ .specify/templates/spec-template.md (không cần cập nhật)
  ✅ .specify/templates/tasks-template.md (không cần cập nhật)

TODO: Không có
=============================================================================
-->

# Hiến pháp Nova Math v2

## Các Nguyên Tắc Cốt Lõi

### 0. Hiểu biết Dự án (Project Understanding) - ĐIỀU KIỆN TIÊN QUYẾT ⚠️

Trước khi bắt đầu BẤT KỲ công việc nào trong dự án, **PHẢI** đọc và hiểu các tài liệu tổng quan sau:

**Tài liệu BẮT BUỘC phải đọc:**
- 📋 `documents/INDEX.md` - Mục lục và cấu trúc tài liệu dự án
- 📖 `documents/tom-tat-bao-cao.md` - Tóm tắt báo cáo tổng quan về dự án

**Yêu cầu:**
- **PHẢI** đọc cả hai file trước khi thực hiện bất kỳ thay đổi code nào
- **PHẢI** hiểu mục tiêu, phạm vi, và ngữ cảnh của dự án
- **PHẢI** tham chiếu lại các tài liệu này khi có câu hỏi về business logic
- **NÊN** cập nhật tài liệu nếu phát hiện thông tin lỗi thời hoặc thiếu sót

**Lý do**: Hiểu biết tổng quan về dự án giúp đưa ra quyết định thiết kế đúng đắn, tránh implement sai hướng, và đảm bảo tính nhất quán với vision của dự án.

---

### I. Kiến trúc Phân tầng (Layered Architecture)

Mọi module code **PHẢI** tuân thủ kiến trúc phân tầng rõ ràng:

**Backend (Spring Boot):**
- **Controller Layer**: Xử lý HTTP requests, validation đầu vào, không chứa business logic
- **Service Layer**: Chứa toàn bộ business logic, orchestration giữa các repository
- **Repository Layer**: Chỉ chịu trách nhiệm truy cập dữ liệu, sử dụng Spring Data JPA
- **Entity/Model Layer**: Định nghĩa domain models và DTO (Data Transfer Objects)

**Frontend (Angular):**
- **Component Layer**: Chỉ chịu trách nhiệm UI và user interactions
- **Service Layer**: Xử lý business logic, gọi API, quản lý state
- **Model Layer**: Định nghĩa interfaces và types cho dữ liệu
- **Module Layer**: Tổ chức feature modules độc lập, lazy-loaded

**Lý do**: Phân tách rõ ràng giúp code dễ bảo trì, test, và mở rộng. Mỗi layer chỉ giao tiếp với layer liền kề.

---

### II. API-First & Contract-Driven Development

Mọi tính năng liên quan đến giao tiếp Backend-Frontend **PHẢI** bắt đầu bằng việc định nghĩa API contract:

- **PHẢI** viết OpenAPI/Swagger specification trước khi implement
- **PHẢI** sử dụng DTOs riêng biệt cho request/response, KHÔNG expose trực tiếp entity
- **PHẢI** tuân thủ RESTful conventions:
  - `GET` cho đọc dữ liệu
  - `POST` cho tạo mới
  - `PUT/PATCH` cho cập nhật
  - `DELETE` cho xóa
- **PHẢI** sử dụng HTTP status codes chuẩn và response format nhất quán
- **PHẢI** versioning API khi có breaking changes (ví dụ: `/api/v1/`, `/api/v2/`)

**Lý do**: API contract là nguồn sự thật duy nhất (single source of truth) cho giao tiếp giữa frontend và backend. Điều này cho phép development song song và giảm thiểu integration issues.

---

### III. Kiểm thử Đa tầng (Multi-Layer Testing)

Mọi code production **PHẢI** có test coverage phù hợp:

**Backend Requirements:**
- **Unit Tests**: Sử dụng JUnit 5 + Mockito cho service layer
- **Integration Tests**: Sử dụng @SpringBootTest với TestContainers cho database
- **Contract Tests**: Sử dụng Spring Cloud Contract hoặc tương đương cho API
- Code coverage tối thiểu: 70% cho critical paths

**Frontend Requirements:**
- **Unit Tests**: Sử dụng Jasmine + Karma cho components và services
- **Component Tests**: Test isolated components với mock dependencies
- **E2E Tests**: Sử dụng Cypress hoặc Playwright cho critical user journeys
- Code coverage tối thiểu: 60% cho components, 80% cho services

**Lý do**: Testing đa tầng đảm bảo chất lượng code và confidence khi refactoring hoặc thêm tính năng mới.

---

### IV. Bảo mật Toàn diện (Security First)

Bảo mật **PHẢI** được tích hợp từ đầu, không phải afterthought:

**Authentication & Authorization:**
- **PHẢI** sử dụng JWT (JSON Web Tokens) cho stateless authentication
- **PHẢI** implement Spring Security cho backend
- **PHẢI** sử dụng Angular Guards cho route protection
- **PHẢI** refresh tokens tự động và xử lý token expiration gracefully

**Data Protection:**
- **PHẢI** validate và sanitize TẤT CẢ input từ client
- **PHẢI** sử dụng parameterized queries, KHÔNG concatenate SQL
- **PHẢI** encrypt sensitive data at rest và in transit (HTTPS only)
- **PHẢI** tuân thủ OWASP Top 10 security practices

**CORS & Headers:**
- **PHẢI** cấu hình CORS chặt chẽ, chỉ allow origins cần thiết
- **PHẢI** sử dụng security headers: CSP, X-Frame-Options, X-Content-Type-Options

**Lý do**: Security breaches có thể gây thiệt hại nghiêm trọng. Prevention tốt hơn cure.

---

### V. Reactive & Responsive Design

Ứng dụng **PHẢI** reactive và responsive:

**Backend (Reactive Streams):**
- **NÊN** sử dụng WebFlux cho high-concurrency endpoints (optional)
- **PHẢI** xử lý async operations properly với CompletableFuture hoặc Reactor
- **PHẢI** implement proper backpressure handling cho streaming data

**Frontend (RxJS & Responsive):**
- **PHẢI** sử dụng RxJS Observables cho async operations
- **PHẢI** unsubscribe đúng cách để tránh memory leaks (sử dụng takeUntil, async pipe)
- **PHẢI** implement responsive design mobile-first
- **PHẢI** hỗ trợ các breakpoints: mobile (< 768px), tablet (768-1024px), desktop (> 1024px)
- **PHẢI** đảm bảo WCAG 2.1 AA accessibility compliance

**Lý do**: Reactive patterns cải thiện performance và user experience. Responsive design đảm bảo ứng dụng hoạt động trên mọi thiết bị.

---

### VI. Quản lý State & Dependency Injection

State management **PHẢI** được thực hiện có hệ thống:

**Backend (Spring IoC):**
- **PHẢI** sử dụng constructor injection, KHÔNG field injection
- **PHẢI** tuân thủ single responsibility principle cho mỗi bean
- **PHẢI** sử dụng proper bean scopes (singleton, prototype, request, session)
- **PHẢI** avoid circular dependencies

**Frontend (Angular DI & State):**
- **PHẢI** sử dụng Angular DI system properly
- **PHẢI** tách biệt component state và application state
- **NÊN** sử dụng NgRx hoặc signals cho complex state management
- **PHẢI** implement OnPush change detection cho performance-critical components
- **PHẢI** sử dụng BehaviorSubject cho shared state trong services

**Lý do**: Proper state management giúp ứng dụng predictable, debuggable, và testable.

---

### VII. Khả năng Quan sát (Observability)

Ứng dụng **PHẢI** có khả năng quan sát cao:

**Logging:**
- **PHẢI** sử dụng structured logging (JSON format) với SLF4J + Logback
- **PHẢI** log ở mức phù hợp: ERROR, WARN, INFO, DEBUG
- **PHẢI** include correlation IDs để trace requests across services
- **KHÔNG** log sensitive data (passwords, tokens, PII)

**Metrics & Monitoring:**
- **PHẢI** expose health endpoints: `/actuator/health`, `/actuator/info`
- **NÊN** sử dụng Micrometer cho custom metrics
- **PHẢI** monitor key performance indicators: response time, error rate, throughput

**Error Handling:**
- **PHẢI** sử dụng global exception handlers (@ControllerAdvice trong Spring)
- **PHẢI** sử dụng Angular ErrorHandler cho global error handling
- **PHẢI** hiển thị user-friendly error messages, log technical details

**Lý do**: Observability cho phép nhanh chóng identify và resolve issues trong production.

---

### VIII. Phiên bản & Tương thích Ngược (Versioning)

Phiên bản **PHẢI** tuân thủ Semantic Versioning (MAJOR.MINOR.PATCH):

**API Versioning:**
- **MAJOR**: Breaking changes - client code cần cập nhật
- **MINOR**: Backward-compatible features mới
- **PATCH**: Backward-compatible bug fixes

**Database Migrations:**
- **PHẢI** sử dụng Flyway hoặc Liquibase cho database migrations
- **PHẢI** viết reversible migrations khi có thể
- **KHÔNG** modify đã-deployed migrations, chỉ thêm mới

**Dependency Management:**
- **PHẢI** lock dependency versions trong pom.xml và package.json
- **PHẢI** review và update dependencies định kỳ (ít nhất monthly)
- **PHẢI** có strategy cho security patches

**Lý do**: Proper versioning đảm bảo deployments an toàn và rollback khả thi.

---

## Tiêu chuẩn Kỹ thuật

### Tech Stack Bắt buộc

| Layer | Technology | Version | Notes |
|-------|------------|---------|-------|
| Backend Runtime | Java | 17+ (LTS) | Sử dụng modern Java features |
| Backend Framework | Spring Boot | 3.x | Với Spring Security, Spring Data JPA |
| Database | PostgreSQL | 15+ | Primary database |
| API Docs | OpenAPI/Swagger | 3.0 | springdoc-openapi |
| Build Tool | Maven | 3.9+ | Hoặc Gradle 8+ |
| Frontend Runtime | Node.js | 18+ (LTS) | Cho development |
| Frontend Framework | Angular | 17+ | Standalone components preferred |
| State Management | RxJS / Signals | Latest | Built-in với Angular |
| CSS Framework | SCSS | - | Với BEM naming convention |
| Package Manager | npm | 9+ | Lock file required |

### Cấu trúc Dự án Chuẩn

```text
nova-math-v2/
├── backend/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/novamath/
│   │   │   │   ├── config/           # Spring configurations
│   │   │   │   ├── controller/       # REST controllers
│   │   │   │   ├── service/          # Business logic
│   │   │   │   ├── repository/       # Data access
│   │   │   │   ├── entity/           # JPA entities
│   │   │   │   ├── dto/              # Request/Response DTOs
│   │   │   │   ├── exception/        # Custom exceptions
│   │   │   │   └── security/         # Security configurations
│   │   │   └── resources/
│   │   │       ├── application.yml
│   │   │       └── db/migration/     # Flyway migrations
│   │   └── test/
│   │       └── java/com/novamath/
│   │           ├── unit/
│   │           ├── integration/
│   │           └── contract/
│   └── pom.xml
│
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── core/                 # Singleton services, guards
│   │   │   ├── shared/               # Shared components, pipes, directives
│   │   │   ├── features/             # Feature modules (lazy-loaded)
│   │   │   └── models/               # Interfaces and types
│   │   ├── assets/
│   │   ├── styles/                   # Global SCSS
│   │   └── environments/
│   ├── angular.json
│   └── package.json
│
├── docs/                             # Project documentation
├── specs/                            # Feature specifications
└── .specify/                         # Spec-driven development configs
```

---

## Quy trình Phát triển

### Git Workflow

- **PHẢI** sử dụng feature branches với naming convention: `feature/###-short-description`
- **PHẢI** viết commit messages theo Conventional Commits format
- **PHẢI** squash commits trước khi merge vào main
- **PHẢI** có ít nhất 1 code review approval trước khi merge
- **PHẢI** pass tất cả CI checks trước khi merge

### Code Review Checklist

- [ ] Code tuân thủ các nguyên tắc trong hiến pháp này
- [ ] Tests đầy đủ và pass
- [ ] Không có security vulnerabilities
- [ ] Performance được xem xét
- [ ] Documentation được cập nhật nếu cần
- [ ] No console.log hoặc System.out.println trong production code

### Definition of Done

Một tính năng chỉ được coi là "Done" khi:

1. ✅ Code được implement và pass tất cả tests
2. ✅ Code review approved
3. ✅ API documentation được cập nhật (nếu applicable)
4. ✅ User documentation được cập nhật (nếu applicable)
5. ✅ CI/CD pipeline pass
6. ✅ Feature được demo và accepted bởi stakeholder

---

## Quản trị

### Thẩm quyền của Hiến pháp

Hiến pháp này là tài liệu quản trị cao nhất cho dự án Nova Math v2:

- **Hiến pháp THAY THẾ** tất cả các hướng dẫn mâu thuẫn khác
- Mọi code mới **PHẢI** tuân thủ các nguyên tắc được định nghĩa ở đây
- Code hiện tại **NÊN** được refactor để tuân thủ khi có cơ hội

### Quy trình Sửa đổi

Để sửa đổi hiến pháp này:

1. Tạo proposal với lý do thay đổi
2. Review bởi team lead hoặc architect
3. Thảo luận và đạt consensus
4. Cập nhật hiến pháp với version mới
5. Thông báo cho team về thay đổi
6. Cập nhật các template phụ thuộc nếu cần

### Compliance Review

- Mọi Pull Request **PHẢI** được kiểm tra tuân thủ hiến pháp
- Violations **PHẢI** được ghi nhận và sửa trước khi merge
- Exceptions **PHẢI** được documented với lý do rõ ràng

---

**Phiên bản**: 1.1.0 | **Thông qua**: 2026-02-01 | **Sửa đổi lần cuối**: 2026-02-01
