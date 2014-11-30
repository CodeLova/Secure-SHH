Secure-SHH
==========
##### I.Tìm hiểu về giao thức SSH:

- SSH (tiếng Anh: Secure Shell) là một giao thức mạng dùng để thiết lập kết nối mạng một cách bảo mật. 
- SSH hoạt động ở lớp trên trong mô hình phân lớp TCP/IP. Các công cụ SSH (như là OpenSSH,...) cung cấp cho người dùng cách thức để thiết lập kết nối mạng được mã hoá để tạo một kênh kết nối riêng tư. Hơn nữa tính năng tunneling của các công cụ này cho phép chuyển tải các giao vận theo các giao thức khác. 
- Do vậy có thể thấy khi xây dựng một hệ thống mạng dựa trên SSH, chúng ta sẽ có một hệ thống mạng riêng ảo VPN đơn giản.

####### 1.SSH là gì?
- SSH là một chương trình tương tác giữa máy chủ và máy khách có sử dụng cơ chế mã hoá đủ mạnh nhằm ngăn chặn các hiện tượng nghe trộm, đánh cắp thông tin trên đường truyền. Các chương trình trước đây: telnet, rlogin không sử dụng phương pháp mã hoá. Vì thế bất cứ ai cũng có thể nghe trộm thậm chí đọc được toàn bộ nội dung của phiên làm việc bằng cách sử dụng một số công cụ đơn giản.
- Sử dụng SSH là biện pháp hữu hiệu bảo mật dữ liệu trên đường truyền từ hệ thống này đến hệ thống khác.

####### 2.Cách thức làm việc của SSH:
SSH làm việc thông qua 3 bước đơn giản:

- Định danh host - xác định định danh của hệ thống tham gia phiên làm việc SSH.
- Mã hoá - thiết lập kênh làm việc mã hoá.
- Chứng thực - xác thực người sử dụng có quyền đăng nhập hệ thống.

Trong bài viết này mình chỉ đi sâu vào nâng cao bảo mật cho SSH.phần tìm hiều về giao thức ssh sẽ có link ở dưới.

##### II. Cấu hình nâng cao ssh:

1. Đổi port mặc định khi truy cập vào ssh:
- Mặc định ssh sẽ sử dụng port 22 để cho phép truy cập vào hệ thống. Như vậy sẽ không an toàn cho hệ thống của ta, nên chuyển sang một port khác.

- Đăng nhập vào bằng tài khoản root:
```
 su -
```
- Để đổi port ta vào file cấu hình ssh: ở đây mình đổi về port 1710.
```
/etc/ssh/sshd_config
...
port 22 -> port 1710 [5]
```
- Sau khi thay đổi ta cấu update firewall:
```
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 1710 -j ACCEPT
```
- Khởi động lại dịch  vụ ssh:
```
service ssh restart
```
- Bây giờ bạn ssh vào máy chủ thông qua cú pháp sau:
```
ssh root@<ip_server> -p port
```
ví dụ : Mình ssh vào máy chủ 172.16.69.145 với tài khoản longnt
```
ssh longnt@172.16.69.145 -p 1710 
```
2. Cấm truy cập vào ssh bằng tài khoản root:

- Tài khoản root là tài khoản có toàn quyền trong hệ điều hành Linux. Chính vì thế khi attacker có được tài khoản root sẽ nguy hiểm cho hệ thống.

Để cấm root đăng nhập thông qua ssh. Ta vào file cấu hình ssh và sửa như sau:

```
/etc/ssh/sshd_config
PermitRootLogin yes -> PermitRootLogin no [28]
```
- Khởi động lại dịch vụ ssh :
```
service ssh restart
```

Lưu ý : Bạn lên đăt mật khẩu root khác với mật khẩu của user.

- Nhưng có trường hợp này sẽ xảy ra và nó đã xảy ra với hệ thống của mình. Cấm truy cập bằng root nhưng một số ip được phép truy cập bằng root thì sao?.
- Vậy thì ta lại chỉ cho một số ip được quyền truy cập bằng root:

Vào trong file cấu hình ssh và thêm vào như sau:
```
etc/ssh/sshd_config

AllowUsers root<ip1>,root<ip2>
```
Khởi động lại dịch vụ ssh :
```
service ssh restart
```


















