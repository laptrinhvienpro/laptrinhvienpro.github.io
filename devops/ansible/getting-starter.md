> Ansible đang là công cụ Configuration Management khá nổi bật hiện nay.

- Là công cụ mã nguồn mở dùng để quản lý cài đặt, cấu hình hệ thống một cách tập trung và cho phép thực thi câu lệnh điều khiển.
- Sử dụng SSH (hoặc Powershell) và các module được viết bằng ngôn ngữ Python để điểu khiển hệ thống.
- Sử dụng định dạng JSON để hiển thị thông tin và sử dụng YAML (Yet Another Markup Language) để xây dựng cấu trúc mô tả hệ thống.

## Features
- Không cần cài đặt phần mềm lên các agent, chỉ cần cài đặt tại master.
- Không service, daemon, chỉ thực thi khi được gọi
- Bảo mật cao ( do sử dụng giao thức SSH để kết nối )
- Cú pháp dễ đọc, dễ học, dễ hiểu

## Installation
```bash
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository -y --update ppa:ansible/ansible
sudo apt install -y ansible
sudo apt install -y sshpass
```
> ssh-pass hỗ trợ chạy ansible đẩy password vào client, không cần phải dùng key.
[More](/devops/ansible/ansible-install)

## Hosts
Đầu tiên ta phải định nghĩa host/group những máy cần ssh vào. Cài đặt host trong ansible giống như /etc/hosts vậy. Nó có chức năng phân giải tên gọi thành ip address. Nhưng linh động hơn rất nhiều, có thể nhóm group lại với nhau, đặt thông số đi kèm hosts....

```bash
sudo mkdir /etc/ansible
sudo sudo /etc/ansible/hosts
```

```ini
;/etc/ansible/hosts
[api]
192.168.106.175
[proxy]
192.168.106.176

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com

[webservers]
www[01:50].example.com
```




## Tasks

    - hosts: all
      user: root
      sudo: no
      vars:
        aaa: bbb
      tasks:
        - ...
      handlers:
        - ...

### Includes

    tasks:
      - include: db.yml
    handlers:
      - include: db.yml user=timmy

## Handlers

    handlers:
      - name: start apache2
        action: service name=apache2 state=started

    tasks:
      - name: install apache
        action: apt pkg=apache2 state=latest
        notify:
          - start apache2

## Vars

    - host: lol
      vars_files:
        - vars.yml
      vars:
        project_root: /etc/xyz
      tasks:
        - name: Create the SSH directory.
          file: state=directory path=${project_root}/home/.ssh/
          only_if: "$vm == 0"

## Roles

    - host: xxx
      roles:
        - db
        - { role:ruby, sudo_user:$user }
        - web

    # Uses:
    # roles/db/tasks/*.yml
    # roles/db/handlers/*.yml

### Task: Failures

    - name: my task
      command: ...
      register: result
      failed_when: "'FAILED' in result.stderr"

      ignore_errors: yes

      changed_when: "result.rc != 2"

### Env vars

    vars:
      local_home: "{{ lookup('env','HOME') }}"

## References

  * [Intro](http://www.ansibleworks.com/docs/intro_configuration.html)
  * [Modules](http://www.ansibleworks.com/docs/modules.html)