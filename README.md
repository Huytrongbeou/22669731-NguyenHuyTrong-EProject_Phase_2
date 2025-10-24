# Hệ Thống E-Commerce Microservices

## 📋 Mô Tả Dự Án

Đây là một hệ thống thương mại điện tử (E-Commerce) được xây dựng theo kiến trúc microservices, sử dụng Node.js, MongoDB, RabbitMQ và Docker. Hệ thống bao gồm 4 services chính:

- **Auth Service**: Xác thực và quản lý người dùng
- **Product Service**: Quản lý sản phẩm và xử lý đơn hàng
- **Order Service**: Xử lý và lưu trữ đơn hàng
- **API Gateway**: Điểm truy cập trung tâm cho tất cả các services

## 🏗️ Kiến Trúc Hệ Thống

```
┌─────────────┐
│   Client    │
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│  API Gateway    │
│   Port: 8080    │
└────────┬────────┘
         │
    ┌────┴────┬────────────┬─────────┐
    ▼         ▼            ▼         ▼
┌────────┐ ┌──────────┐ ┌──────┐ ┌──────────┐
│ Auth   │ │ Product  │ │Order │ │ MongoDB  │
│ :3000  │ │  :3001   │ │:3002 │ │          │
└────────┘ └─────┬────┘ └──┬───┘ └──────────┘
                 │         │
                 └────┬────┘
                      ▼
                ┌──────────┐
                │ RabbitMQ │
                └──────────┘
```

## 🚀 Công Nghệ Sử Dụng

- **Backend**: Node.js, Express.js
- **Database**: MongoDB
- **Message Broker**: RabbitMQ
- **Authentication**: JWT (JSON Web Token)
- **Containerization**: Docker, Docker Compose
- **Testing**: Mocha, Chai
- **CI/CD**: GitHub Actions

## 📁 Cấu Trúc Thư Mục

```
.
├── api-gateway/          # API Gateway service
├── auth/                 # Authentication service
│   ├── src/
│   │   ├── config/      # Cấu hình
│   │   ├── controllers/ # Controllers
│   │   ├── middlewares/ # Middlewares
│   │   ├── models/      # Database models
│   │   ├── repositories/# Data access layer
│   │   ├── routes/      # API routes
│   │   ├── services/    # Business logic
│   │   └── test/        # Unit tests
│   ├── Dockerfile
│   └── package.json
├── product/              # Product service
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── repositories/
│   │   ├── routes/
│   │   ├── services/
│   │   ├── test/
│   │   └── utils/       # Utilities (RabbitMQ, Auth)
│   ├── Dockerfile
│   └── package.json
├── order/                # Order service
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── repositories/
│   │   ├── routes/
│   │   ├── services/
│   │   └── utils/
│   ├── Dockerfile
│   └── package.json
├── docker-compose.yml    # Docker Compose configuration
├── .github/
│   └── workflows/
│       └── test.yml      # CI/CD pipeline
└── README.md
```

## ⚙️ Cài Đặt và Chạy Dự Án

### Yêu Cầu Hệ Thống

- Docker Desktop
- Docker Compose
- Node.js 18+ (nếu chạy local)
- Git

### 1. Clone Repository

```bash
git clone <repository-url>
cd Docker_Test_V4
```

### 2. Tạo File .env

Tạo file `.env` trong mỗi thư mục service:

**auth/.env:**
```env
MONGODB_AUTH_URI=mongodb://mongodb:27017/auth
JWT_SECRET=your-secret-key-here
NODE_ENV=development
```

**product/.env:**
```env
MONGODB_PRODUCT_URI=mongodb://mongodb:27017/product
JWT_SECRET=your-secret-key-here
NODE_ENV=development
RABBITMQ_URL=amqp://rabbitmq:5672
```

**order/.env:**
```env
MONGODB_ORDER_URI=mongodb://mongodb:27017/order
JWT_SECRET=your-secret-key-here
NODE_ENV=development
RABBITMQ_URL=amqp://rabbitmq:5672
```

### 3. Chạy Với Docker Compose

