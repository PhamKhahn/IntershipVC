# Chap 1 Kiến trúc hệ thống và thiết kế Ansible

# I. Inventory parsing and data sources
**Inventory**
- Trong Ansible, không có gì diễn ra mà thiếu inventory. kể cả các ad hoc trên localhost cũng yêu cầu 1 inventory

- Khi thực thi ansible/ ansible-playbook, 1 inventory phải được tham chiếu

- Inventory là các tệp/ thư mục tồn tại trên cùng hệ thống chạy ansible/ansible-playbook
- Tham chiếu bằng đối số --inventory-file (-i) hoặc xác định bằng cách xác định đường dẫn trong file config Ansible

- Inventory có thể là tĩnh/ động,hoặc kết hợp cả 2.

**Variable data**
- chẳng hạn như chi tiết cụ thể các kết nối với 1 host cụ thể trong inventory

## **Static inventory*
- Dạng inventory cơ bản nhất
- 1 static inventory sẽ gồm 1 file đơn dạng ini
- Liệt kê tên hệ thống trong inventory.
    - Các tên này tham chiếu các tên máy cụ thể hoặc group riêng biệt -> Nên sắp xếp các host thành các group dựa trên chức năng.
    - Các group có thể bao gồm các group khác
    - Các host không nằm trong group nào phải được khai báo đầu tiên.

Ví dụ:

```
[web]
mastery.example.name
[dns]
backend.example.name
[database]
backend.example.name
[frontend:children]
web
[backend:children]
dns
database
```
- Tạo 3 group với 1 hệ thống trong 1 group
- Tạo 2 group nhóm 3 group trên.

## Inventory ordering
keyword **order** từ bản Ansible 2.4

Thông thường , Ansible xử lý các host theo thứ tự được chỉ định trong inventory file.

Tuy nhiên các giá trị sau có thể được set cho **order** 
- *inventory* : Option mặc định, nghĩa là Ansible tiến hành như bt, xử lý các host theo thứ tự được liệt kê trong file inventory
- *reverse_inventory* : Xử lý theo thứ tự ngược lại
- *sorted* : các host được xử lý theo bảng chữ cái 
- *reverse_sorted* : thứ tự ngược lại vs sorted
- *shuffle* : Các host xáo trộn theo thứ tự ngẫu nhiên

## Inventory variable data
Dữ liệu trong inventory thường nhiều chứ không chỉ là tên hệ thống và group
- Dữ liệu riêng của máy chủ -> sử dụng trong các template
- Dữ liệu group -> dùng trong các đối số của task/condition
- Các tham số hành vi -> điều chỉnh ansible tương tác với 1 hệ thống

Các biến khá hữu dụng trong Ansible. 
Biến có thể đến từ nhiều nguồn và có thể ghi đè lên nhau

```
[web]
mastery.example.name ansible_host=192.168.10.25
[dns]
backend.example.name
[database]
backend.example.name
[frontend:children]
web
[backend:children]
dns
database
[web:vars]
http_port=88
proxy_timeout=5
[backend:vars]
ansible_port=314
[all:vars]
ansible_ssh_user=otto
```
- ansible_host : chỉ định IP của host (chỉ định cho Ansible thay vì việc tra cứu DNS)
- http_port : có thể được sử dụng trong tệp cấu hình NGINX
- proxy_timeout : có thể được sử dụng để xác định hành vi HAProxy.
- ansible_port : chỉ định port trên host mà Ansible sẽ SSH tới. thường mặc định là 22

- ansible_ssh_user: user mà Ansible sẽ sử dụng để SSH vào các hệ thống
    - [all:vars] : tất cả các host


```
ansible_host : chỉ định DNS name/ Docker container name

ansible_port : port trên host mà Ansible kết nối đến 

ansible_user : username mà Ansible sẽ kết nối tới host

ansible_ssh_pass : password để xác thực (kết hợp ansible_user)

ansible_ssh_private_key_file : SSH private key file được dùng để kết nối tới host

ansible_ssh_common_args : define tham số SSH để  nối vào các đối số mặc định cho ssh,sftp,scp

ansible_sftp_extra_args : Được sử dụng để chỉ định các đối số bổ sung sẽ được chuyển sang sftp binary khi được gọi bởi Ansible.

ansible_scp_extra_args

ansible_ssh_extra_args 

ansible_ssh_pipelining 

ansible_ssh_executable

ansible_become : Xác định xem có thực hiện leo thang đặc quyền (sudo) với host hay không

ansible_become_method : 1 pp leo thang đặc quyền. có thể dùng sudo,su,pbrun,pfexec
```

