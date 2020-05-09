# INSTALL

> Server Master: 192.168.1.100
> Server Agent: 192.168.1.101

## 1. Setup
```bash
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

## 2. Add SSH User on Agent

Add user
```bash
sudo adduser ansible
```

Cấu hình sudo cho phép tài khoản ansible sử dụng không cần password
```bash
sudo nano /etc/sudoers.d/ansible
```
Nội dung
> ansible ALL=(ALL)   NOPASSWD:ALL

## 3. Tạo SSH Key để kết nối
Để thuận tiện cho việc sử dụng Ansible cũng như giới hạn 1 số hệ thống chỉ cho phép xác thực qua key. (Ansible có hỗ trợ kết nối thông qua password)

### Tạo ssh keyfile: ( trên master )
```bash
ssh-keygen -C "ansible@master"
```
> Lý do không đặt ssh key ở thư mục mặc định là do rất có thể có nhiều người quản trị cùng tham gia quản lý.)

### Copy keyfile sang Agent
```bash
ssh-copy-id -i /etc/ansible/ansible_key.pub ansible@192.168.1.101
```

### Kiểm tra
```bash
ssh -i /etc/ansible/ansible_key ansible@192.168.1.101 
```