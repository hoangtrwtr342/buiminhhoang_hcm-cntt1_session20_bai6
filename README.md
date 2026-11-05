# Bài 6 : Ngăn chặn rò rỉ thông tin bí mật trên GitHub


### 1. Pre-push Checklist
  1. Chạy công cụ quét: gitleaks detect --source .
  
  2. Soát code chuẩn bị commit: git diff --staged
  
  3. Kiểm tra trạng thái: Đảm bảo file chứa secret (.env, *.pem, *.key) không bị track.

###2. Tệp .env.example mẫu
```plaintext
PORT=8080
DB_HOST=localhost
DB_USER=your_user
DB_PASS=your_password
JWT_SECRET=your_secret_key
STRIPE_KEY=pk_test_placeholder
AWS_KEY=your_aws_key
```
### 3. Bắt buộc 2FA trên GitHub

- Vào Organization Settings > Authentication security > Bật "Require two-factor authentication for everyone". Thành viên chưa kích hoạt 2FA sẽ tự động bị chặn quyền truy cập.

### 4. Cấu trúc repo mẫu

```Plaintext
ecommerce-repo/
├── .env.example   # Công khai (chỉ chứa tên biến)
├── .gitignore     # Bỏ qua: .env, *.key, node_modules/
└── src/           # Code chính
```
### 5. Rủi ro khi thiếu .gitignore

- Lộ secret tức thì: File .env bị đẩy lên GitHub Public, bot tự động quét và đánh cắp mã khóa trong vài giây.

- Tổn thất lớn: Kẻ xấu dùng API key để tạo tài nguyên cày coin (phát sinh chi phí lớn) hoặc truy cập xóa/tống tiền cơ sở dữ liệu.

- Kẹt vĩnh viễn trong Git History: Dù xóa file ở commit sau thì dữ liệu cũ vẫn còn nguyên trong lịch sử commit, bắt buộc phải hủy và đổi lại toàn bộ API keys.
