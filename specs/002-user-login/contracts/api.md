# API Contracts - Đăng nhập (User Login)

> **Spec**: [../spec.md](../spec.md)  
> **Phiên bản**: 1.0.0  
> **Cập nhật lần cuối**: 2026-02-01  

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

### Authentication

- **Public Endpoints**: `/auth/login` (Không yêu cầu token)
- **Protected Endpoints**: Yêu cầu `Authorization: Bearer {accessToken}`

---

## Danh Sách API Endpoints

| STT | Method | Endpoint | Mô tả | Auth |
|-----|--------|----------|-------|------|
| 1 | POST | `/auth/login` | Đăng nhập vào hệ thống | ❌ Public |

---

## Chi Tiết Từng API

### API 1: Đăng nhập

#### Thông tin cơ bản

| Thuộc tính | Giá trị |
|------------|---------|
| **Method** | `POST` |
| **Endpoint** | `/auth/login` |
| **Mô tả** | Xác thực user và trả về JWT token |

#### Request

**Headers:**
| Header | Giá trị | Bắt buộc |
|--------|---------|----------|
| Content-Type | `application/json` | ✅ |

**Body:**
```json
{
  "username": "student1",
  "password": "password123"
}
```

#### Response

**✅ Thành công (200 OK):**
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "expiresIn": 1800,
    "user": {
      "id": 1,
      "username": "student1",
      "fullName": "Nguyen Van A",
      "role": "STUDENT"
    }
  },
  "message": "Đăng nhập thành công"
}
```

**❌ Đăng nhập thất bại (401 Unauthorized):**
```json
{
  "success": false,
  "error": {
    "code": "AUTH_FAILED",
    "message": "Tên tài khoản hoặc mật khẩu không đúng"
  }
}
```

**❌ Tài khoản bị khóa (403 Forbidden):**
```json
{
  "success": false,
  "error": {
    "code": "ACCOUNT_LOCKED",
    "message": "Tài khoản đã bị khóa. Vui lòng thử lại sau 30 phút."
  }
}
```

---

## Mã Lỗi Chung

| Mã lỗi | HTTP Status | Mô tả |
|--------|-------------|-------|
| `AUTH_FAILED` | 401 | Sai username/password |
| `ACCOUNT_LOCKED` | 403 | Tài khoản bị khóa do nhập sai quá 5 lần |
| `VALIDATION_ERROR` | 400 | Input không hợp lệ |
