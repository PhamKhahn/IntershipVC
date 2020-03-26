# Byobu
## I. Cài đặt
CentOs7
```
sudo yum install epel-release -y
sudo yum install byobu -y --enablerepo=epel-testing
```
Ubuntu
```
sudo apt install -y byobu
```
## II. Thao tác
Dòng cuối cùng có thông tin về các tab đang mở, ngày giờ, CPU, RAM...

Lưu ý không mở byobu bên trong 1 byobu khác. ( Sẽ dẫn tới bị crash)

Chỉ mở tab mới bằng F2 (dùng sau khi mở tab byobu đầu tiên bằng lệnh "byobu")

sau khi mở tab byobu đầu tiên bằng lệnh "byobu"

1 số thao tác cơ bản ta có thể làm sau đó:
```
    F1 : mở help xem hướng dẫn
    F2 : mở 1 tab mới
    F3 : chuyển về tab đằng sau (hoặc Shift + <-)
    F4 : chuyển lên tab phía trước (hoặc Shift + ->)
    exit : Đóng hẳn tab (dừng các tác vụ đang thực thi trên tab đó)
    Shift + F2 : chia đôi,ba,... màn hình theo chiều dọc
    Ctrl + F2 : chia đôi,ba,.. màn hình theo chiều ngang
    F6 : Đóng các tab của byobu xuống, về terminal ban đầu (Muốn mở lên gõ "byobu")
    F8 : Đặt tên cho tab
    
```