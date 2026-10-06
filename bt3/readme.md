# Exercise 3 - Cấu hình tường lửa UFW và chẩn đoán cổng mạng

## 1. Cấu hình UFW

Thiết lập chính sách mặc định:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Kết quả:

```text
Default incoming policy changed to 'deny'
Default outgoing policy changed to 'allow'
```

## 2. Kiểm tra trạng thái UFW

Lệnh:

```bash
sudo ufw status verbose
```

Kết quả:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), deny (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp (OpenSSH)           ALLOW IN    Anywhere
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
8080/tcp                   ALLOW IN    Anywhere
8090/tcp                   ALLOW IN    Anywhere
80/tcp (Nginx HTTP)        ALLOW IN    Anywhere
22/tcp (OpenSSH (v6))      ALLOW IN    Anywhere (v6)
22/tcp (v6)                 ALLOW IN    Anywhere (v6)
80/tcp (v6)                 ALLOW IN    Anywhere (v6)
8080/tcp (v6)               ALLOW IN    Anywhere (v6)
8090/tcp (v6)               ALLOW IN    Anywhere (v6)
80/tcp (Nginx HTTP (v6))   ALLOW IN    Anywhere (v6)
```

## 3. Kiểm tra các cổng đang lắng nghe

Lệnh:

```bash
ss -tlnp
```

Kết quả:

```text
State         Recv-Q        Send-Q               Local Address:Port               Peer Address:Port       Process
LISTEN        0             4096                    127.0.0.54:53                      0.0.0.0:*
LISTEN        0             4096                       0.0.0.0:22                      0.0.0.0:*
LISTEN        0              511                       0.0.0.0:80                      0.0.0.0:*
LISTEN        0              511                       0.0.0.0:8090                    0.0.0.0:*
LISTEN        0              511                       0.0.0.0:8080                    0.0.0.0:*
LISTEN        0             4096                 127.0.0.53%lo:53                      0.0.0.0:*
LISTEN        0             4096                          [::]:22                         [::]:*
LISTEN        0              511                           [::]:80                         [::]:*
LISTEN        0              511                           [::]:8090                       [::]:*
LISTEN        0              511                           [::]:8080                       [::]:*
```

## 4. Kết luận

* UFW đang ở trạng thái `active`.
* Chính sách mặc định: chặn kết nối vào (`deny incoming`).
* Chính sách mặc định: cho phép kết nối ra (`allow outgoing`).
* SSH port `22/tcp` được phép kết nối.
* Web application port `8080/tcp` được phép kết nối.
* Port `8080` đang thực sự lắng nghe trên máy chủ.
* Các port `80` và `8090` cũng đang được cấu hình và sử dụng trên máy chủ.
