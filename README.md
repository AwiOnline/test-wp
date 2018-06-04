# WordPress + Nginx Ansible Playbook
Ansible playbook and roles for installing WordPress + Nginx + PHP + Postfix server
and push WordPress code to git repo.

### Requirements
- Ansible 2.0.0 or newer
- Ubuntu 18.04 (installed on your web server or virtual machine)

### Instructions:

### 1. Configure your web server for ssh

First of  all you need to allow connections from your development machine to the web server over ssh. You may find `ssh-copy-id` helpful. 

### 2. Set the web server IP address

Create a hosts file to set your web server's IP:

Change `10.0.2.16` to your server's URL or the IP address of your virtual machine:

```
[wordpress]
10.0.2.16
```
### 3. Set some variables
Please set variables for your WordPress site like hostname, administrator username and email etc. in file group_vars/all

```
wp_db_name: wordpress
wp_db_user: wordpress
wp_db_password: wordpress_db_password

mysql_port: 3306
mysql_root_password: root

auto_up_disable: true

server_hostname: bitfury.test

wp_system_user: www-data

core_update_level: true

wp_version: 4.9.5
wp_sha1sum: 6992f19163e21720b5693bed71ffe1ab17a4533a

admin_username: admin
admin_password: TruameushCuf
admin_email: awi@ukr.net

wp_cli_phar_url: https://raw.github.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
wp_cli_bin_path: "/usr/bin/{{ wp_cli_bin_command }}"
wp_cli_bin_command: wp

git_repo: git@github.com:AwiOnline/bitfury-test-wp.git
```


### 4. Generate ssh keys
You need to generate ssh keypair  to operate with git repo. You may use ssh-keygen command like this:

```
$ ssh-keygen -t rsa -b 2048 -f key.pem
```
and after this put key.pem to dir with playbook and key.pem.pub use to deploy key with write access in repository you need to create for this environment.

### 5. Run the playbook

```
$ ansible-playbook playbook.yml -i hosts
```

This tells ansible to use the inventory file we've called "hosts".

### 4. Finish the install
Change /etc/hosts file in your system (in linux system) to access your server by hostname by adding sting like this:
```
$ 10.0.2.16	bitfury.test
```
Open your web browser and navigate to [http://bitfury.test](http://bitfury.test) (or your webserver's IP) to finish the WordPress installation.

### Additional tasks

Also there is playbook that perform simple test smoke_test.yml. Playbook tests answer from webserver and compare string from home page.

If you need to deploy WordPress code from repositoy playbook deploy.yml helps you. By default playbook make checkout from master. But you can specify that you want checkout from:
```
$ ansible-playbook -i hosts deploy.yml --extra-vars '{"version":"1.23.45"}'
```
