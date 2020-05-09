# Example 1: Ping
```bash
# -i: hosts path
# -m: module
# -u: user
# -k: use password
ansible -i /etc/ansible/hosts api -m ping -u ansible -k
```

Output
```
$ SSH password: (nhập pass của host api)
192.168.106.175 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
> Nếu chạy lỗi như sau: "**bash: /usr/bin/python: No such file or directory"** thì bạn cần phải cài thêm python trên client

# Example 2: Excute Command with Root Permission
```bash
# -K: nhập sudo password
# -s: thực thi lệnh với sudo
ansible -i /etc/ansible/hosts api -m shell -a "whoami" -u ansible -k -K -s
```
Output
```
$ SSH password:
$ SUDO password[defaults to SSH password]:
192.168.106.175 | SUCCESS | rc=0 >>
root
```

# Example 3: Predefine client user / ssh key
```bash
nano /etc/ansible/hosts
```
```ini
[api]
192.168.106.175 ansible_ssh_user=ansible  ansible_ssh_private_key_file=/etc/ansible/key_ssh_server/id_rsa
```
Output
```
$ ansible -i hosts api -m shell -a "whoami"
192.168.106.175 | SUCCESS | rc=0 >>
ansible
```

# Other basic commands
Ansible option
```
-i : inventory host. Load thư viện host
-m : gọi module của ansible
-a : command được gửi vào module
-u : user
-vvvv : debug option
```

```bash
$ ansible -i hosts all -m ping
$ ansible -i 'localhost,' -c local all -m ping  (Ping local)
$ ansible -i hosts all -m shell -a 'top -bcn1 | head'
$ ansible -i hosts all -m command -a uptime
Mặc định, ansible sẽ cho module là "command". Nên ta ko cần -m command thêm vào cũng được.
$ ansible -i hosts all -a uptime
- restart mysql
$ ansible dbserver -m service -a "name=mysql state=restarted" --key-file=~/.ssh/db.pem -u ubuntu --sudo
```
> Tất cả module của ansible bạn có thể tham khảo ở đây http://docs.ansible.com/ansible/list_of_all_modules.html