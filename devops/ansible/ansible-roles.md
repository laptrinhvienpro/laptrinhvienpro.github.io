Các tác vụ liên quan đến nhau có thể được tập hợp lại thành role, sau đó áp dụng cho một nhóm các máy khi cần thiết.

## Structure

- **Task**: Chứa các file yaml định nghĩa các nhiệm vụ chính khi triển khai.
- **Handles**: Chứa các handler được sử dụng trong role
- **Files**: chứa các file dc sử dụng bởi role, ví dụ như các file ảnh.
- **Templates**: chứa các template file được sử dụng trong role, ví dụ như các file configuration... Các file này có đuôi *.j2, sử dụng jinja2 syntax
- **Vars**: định nghĩa các variable được sử dụng ở trong roles
- **Defaults**: Định nghĩa các giá trị default của các variable được sử dụng trong roles. Nếu variable không được định nghiã trong thư mục vars, các giá trị default này sẽ được gọi.
- **Meta**: thư mục này chứa meta data của roles

```
roles/
  common/
    tasks/
    handlers/
    files/              # 'copy' will refer to this
    templates/          # 'template' will refer to this
    meta/               # Role dependencies here
    vars/
    defaults/
      main.yml
```
> Không nhất thiết phải sử dụng tất cả các thư mục sau khi tạo một role.

Chú ý bên trong thư mục phải tuân thủ việc khai báo tên file , tên folder cho role
```
roles/x/tasks/main.yml
roles/x/handlers/main.yml
roles/x/vars/main.yml
roles/x/defaults/main.yml
roles/x/meta/main.yml
```

Có thể khởi tạo cấu trúc thư mục template để tham khảo bằng lệnh sau `ansible-galaxy init __template__`
![ansible-template](https://laptrinhvienpro.gitlab.io/_images/ansible-roles-structure.png ':size=10%')

## Role search path
Bạn phải khai báo việc set role chính xác trong ansible.cfg để ansible có thể hiểu được bạn viết role và thực thi nó.
```ansible.cfg
[default]
roles_path = ../roles
```

## Using Roles
Bạn có thể sử dụng role theo cách sau
```yaml
- hosts: dev
  roles:
    - nginx
```
Tham khảo thêm [tại đây](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html).

## Demo
**/roles/nginx/tasks/main.yml**
```yaml
- name: NGINX --> Install the nginx packages for Ubuntu target
  become: yes
  apt: 
    name: "nginx"
    update-cache: yes

- name: Check nginx status
  shell: bash -lc "systemctl status nginx"
  register: nginx_status
- debug: 
    var: nginx_status.stdout_lines

- name: NGINX --> Copy extra/sites configuration files
  become: yes
  template:
    src: nginx.conf.j2
    dest: "{{ nginx_conf_dir }}/nginx.conf"
```

**/roles/nginx/templates/nginx.conf.j2**
```
user www-data;
worker_processes auto ;

error_log  {{ nginx_log_dir }}/error.log {{ nginx_error_log_level }};
pid        {{ nginx_pid_file }};

worker_rlimit_nofile {{ nginx_worker_rlimit_nofile }};

events {
    worker_connections {{ nginx_worker_connections }};
}

http {
	default_type  application/octet-stream;
	access_log  {{ nginx_log_dir }}/access.log;
	keepalive_timeout {{ keepalive_timeout }};
	send_timeout {{ send_timeout }};
	client_body_timeout {{ client_body_timeout }};
	client_header_timeout {{ client_header_timeout }};
	proxy_send_timeout {{ proxy_send_timeout }};
	proxy_read_timeout {{ proxy_read_timeout }};

	gzip {{ nginx_gzip }};
	gzip_types  text/css text/javascript application/javascript;

	include /etc/nginx/mime.types;
	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}
```

**vars/front.yml**
```yaml
nginx_conf_dir: "/etc/nginx"
nginx_server_name: "localhost"
nginx_service_name: "nginx"
nginx_user: "nginx"
nginx_group: "nginx"
nginx_pid_file: "/var/run/nginx.pid"
nginx_worker_connections: 1024
nginx_worker_rlimit_nofile: 1024
nginx_log_dir: "/var/log/nginx"
nginx_error_log_level: "error"
nginx_gzip: "on"
nginx_start_service: true
nginx_start_at_boot: true
keepalive_timeout: 600
send_timeout: 600
client_body_timeout: 600
client_header_timeout: 600
proxy_send_timeout: 600
proxy_read_timeout: 600
```

**main-playbook.yml**
```yaml
- hosts: dev
  user: "{{ ansible_ssh_user }}"
  become_method: sudo
  gather_facts: True
  vars_files:
  - vars/front.yml
  roles:
  - nginx
```

**ansible.cf**
```ini
[defaults]
roles_path = ../roles
```

Chạy lệnh
```bash
ansible-playbook -i inventory/hosts main-playbook.yml --extra-vars " ansible_ssh_user=ubuntu"
```

![ansible-playbook-result-1](https://laptrinhvienpro.github.io/_media/ansible-playbook-result-1.png)

![ansible-playbook-result-2](/_media/ansible-playbook-result-2.png)

> Như vậy chúng ta sẽ bóc các tác vụ có điểm chung trong playbook.yml để chuyển chúng về thành 1 role là cài đặt và cấu hình nginx để chúng ta thuận tiện trong quá trình viết nhiều task.

## References

 * http://www.ansibleworks.com/docs/playbooks_roles.html
