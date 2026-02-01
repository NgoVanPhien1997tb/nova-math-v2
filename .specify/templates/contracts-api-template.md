# API Contracts - [TÊN TÍNH NĂNG]

> **Spec**: [link đến spec.md]  
> **Phiên bản**: 1.0.0  
> **Cập nhật lần cuối**: [NGÀY]  
> **Tác giả**: [TÊN]

---

## Mục Lục

1. [Thông Tin Chung](#thông-tin-chung)
2. [Danh Sách API Endpoints](#danh-sách-api-endpoints)
3. [Chi Tiết Từng API](#chi-tiết-từng-api)
4. [Mã Lỗi Chung](#mã-lỗi-chung)

---

## Thông Tin Chung

### Base URL

| Môi trường | URL |
|------------|-----|
| Development | `http://localhost:8080/api/v1` |
| Staging | `https://staging-api.example.com/api/v1` |
| Production | `https://api.example.com/api/v1` |

### Authentication

<!-- Mô tả cách xác thực, ví dụ: -->
- **Loại**: Bearer Token (JWT)
- **Header**: `Authorization: Bearer {accessToken}`
- **Thời hạn token**: 24 giờ

### Response Format Chuẩn

**Thành công:**
```json
{
  "success": true,
  "data": { ... },
  "message": "Thao tác thành công"
}
```

**Thất bại:**
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Mô tả lỗi cho người dùng",
    "details": { ... }
  }
}
```

---

## Danh Sách API Endpoints

| STT | Method | Endpoint | Mô tả | Auth |
|-----|--------|----------|-------|------|
| 1 | POST | `/example` | Tạo mới ví dụ | ✅ |
| 2 | GET | `/example` | Lấy danh sách | ✅ |
| 3 | GET | `/example/{id}` | Lấy chi tiết | ✅ |
| 4 | PUT | `/example/{id}` | Cập nhật | ✅ |
| 5 | DELETE | `/example/{id}` | Xóa | ✅ |

---

## Chi Tiết Từng API

---

### API 1: [Tên API]

#### Thông tin cơ bản

| Thuộc tính | Giá trị |
|------------|---------|
| **Method** | `POST` |
| **Endpoint** | `/example` |
| **Mô tả** | Tạo mới một ví dụ |
| **Yêu cầu Auth** | ✅ Có |

#### Request

**Headers:**
| Header | Giá trị | Bắt buộc |
|--------|---------|----------|
| Content-Type | `application/json` | ✅ |
| Authorization | `Bearer {token}` | ✅ |

**Body:**
```json
{
  "field1": "string (bắt buộc) - Mô tả field 1",
  "field2": 123,
  "field3": {
    "nestedField": "value"
  }
}
```

**Ví dụ Request:**
```json
{
  "field1": "Giá trị thực tế",
  "field2": 100,
  "field3": {
    "nestedField": "nested value"
  }
}
```

#### Response

**✅ Thành công (200 OK / 201 Created):**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "field1": "Giá trị thực tế",
    "field2": 100,
    "field3": {
      "nestedField": "nested value"
    },
    "createdAt": "2026-02-01T10:00:00Z",
    "updatedAt": "2026-02-01T10:00:00Z"
  },
  "message": "Tạo thành công"
}
```

**❌ Lỗi validation (400 Bad Request):**
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Dữ liệu không hợp lệ",
    "details": {
      "field1": "Field1 không được để trống",
      "field2": "Field2 phải là số dương"
    }
  }
}
```

**❌ Chưa đăng nhập (401 Unauthorized):**
```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Vui lòng đăng nhập để tiếp tục"
  }
}
```

**❌ Không có quyền (403 Forbidden):**
```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "Bạn không có quyền thực hiện thao tác này"
  }
}
```

---

### API 2: [Tên API - Lấy danh sách]

#### Thông tin cơ bản

| Thuộc tính | Giá trị |
|------------|---------|
| **Method** | `GET` |
| **Endpoint** | `/example` |
| **Mô tả** | Lấy danh sách với phân trang |
| **Yêu cầu Auth** | ✅ Có |

#### Request

**Query Parameters:**
| Tham số | Kiểu | Mặc định | Mô tả |
|---------|------|----------|-------|
| page | number | 1 | Số trang (bắt đầu từ 1) |
| limit | number | 10 | Số lượng mỗi trang (tối đa 100) |
| search | string | - | Tìm kiếm theo từ khóa |
| sortBy | string | createdAt | Sắp xếp theo field |
| sortOrder | string | desc | Thứ tự: `asc` hoặc `desc` |

**Ví dụ URL:**
```
GET /example?page=1&limit=10&search=keyword&sortBy=createdAt&sortOrder=desc
```

#### Response

**✅ Thành công (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "field1": "Giá trị 1",
      "createdAt": "2026-02-01T10:00:00Z"
    },
    {
      "id": 2,
      "field1": "Giá trị 2",
      "createdAt": "2026-02-01T09:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "totalItems": 50,
    "totalPages": 5,
    "hasNextPage": true,
    "hasPrevPage": false
  }
}
```

