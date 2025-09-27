
---

````
# 🍪 Cookie & Session Authentication (Node.js + MongoDB)

## 📌 Giới thiệu
Dự án này minh họa cách xây dựng hệ thống **Authentication** trong Node.js sử dụng:
- `express-session` để quản lý session
- `connect-mongo` để lưu session vào MongoDB
- `bcryptjs` để hash mật khẩu
- Cookie (`connect.sid`) để xác thực người dùng

---

## ⚙️ Cài đặt

### 1. Clone repo
```bash
git clone https://github.com/HaiDang102/cookie_session_auth.git
cd cookie_session_auth
````

### 2. Cài đặt dependencies

```bash
npm install
```

### 3. Chạy MongoDB

Đảm bảo bạn đã cài đặt MongoDB và chạy trên `mongodb://127.0.0.1:27017`.

Dự án sẽ tự động tạo database:

```
sessionAuth
```

và collection `sessions` để lưu cookie session.

### 4. Chạy server

```bash
node app.js
```

Server sẽ chạy tại:

```
http://localhost:3000
```

---

## 🧪 Test với Postman

### 1. Register (Đăng ký)

* **Method:** `POST`
* **URL:** `http://localhost:3000/auth/register`
* **Body (raw JSON):**

```json
{
  "username": "alice",
  "password": "123456"
}
```

✅ Kết quả:

```json
{ "message": "User registered successfully!" }
```

---

### 2. Login (Đăng nhập)

* **Method:** `POST`
* **URL:** `http://localhost:3000/auth/login`
* **Body (raw JSON):**

```json
{
  "username": "alice",
  "password": "123456"
}
```

✅ Kết quả:

```json
{ "message": "Login successful!" }
```

👉 Trong Postman tab **Cookies**, bạn sẽ thấy cookie:

```
connect.sid
```

Đồng thời MongoDB lưu session trong collection `sessions`.

---

### 3. Profile (Route bảo vệ)

* **Method:** `GET`
* **URL:** `http://localhost:3000/auth/profile`

✅ Nếu đã login:

```json
{
  "_id": "67003cbd9a56eaa97e09f14a",
  "username": "alice",
  "__v": 0
}
```

❌ Nếu chưa login hoặc session hết hạn:

```json
{ "error": "Unauthorized" }
```

---

### 4. Logout (Đăng xuất)

* **Method:** `GET`
* **URL:** `http://localhost:3000/auth/logout`

✅ Kết quả:

```json
{ "message": "Logout successful!" }
```

 Lúc này cookie `connect.sid` bị xóa, gọi lại `/auth/profile` sẽ trả `Unauthorized`.

---

##  Cấu trúc thư mục

```
cookie_session_auth/
│── app.js               # Điểm khởi động server
│── routes/
│   └── auth.js          # Các route đăng ký, đăng nhập, profile, logout
│── models/
│   └── User.js          # Mongoose model + hash password
│── package.json
│── README.md
```

---

## 📖 Ghi chú

* Session mặc định hết hạn sau **1 giờ** (`maxAge: 1000 * 60 * 60`).
* Để triển khai production cần bật HTTPS và config `cookie.secure = true`.

---

```

---

