Chúng ta ko rảnh để gõ lại từng lệnh phía trên phải không. Ansible rất linh hoạt khi hỗ trợ playbook bằng ngôn ngữ YAML (file .yml). Từ đó, khi admin cần setup server/service nào. Chỉ cần gọi file yml này ra, tất cả sẽ được thực thi một cách tự động.

## Example
Để cho dễ làm quen nhất, tôi sẽ hướng dẫn các bạn cài đặt Apache webserver và deploy đơn giản .config, .html. Ansible server là ubuntu, client là centos. Ở phần sau tôi sẽ hướng dẫn các bạn config cho ansible tự detect OS và chọn apt/yum riêng.

**install-apache-playbook.yml**
```yaml
- hosts: servertest 
  remote_user: root
  tasks:
  # Cài đặt gói tin httpd và start .
  - name: Install HTTP
    yum: name=httpd state=latest
  - name: Start HTTPD after install
    service: name=httpd state=started
  # Deploy config
  ## Backup
  - name: Backup config HTTP (backup from client)
    command: cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.backup1
  ## Deploy
  - name: Deploy config httpd
    template:
     src: "/etc/ansible/config/httpd.conf"
     dest: "/etc/httpd/conf/httpd.conf"
     owner: root
     group: root
     mode: 0644
  # Đẩy source về client
  - name: Deploy web file
    template:
     src: "/etc/ansible/config/index.html"
     dest: "/var/www/html/index.html"
  # Khởi động lại apache để áp dụng config
  - name: Start HTTPD after install
    service: name=httpd state=restarted
```
**CLI**
```bash
ansible-playbook install-apache-playbook.yml
```

**Output**
```
PLAY ***************************************************************************

TASK [setup] *******************************************************************
changed: [192.168.106.150]

TASK [Install HTTP] ************************************************************
changed: [192.168.106.150]

TASK [Start HTTPD after install] ***********************************************
changed: [192.168.106.150]

TASK [Backup config HTTP (backup from client)] *********************************
changed: [192.168.106.150]

TASK [Deploy new config] *******************************************************
changed: [192.168.106.150]

TASK [Deploy web file] *********************************************************
changed: [192.168.106.150]

PLAY RECAP *********************************************************************
192.168.106.150            : ok=6    changed=3    unreachable=0    failed=0
```

## Playbook Structure

### Tasks

    - hosts: all
      user: root
      sudo: no
      vars:
        aaa: bbb
      tasks:
        - ...
      handlers:
        - ...

#### Includes

    tasks:
      - include: db.yml
    handlers:
      - include: db.yml user=timmy

### Handlers

    handlers:
      - name: start apache2
        action: service name=apache2 state=started

    tasks:
      - name: install apache
        action: apt pkg=apache2 state=latest
        notify:
          - start apache2

### Vars

    - host: lol
      vars_files:
        - vars.yml
      vars:
        project_root: /etc/xyz
      tasks:
        - name: Create the SSH directory.
          file: state=directory path=${project_root}/home/.ssh/
          only_if: "$vm == 0"

### Roles

    - host: xxx
      roles:
        - db
        - { role:ruby, sudo_user:$user }
        - web

    # Uses:
    # roles/db/tasks/*.yml
    # roles/db/handlers/*.yml