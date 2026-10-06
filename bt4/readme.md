# Exercise 4 - Quản lý tiến trình nền với nohup và tín hiệu Kill

## 1. Tạo shell script

File `loop-monitor.sh`:

```bash
#!/bin/bash

while true; do
    echo "System time: $(date)" >> /tmp/monitor.log
    sleep 5
done
```

Cấp quyền thực thi:

```bash
chmod +x loop-monitor.sh
```

Kiểm tra:

```text
-rwxrwxr-x 1 khanh khanh 97 Oct 6 09:56 loop-monitor.sh
```

## 2. Chạy script bằng nohup

Lệnh:

```bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
```

Kết quả:

```text
[1] 149170
```

PID của tiến trình là:

```text
149170
```

## 3. Kiểm tra tiến trình

Lệnh:

```bash
pgrep -f loop-monitor.sh
```

Kết quả:

```text
149170
```

Kiểm tra bằng `ps`:

```bash
ps aux | grep loop-monitor.sh
```

Kết quả cho thấy tiến trình `/bin/bash ./loop-monitor.sh` đang chạy nền.

## 4. Kiểm tra file log

Lệnh:

```bash
tail -n 10 /tmp/monitor.log
```

Kết quả:

```text
System time: Tue Oct  6 09:57:41 AM UTC 2026
System time: Tue Oct  6 09:57:46 AM UTC 2026
System time: Tue Oct  6 09:57:51 AM UTC 2026
System time: Tue Oct  6 09:57:56 AM UTC 2026
System time: Tue Oct  6 09:58:01 AM UTC 2026
System time: Tue Oct  6 09:58:53 AM UTC 2026
System time: Tue Oct  6 09:58:58 AM UTC 2026
System time: Tue Oct  6 09:59:03 AM UTC 2026
System time: Tue Oct  6 09:59:08 AM UTC 2026
System time: Tue Oct  6 09:59:13 AM UTC 2026
```

Các dòng log được ghi cách nhau khoảng 5 giây, chứng minh script hoạt động đúng.

## 5. Dừng tiến trình bằng SIGTERM

Sử dụng tín hiệu SIGTERM (15):

```bash
kill -15 149170
```

Kiểm tra lại:

```bash
pgrep -af loop-monitor.sh
```

Không còn tiến trình `loop-monitor.sh`.

Terminal xác nhận:

```text
[1]+  Terminated  nohup ./loop-monitor.sh > /dev/null 2>&1
```

## 6. Kết luận

* Script `loop-monitor.sh` được tạo thành công.
* Script được cấp quyền thực thi.
* Script chạy nền độc lập bằng `nohup`.
* Tiến trình được xác định bằng PID `149170`.
* File `/tmp/monitor.log` được cập nhật định kỳ 5 giây.
* Tiến trình được dừng thành công bằng `kill -15` (SIGTERM).
* Không cần sử dụng SIGKILL (9) vì SIGTERM đã dừng tiến trình thành công.
