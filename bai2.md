# Hướng dẫn hoàn thành Bài 2: Cấu hình phân quyền Nhóm và sudoers

## Bước 1: Tạo nhóm và người dùng mới

Mở Terminal và chạy lần lượt các lệnh sau (nhập mật khẩu root của bạn nếu được yêu cầu):

```bash
# Tạo nhóm devops-admin
sudo groupadd devops-admin

# Tạo tài khoản deployer (hệ thống sẽ yêu cầu bạn thiết lập mật khẩu cho user này)
sudo adduser deployer

# Thêm user deployer vào nhóm devops-admin
sudo usermod -aG devops-admin deployer
```

## Bước 2: Cấu hình tệp sudoers bằng visudo

1. Mở tệp cấu hình sudoers:
```bash
sudo visudo
```
2. Di chuyển con trỏ chuột xuống cuối tệp tin và thêm chính xác dòng sau vào:
```text
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```
3. Lưu và thoát (Nếu dùng `nano` bên trong visudo: Nhấn `Ctrl+O` -> `Enter` để lưu, sau đó `Ctrl+X` để thoát).

## Bước 3: Kiểm tra cấu hình

1. Chuyển sang tài khoản `deployer`:
```bash
su - deployer
```
2. Kiểm tra quyền `sudo`:
```bash
sudo -l
```
*(Hãy copy toàn bộ kết quả hiển thị trên màn hình của lệnh này để lát nữa dán vào file báo cáo README.md)*

3. Chạy thử lệnh khởi động lại dịch vụ `cron` (sẽ không bị hỏi mật khẩu):
```bash
sudo systemctl restart cron
```
4. Gõ `exit` để thoát khỏi user `deployer`, quay lại user ban đầu của bạn:
```bash
exit
```

## Bước 4: Tạo cấu trúc thư mục và tệp báo cáo README.md

Tạo thư mục bài tập và tệp báo cáo `README.md` theo yêu cầu:

```bash
mkdir -p homework/session_06/ex2
cd homework/session_06/ex2
```

Tạo file `README.md` (Bạn nhớ thay thế phần `[DÁN KẾT QUẢ...]` bằng nội dung bạn đã copy ở Bước 3):

```bash
cat << 'EOF' > README.md
# Báo cáo Bài 2: Cấu hình phân quyền Nhóm và sudoers bằng visudo

## 1. Kết quả chạy lệnh `sudo -l` khi đăng nhập bằng user deployer:

```text
[DÁN KẾT QUẢ COPY TỪ LỆNH SUDO -L VÀO ĐÂY]
```

## 2. Dòng cấu hình đã thêm vào sudoers:

```text
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```
EOF
```

## Bước 5: Commit và Push lên GitHub

Cuối cùng, đẩy kết quả lên kho lưu trữ GitHub của bạn:

```bash
# Thêm file báo cáo vào git
git add README.md

# Ghi lại commit
git commit -m "Hoàn thành Bài 2: Cấu hình phân quyền devops-admin trong sudoers"

# Đẩy code lên GitHub (thay 'main' bằng nhánh của bạn nếu cần)
git push origin main
```