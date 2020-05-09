Các tác vụ liên quan đến nhau có thể được tập hợp lại thành role, sau đó áp dụng cho một nhóm các máy khi cần thiết.

## Structure

- **Task**: Chứa các file yaml định nghĩa các nhiệm vụ chính khi triển khai.
- **Handles**: Chứa các handler được sử dụng trong role
- **Files**: chứa các file dc sử dụng bởi role, ví dụ như các file ảnh.
- **Templates**: chứa các template file được sử dụng trong role, ví dụ như các file configuration... Các file này có đuôi *.j2, sử dụng jinja2 syntax
- **Vars**: định nghĩa các variable được sử dụng ở trong roles
- **Defaults**: Định nghĩa các giá trị default của các variable được sử dụng trong roles. Nếu variable không được định nghiã trong thư mục vars, các giá trị default này sẽ được gọi.
- **Meta**: thư mục này chứa meta data của roles

Có thể khởi tạo cấu trúc thư mục role bằng lệnh sau `ansible-galaxy init __template__`
![ansible-template](https://laptrinhvienpro.gitlab.io/_images/ansible-roles-structure.png ':size=10%')

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

## References

 * http://www.ansibleworks.com/docs/playbooks_roles.html
