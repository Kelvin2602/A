# 🚀 HƯỚNG DẪN KHỞI ĐỘNG DỰ ÁN (startup-guide)

> Áp dụng cho repo **kelvin262292/3d** – 3D Model E-commerce Platform  
> Thời gian hoàn thành: ~10-15 phút

---

## 1️⃣ Clone dự án từ GitHub

```bash
# 1. Chọn thư mục làm việc
cd ~/workspace            # hoặc nơi bạn muốn lưu project

# 2. Clone repository
git clone https://github.com/kelvin262292/3d.git
cd 3d
```

---

## 2️⃣ Thiết lập environment variables

1. **Tạo file cấu hình**

```bash
cp .env.example .env.local   # hoặc tự tạo .env.local
```

2. **Điền giá trị bắt buộc**

| Biến | Mô tả | Ví dụ |
|------|-------|-------|
| `DATABASE_URL` | Chuỗi kết nối PostgreSQL **(khuyên dùng)** | `postgresql://user:pass@localhost:5432/3d_db` |
| `NEXTAUTH_SECRET` | Khoá JWT tối thiểu 32 ký tự | `super-secret-jwt-key-1234567890` |
| `NEXTAUTH_URL` | Url site khi chạy local | `http://localhost:3000` |
| `STRIPE_SECRET_KEY` / `STRIPE_PUBLISHABLE_KEY` | Khoá Stripe (test) | Tạo trên dashboard Stripe |
| `CLOUDINARY_*` | Thông tin Cloudinary (tuỳ chọn) | |

> 🔹 Nếu chỉ **thử nghiệm nhanh**, bạn có thể dùng SQLite:  
> `DATABASE_URL="file:./dev.db"`

---

## 3️⃣ Cài đặt dependencies

### Chọn package manager

```bash
# 1. Dùng npm
npm install

# 2. Hoặc dùng pnpm (khuyên dùng)
npm install -g pnpm
pnpm install
```

> ⏱ Mất 1-2 phút – yêu cầu Node.js ≥ 18

---

## 4️⃣ Thiết lập database

### 4.1 Generate Prisma client

```bash
npx prisma generate
```

### 4.2 Push schema & migrate

```bash
# PostgreSQL hoặc SQLite
npx prisma db push
```

### 4.3 (Optional) Seed dữ liệu mẫu

```bash
npm run db:seed
```

### 4.4 Docker (PostgreSQL nhanh)

```bash
docker run -d --name pg-3d -e POSTGRES_PASSWORD=secret \
  -p 5432:5432 postgres:16
# cập nhật DATABASE_URL tương ứng
```

---

## 5️⃣ Khởi động server

```bash
# Development
npm run dev           # hoặc pnpm dev
# Mặc định tại: http://localhost:3000
```

### 5.1 Build & chạy production local

```bash
npm run build
npm start
```

---

## 6️⃣ Checklist kiểm thử nhanh

| Mục | Đường dẫn | Kết quả mong đợi |
|-----|-----------|-----------------|
| Trang chủ | `/` | Logo, hero, sản phẩm nổi bật |
| Danh sách sản phẩm | `/products` | Bộ lọc, tìm kiếm hoạt động |
| Chi tiết sản phẩm | `/products/[slug]` | 3D viewer tương tác mượt |
| Auth | `/login`, `/register` | Đăng nhập & đăng ký thành công |
| Giỏ hàng | `/cart` | Thêm/xoá sản phẩm, tổng tiền đúng |
| Thanh toán Stripe | `/checkout` | Redirect test thành công |
| Admin dashboard | `/admin/dashboard` | Thống kê hiển thị |
| CRUD sản phẩm | `/admin/products` | Thêm/sửa/xoá phản ánh ra frontend ngay |

### Chạy test tự động

```bash
# Unit tests (Jest)
npm test

# E2E tests (Playwright)
npm run test:e2e
```

---

## 7️⃣ Khắc phục sự cố thường gặp

| Vấn đề | Triệu chứng | Cách xử lý |
|--------|-------------|-----------|
| **DB connect error** | `DATABASE_URL` sai | Kiểm tra chuỗi kết nối / service Postgres |
| **Port 3000 bận** | Server không start | `PORT=3001 npm run dev` |
| **Prisma migration lỗi** | Model mới chưa push | `npx prisma db push --force-reset` |
| **Dependencies conflict** | `npm ERR! ERESOLVE` | `npm install --legacy-peer-deps` |
| **Stripe invalid key** | 400 tại checkout | Dùng khóa test bắt đầu bằng `sk_test_…` |
| **3D model không load** | Viewer trắng | Kiểm tra đường dẫn `modelUrl` & CORS |
| **CSP blocking** | Console báo CSP | Sửa CSP trong `next.config.mjs` |

---

## 🎉 Bạn đã sẵn sàng!

Mở `http://localhost:3000`, trải nghiệm cửa hàng 3D và bắt đầu tuỳ biến.  
Gặp lỗi? Xem phần **Troubleshooting** hoặc tạo issue trên GitHub. Chúc bạn thành công!  