```bash
# Build và start tất cả services
docker-compose up --build -d

# Xem logs
docker-compose logs -f

# Dừng tất cả services
docker-compose down

# Dừng và xóa volumes
docker-compose down -v
```

### 4. Kiểm Tra Services

- Auth Service: http://localhost:3000
- Product Service: http://localhost:3001
- Order Service: http://localhost:3002
- API Gateway: http://localhost:8080
- RabbitMQ Management: http://localhost:15672 (user: guest, pass: guest)

## 🧪 Testing

### Chạy Tests Locally

```bash
# Test Auth Service
cd auth
npm install
npm test

# Test Product Service
cd product
npm install
npm test
```

### CI/CD với GitHub Actions

Project sử dụng GitHub Actions để tự động chạy tests và build Docker images khi push code.

Workflow bao gồm:
1. **test-auth**: Chạy unit tests cho Auth service
2. **test-product**: Chạy unit tests cho Product service
3. **build-and-push**: Build và push Docker images lên Docker Hub

## 📝 API Documentation

### Auth Service (Port 3000)

#### 1. Đăng Ký User
```http
POST /register
Content-Type: application/json

{
  "username": "testuser",
  "password": "password123"
}
```

**Response (201):**
```json
{
  "_id": "...",
  "username": "testuser"
}
```

#### 2. Đăng Nhập
```http
POST /login
Content-Type: application/json

{
  "username": "testuser",
  "password": "password123"
}
```

**Response (200):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### 3. Dashboard (Protected)
```http
GET /dashboard
Authorization: Bearer <token>
```

**Response (200):**
```json
{
  "message": "Welcome to dashboard"
}
```

### Product Service (Port 3001)

#### 1. Tạo Sản Phẩm
```http
POST /
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Laptop Gaming",
  "description": "Laptop chơi game cao cấp",
  "price": 25000000
}
```

**Response (201):**
```json
{
  "_id": "...",
  "name": "Laptop Gaming",
  "description": "Laptop chơi game cao cấp",
  "price": 25000000
}
```

#### 2. Lấy Tất Cả Sản Phẩm
```http
GET /
Authorization: Bearer <token>
```

**Response (200):**
```json
[
  {
    "_id": "...",
    "name": "Laptop Gaming",
    "description": "Laptop chơi game cao cấp",
    "price": 25000000
  }
]
```

#### 3. Lấy Sản Phẩm Theo ID
```http
GET /:id
Authorization: Bearer <token>
```

#### 4. Mua Sản Phẩm (Tạo Đơn Hàng)
```http
POST /buy
Authorization: Bearer <token>
Content-Type: application/json

[
  {
    "_id": "product_id_1",
    "quantity": 2
  },
  {
    "_id": "product_id_2",
    "quantity": 1
  }
]
```

**Response (201):**
```json
{
  "_id": "unique-order-uuid",
  "username": "testuser",
  "products": [
    {
      "_id": "...",
      "name": "Laptop Gaming",
      "price": 25000000,
      "quantity": 2
    }
  ],
  "totalPrice": 50000000,
  "status": "pending"
}
```

### Order Service (Port 3002)

#### 1. Lấy Tất Cả Đơn Hàng
```http
GET /
Authorization: Bearer <token>
```

**Response (200):**
```json
[
  {
    "_id": "...",
    "orderMapId": "unique-order-uuid",
    "user": "testuser",
    "products": [
      {
        "productId": "...",
        "productName": "Laptop Gaming",
        "quantity": 2,
        "price": 25000000
      }
    ],
    "totalPrice": 50000000,
    "status": "completed"
  }
]
```

#### 2. Lấy Đơn Hàng Theo ID
```http
GET /:id
Authorization: Bearer <token>
```

## 🔄 Luồng Hoạt Động

### 1. Luồng Xác Thực (Authentication Flow)
```
Client → POST /register → Auth Service → MongoDB → Response
Client → POST /login → Auth Service → Generate JWT → Response
Client → Request with JWT → Verify Token → Allow/Deny
```

