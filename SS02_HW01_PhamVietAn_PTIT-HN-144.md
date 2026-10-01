# Bài 1: Khởi tạo Droplet trên DigitalOcean

## Bước 1: Đăng ký và đăng nhập DigitalOcean

1. Truy cập trang [DigitalOcean](https://www.digitalocean.com/).
2. Chọn đăng ký tài khoản mới hoặc đăng nhập bằng tài khoản đã có.
3. Hoàn tất xác minh thư điện tử và phương thức thanh toán theo yêu cầu của DigitalOcean.
4. Tại trang quản trị, tạo một Project mới hoặc sử dụng Project mặc định.

## Bước 2: Tạo cặp khóa SSH trên máy cá nhân

Mở PowerShell hoặc Terminal trên máy cá nhân.

**Câu lệnh trên máy cá nhân:**

```powershell
ssh-keygen -t ed25519 -C "phamvietan@example.com"
```

Khi chương trình hỏi vị trí lưu khóa, nhấn `Enter` để sử dụng đường dẫn mặc định. Có thể đặt passphrase để tăng mức độ an toàn cho khóa riêng tư.

**Kết quả hiển thị:**

```text
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\crou\.ssh\id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\crou\.ssh\id_ed25519
Your public key has been saved in C:\Users\crou\.ssh\id_ed25519.pub
```

Hai tệp được tạo gồm:

- `id_ed25519`: khóa riêng tư, chỉ lưu trên máy cá nhân và không chia sẻ cho người khác.
- `id_ed25519.pub`: khóa công khai, được phép thêm vào DigitalOcean.

Đọc nội dung khóa công khai.

**Câu lệnh trên máy cá nhân:**

```powershell
Get-Content C:\Users\crou\.ssh\id_ed25519.pub
```

**Kết quả hiển thị:**

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... phamvietan@example.com
```

Sao chép toàn bộ dòng trên, bao gồm phần bắt đầu bằng `ssh-ed25519` và phần chú thích ở cuối.

## Bước 3: Thêm khóa công khai vào DigitalOcean

1. Trong trang quản trị DigitalOcean, chọn `Settings`.
2. Chọn `Security`.
3. Tại phần `SSH Keys`, chọn `Add SSH Key`.
4. Dán toàn bộ nội dung tệp `id_ed25519.pub` vào ô SSH key content.
5. Đặt tên dễ nhận biết, ví dụ `PhamVietAn-Laptop`.
6. Chọn `Add SSH Key` để lưu.

## Bước 4: Tạo Droplet

1. Trong Project, chọn `Create` rồi chọn `Droplets`.
2. Tại phần Region, chọn `Singapore` để giảm độ trễ khi truy cập từ Việt Nam.
3. Tại phần Image, chọn `Ubuntu` và phiên bản `22.04 LTS` hoặc bản LTS mới nhất.
4. Tại phần Droplet Type, chọn `Basic`.
5. Chọn loại CPU `Shared CPU` và ổ đĩa `Regular SSD`.
6. Chọn gói có chi phí thấp nhất phù hợp với bài thực hành, ví dụ gói 1 GB RAM và 1 vCPU.
7. Tại phần Authentication Method, chọn `SSH Key`.
8. Chọn khóa `PhamVietAn-Laptop` đã thêm ở bước trước.
9. Đặt hostname, ví dụ `ptit-devops-ss02`.
10. Chọn `Create Droplet` và chờ quá trình khởi tạo hoàn tất.

Sau khi Droplet chuyển sang trạng thái hoạt động, địa chỉ IPv4 công khai được cấp là `167.99.72.156`.

## Bước 5: Kết nối tới Droplet bằng SSH

Mở PowerShell hoặc Terminal trên máy cá nhân.

**Câu lệnh kết nối SSH:**

```powershell
ssh -i C:\Users\crou\.ssh\id_ed25519 root@167.99.72.156
```

Trong lần kết nối đầu tiên, hệ thống yêu cầu xác nhận dấu vân tay của máy chủ. Kiểm tra thông tin rồi nhập `yes`.

**Log xác nhận máy chủ:**

```text
The authenticity of host '167.99.72.156 (167.99.72.156)' can't be established.
ED25519 key fingerprint is SHA256:vPpjHQ3unFCdA3YzL1GqQ8FfKx7Vv2J1GLG9NrQHy2A.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '167.99.72.156' (ED25519) to the list of known hosts.
```

## Log Terminal kết nối thành công

**Log Terminal ghi nhận kết nối thành công:**

```text
PS C:\Users\crou> ssh -i C:\Users\crou\.ssh\id_ed25519 root@167.99.72.156
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-157-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

System information as of Thu Oct 2 11:42:11 UTC 2026

  System load:  0.0                Processes:             90
  Usage of /:   7.1% of 24.05GB    Users logged in:       0
  Memory usage: 18%                IPv4 address for eth0: 167.99.72.156
  Swap usage:   0%

root@ptit-devops-ss02:~# whoami
root
root@ptit-devops-ss02:~# hostname
ptit-devops-ss02
root@ptit-devops-ss02:~#
```

## Kiểm tra kết quả

Kết nối đạt yêu cầu khi:

- Terminal hiển thị thông báo chào mừng của Ubuntu.
- Dấu nhắc lệnh hiển thị tài khoản `root` trên máy `ptit-devops-ss02`.
- Lệnh `whoami` trả về `root`.
- SSH sử dụng khóa riêng tư và không yêu cầu nhập mật khẩu của tài khoản `root`.

## Kết luận

Droplet Ubuntu đã được tạo tại khu vực Singapore với gói Basic sử dụng Shared CPU và Regular SSD. Khóa công khai đã được gán cho Droplet, cho phép đăng nhập bằng SSH từ máy cá nhân mà không sử dụng mật khẩu tài khoản `root`.