---

### API 3: [Tên API - Lấy chi tiết]

#### Thông tin cơ bản

| Thuộc tính | Giá trị |
|------------|---------|
| **Method** | `GET` |
| **Endpoint** | `/example/{id}` |
| **Mô tả** | Lấy chi tiết một item |
| **Yêu cầu Auth** | ✅ Có |

#### Request

**Path Parameters:**
| Tham số | Kiểu | Mô tả |
|---------|------|-------|
| id | number | ID của item |

**Ví dụ URL:**
```
GET /example/123
```

#### Response

**✅ Thành công (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": 123,
    "field1": "Giá trị",
    "field2": 100,
    "createdAt": "2026-02-01T10:00:00Z",
    "updatedAt": "2026-02-01T10:00:00Z"
  }
}
```

**❌ Không tìm thấy (404 Not Found):**
```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "Không tìm thấy item với ID: 123"
  }
}
```

---

## Mã Lỗi Chung

| Mã lỗi | HTTP Status | Mô tả |
|--------|-------------|-------|
| `VALIDATION_ERROR` | 400 | Dữ liệu đầu vào không hợp lệ |
| `UNAUTHORIZED` | 401 | Chưa đăng nhập hoặc token hết hạn |
| `FORBIDDEN` | 403 | Không có quyền truy cập |
| `NOT_FOUND` | 404 | Không tìm thấy tài nguyên |
| `CONFLICT` | 409 | Xung đột dữ liệu (ví dụ: email đã tồn tại) |
| `INTERNAL_ERROR` | 500 | Lỗi server |

---

## Ghi Chú Cho Developer

### Backend (Java Spring Boot)

```java
// Ví dụ Controller
@RestController
@RequestMapping("/api/v1/example")
public class ExampleController {
    
    @PostMapping
    public ResponseEntity<ApiResponse<ExampleDTO>> create(@RequestBody CreateExampleRequest request) {
        // Implement theo contract ở trên
    }
}
```

### Frontend (Angular)

```typescript
// Ví dụ Service
@Injectable({ providedIn: 'root' })
export class ExampleService {
  private baseUrl = '/api/v1/example';

  create(data: CreateExampleRequest): Observable<ApiResponse<Example>> {
    return this.http.post<ApiResponse<Example>>(this.baseUrl, data);
  }
}

// Interface theo contract
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  message?: string;
  error?: {
    code: string;
    message: string;
    details?: Record<string, string>;
  };
}
```

---

## Lịch Sử Thay Đổi

| Phiên bản | Ngày | Người thay đổi | Mô tả |
|-----------|------|----------------|-------|
| 1.0.0 | [NGÀY] | [TÊN] | Khởi tạo contract |
