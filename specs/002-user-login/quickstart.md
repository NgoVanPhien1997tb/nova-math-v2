# Quickstart: Đăng nhập (User Login)

## 1. Setup Backend
1. Đảm bảo PostgreSQL đang chạy.
2. Cấu hình `application.properties`:
   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/novamath
   spring.datasource.username=postgres
   spring.datasource.password=password
   jwt.secret=YOUR_VERY_SECURE_SECRET_KEY_MIN_256_BITS
   jwt.expiration=1800000
   ```
3. Chạy Flyway migration để tạo bảng `users`.

## 2. Setup Frontend
1. Chuyển vào folder `frontend`.
2. Chạy `npm install`.
3. Chạy `npm start`.

## 3. Testing
- Import `contracts/api.md` vào Postman.
- Gọi `POST /api/v1/auth/login` với:
  - User: `student1` / `password123`
- Kiểm tra nhận được Token.