### 2. Luồng Tạo Đơn Hàng (Order Creation Flow)
```
Client → POST /buy → Product Service
                    ↓
              Validate Products
                    ↓
        Publish to RabbitMQ (order_queue)
                    ↓
              Order Service (Consumer)
                    ↓
            Save to MongoDB
                    ↓
        Publish to RabbitMQ (product_queue)
                    ↓
        Product Service (Consumer)
                    ↓
            Return Order to Client
```

## 🧪 Testing Với Postman

### Bước 1: Đăng Ký và Đăng Nhập
1. Register user mới
2. Login để lấy JWT token
3. Lưu token vào Environment variable

### Bước 2: Tạo Sản Phẩm
1. Sử dụng token để tạo vài sản phẩm
2. Lưu product IDs

### Bước 3: Mua Sản Phẩm
1. Gọi endpoint `/buy` với danh sách product IDs
2. Nhận order confirmation

### Bước 4: Kiểm Tra Đơn Hàng
1. Gọi endpoint `GET /` trên Order Service
2. Xác nhận đơn hàng đã được tạo thành công

### Import Postman Collection

Tạo file `postman_collection.json` và import vào Postman để test nhanh.

## 🔒 Bảo Mật

- **JWT Authentication**: Tất cả endpoints (trừ register/login) đều yêu cầu JWT token
- **Password Hashing**: Sử dụng bcrypt để hash mật khẩu
- **Environment Variables**: Sensitive data được lưu trong .env files
- **Docker Secrets**: Sử dụng GitHub Secrets cho CI/CD

## 📊 GitHub Actions CI/CD

### Workflow Steps

1. **test-auth**:
   - Setup Node.js với cache
   - Cài đặt dependencies
   - Tạo .env file với test data
   - Chạy unit tests

2. **test-product**:
   - Setup Node.js với cache
   - Start Auth service cho integration tests
   - Chạy product tests

3. **build-and-push**:
   - Build Docker images cho tất cả services
   - Push images lên Docker Hub
   - Sử dụng cache để tăng tốc độ build

### GitHub Secrets Cần Thiết

Cấu hình trong **Settings → Secrets and variables → Actions**:

- `JWT_SECRET`: Secret key cho JWT
- `LOGIN_TEST_USER`: Username cho testing
- `LOGIN_TEST_PASSWORD`: Password cho testing
- `DOCKER_PASSWORD`: Docker Hub password/token
- `MONGODB_AUTH_URI`: MongoDB connection string (optional)
- `MONGODB_PRODUCT_URI`: MongoDB connection string (optional)

## 🐛 Troubleshooting

### Lỗi MongoDB Connection
```bash
# Kiểm tra MongoDB container
docker-compose ps
docker-compose logs mongodb

# Restart MongoDB
docker-compose restart mongodb
```

### Lỗi RabbitMQ Connection
```bash
# Kiểm tra RabbitMQ
docker-compose logs rabbitmq

# Truy cập RabbitMQ Management UI
open http://localhost:15672
```

### Port Already in Use
```bash
# Tìm process đang sử dụng port
netstat -ano | findstr :3000

# Kill process (Windows)
taskkill /PID <PID> /F
```

### Xóa Tất Cả Containers và Volumes
```bash
docker-compose down -v
docker system prune -a
```

## 📈 Cải Tiến Trong Tương Lai

- [ ] Thêm service Payment
- [ ] Implement caching với Redis
- [ ] Thêm API documentation với Swagger
- [ ] Implement rate limiting
- [ ] Thêm monitoring với Prometheus/Grafana
- [ ] Implement database migrations
- [ ] Thêm service discovery với Consul
- [ ] Implement circuit breaker pattern

## 👥 Đóng Góp

Mọi đóng góp đều được chào đón! Vui lòng:

1. Fork repository
2. Tạo branch mới (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Tạo Pull Request

## 📄 License

This project is licensed under the MIT License.

## 📞 Liên Hệ

- **Author**: Nguyen Huy Trong
- **Student ID**: 22669731
- **Project**: EProject Phase 2

---

**Made with ❤️ using Node.js & Docker**
