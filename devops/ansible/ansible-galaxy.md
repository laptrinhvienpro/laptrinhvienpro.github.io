# Ansible Galaxy
> Ansible Galaxy là một trang web miễn phí để tìm kiếm, tải xuống, xếp hạng và xem xét tất cả các tính chất được cộng đồng Ansible phát triển.

Tôi sẽ giới thiệu một số command để bạn có thể tải xuống , tạo mới hay quản lý roles/

## Install Role
Bạn dùng command ansible-galaxy để download role từ Galaxy :
`ansible-galaxy install geerlingguy.nginx`

Kết quả
```
downloading role 'nginx', owned by geerlingguy
downloading role from https://github.com/geerlingguy/ansible-role-nginx/archive/2.7.0.tar.gz
extracting geerlingguy.nginx to /home/mai.thanh.long/Desktop/roles/geerlingguy.nginx
geerlingguy.nginx (2.7.0) was installed successfully
```

### Xem danh sách các role đã được install
```bash
ansible-galaxy list
```

## Create Role
```bash
ansible-galaxy init __template__
```
## Search for Roles
```bash
ansible-galaxy search apache --author geerlingguy
```

**Output**
```
Found 14 roles matching your search:

Name Description

geerlingguy.adminer---- Installs Adminer for Database management.
geerlingguy.apache---- Apache 2.x for Linux.
geerlingguy.apache-php-fpm---- Apache 2.4+ PHP-FPM support for Linux.
geerlingguy.certbot---- Installs and configures Certbot (for Let's Encrypt).
geerlingguy.drupal---- Deploy or install Drupal on your servers.
geerlingguy.htpasswd---- htpasswd installation and helper role for Linux servers.
geerlingguy.munin---- Munin monitoring server for RedHat/CentOS or Debian/Ubuntu.
geerlingguy.php---- PHP for RedHat/CentOS/Fedora/Debian/Ubuntu.
geerlingguy.pimpmylog---- Pimp my Log installation for Linux
geerlingguy.solr---- Apache Solr for Linux.
geerlingguy.supervisor---- Supervisor (process state manager) for Linux.
geerlingguy.svn---- SVN web server for Linux
geerlingguy.tomcat6---- Tomcat 6 for RHEL/CentOS and Debian/Ubuntu.
geerlingguy.varnish ---- Varnish for Linux.
```

## Remove an installed role
```bash
ansible-galaxy remove username.role_name
```

[View more](https://docs.ansible.com/ansible/latest/index.html)