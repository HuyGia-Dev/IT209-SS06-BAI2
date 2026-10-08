# Hướng dẫn hoàn thành Bài 4 và đẩy code lên GitHub

Các bước dưới đây sẽ giúp bạn tạo các tệp cần thiết, thực hiện các lệnh yêu cầu và đẩy kết quả lên thư mục `homework/session_06/ex4/` trên GitHub.

## Bước 1: Tạo cấu trúc thư mục
Mở Terminal và tạo thư mục bài tập theo đúng đường dẫn yêu cầu, sau đó di chuyển vào thư mục đó:
```bash
mkdir -p homework/session_06/ex4
cd homework/session_06/ex4
```

## Bước 2: Tạo và cấp quyền cho shell script
Tạo file `loop-monitor.sh` bằng lệnh `cat` và cấp quyền thực thi:
```bash
cat << 'EOF' > loop-monitor.sh
#!/bin/bash
while true; do
    echo "System time: $(date)" >> /tmp/monitor.log
    sleep 5
done
EOF

chmod +x loop-monitor.sh
```

## Bước 3: Chạy kịch bản dưới nền và kiểm tra
1. Khởi chạy kịch bản bằng `nohup`:
```bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
```
2. Lấy PID (Số định danh tiến trình) của script:
```bash
pgrep -f loop-monitor.sh
```
*(Ghi nhớ số PID hiện ra để đưa vào báo cáo và sử dụng ở bước sau)*

3. Kiểm tra log để xem kịch bản có đang ghi dữ liệu không:
```bash
tail -n 10 /tmp/monitor.log
```

## Bước 4: Tắt tiến trình
Sử dụng số PID bạn vừa lấy được ở trên để tắt tiến trình một cách an toàn bằng tín hiệu `SIGTERM` (15):
```bash
kill -15 <PID_CUA_TIEN_TRINH>
```
Kiểm tra lại xem tiến trình đã tắt chưa:
```bash
ps aux | grep loop-monitor.sh
```

## Bước 5: Tạo tệp báo cáo README.md
Tạo tệp `README.md` theo yêu cầu của bài tập để ghi lại các lệnh và kết quả. (Bạn hãy thay `<PID_CUA_TIEN_TRINH>` bằng con số thực tế lúc bạn chạy).

```bash
cat << 'EOF' > README.md
# Báo cáo Bài 4: Quản lý tiến trình nền với nohup và tín hiệu Kill

## 1. Lệnh khởi chạy nohup
\`\`\`bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
\`\`\`

## 2. Kết quả tail file log
\`\`\`bash
tail -n 10 /tmp/monitor.log
# (Kết quả trả về hiển thị dòng "System time: ...")
\`\`\`

## 3. Lệnh kill kèm PID
\`\`\`bash
pgrep -f loop-monitor.sh
# PID trả về ví dụ: 12345
kill -15 12345
\`\`\`
EOF
```

## Bước 6: Commit và Push lên GitHub
Thực hiện chuỗi lệnh Git sau để đẩy thư mục `homework/session_06/ex4/` lên kho lưu trữ của bạn:

```bash
# Thêm toàn bộ các thay đổi trong thư mục hiện tại
git add .

# Ghi lại commit với thông điệp rõ ràng
git commit -m "Hoàn thành Bài 4: Quản lý tiến trình với nohup và kill"

# Đẩy code lên nhánh hiện tại trên GitHub (ví dụ nhánh main)
git push origin main
```
*(Lưu ý: Nếu bạn đang làm việc trên nhánh khác, hãy thay `main` bằng tên nhánh của bạn).*