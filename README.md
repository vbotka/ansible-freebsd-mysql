# freebsd_mysql

[![quality](https://img.shields.io/ansible/quality/27910)](https://galaxy.ansible.com/vbotka/freebsd_mysql)
[![Build Status](https://app.travis-ci.com/vbotka/ansible-freebsd-mysql.svg?branch=master)](https://app.travis-ci.com/vbotka/ansible-freebsd-mysql)
[![GitHub tag](https://img.shields.io/github/v/tag/vbotka/ansible-freebsd-mysql)](https://github.com/vbotka/ansible-freebsd-mysql/tags)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mysql/) FreeBSD. Install and configure MySQL.

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-mysql/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).


## Requirements

### Roles

* vbotka.ansible_lib

### Collections

* ansible.posix
* community.general
* community.mysql

### Others

See *defaults/main.yml*


## Role Variables

* See *defaults/main.yml* and examples in *vars/main.yml*
* MySQL version less then 5.7 needs the file *bsd_mysql_secret_local_file* with the root password
* By default the server is disabled *bsd_mysql_enable: False*
* freebsd_flavors_enable (default: False). This variable enables the flavors stored in
  *freebsd_flavors*. By default the flavors are disabled. This means that the default flavors from
  */etc/make.conf* will be installed. Enable this variable only if
  *freebsd_install_method=packages*. Ports won't recognize the flavors and the installation will
  crash. See the variables *bsd_mysql_packages* in *defaults/main.yml*.


## Workflow

1) Change shell on the remote host to /bin/sh if necessary

```bash
shell> ansible dbserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' \
       -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install the role and collections

```bash
shell> ansible-galaxy role install vbotka.freebsd_mysql
```

Install the collections if necessary

```bash
shell> ansible-galaxy collection install community.general
shell> ansible-galaxy collection install community.mysql
shell> ansible-galaxy collection install ansible.posix
```


3) Fit variables to your needs.


4) Create playbook and inventory

```bash
shell> cat mysql.yml
- hosts: dbserver
  roles:
    - vbotka.freebsd_mysql
```

```bash
shell> cat hosts
[dbserver]
<SERVER1-IP-OR-FQDN>
<SERVER2-IP-OR-FQDN>

[dbserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=true
ansible_become_user=root
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python3.9
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure MySQL

In development, test the role step by step

* Create directories and files

By default, the lists *bsd_mysql_directories* and *bsd_mysql_files*
are empty. As a result the below tasks will be skipped. You might want
to create *bsd_mysql_mysql_user* first if you want to manage any
directories or files before the packages are installed

```bash
shell> ansible-playbook mysql.yml -t bsd_mysql_directories
shell> ansible-playbook mysql.yml -t bsd_mysql_files
```

* Test sanity

```bash
shell> ansible-playbook mysql.yml -t bsd_mysql_sanity
```

* Review variables

```bash
shell> ansible-playbook mysql.yml -t bsd_mysql_debug -e bsd_mysql_debug=true
```

* Install packages

```bash
shell> ansible-playbook mysql.yml -t bsd_mysql_packages -e bsd_mysql_install=true -e freebsd_flavors_enable=true
```

* Apply patches

Review patches in the *file* directory and fit the variable *bsd_mysql_patches* to your
needs. Default v80 is secure initialization (--initialize) and output of *mysql_create_auth_tables*
to console. v80 works also for v81.

```bash
shell>  ansible-playbook mysql.yml -t bsd_mysql_patches -e bsd_mysql_patch_backup=true
```

* Configure mycnf.yml

```bash
shell> ansible-playbook freebsd-mysql.yml -t bsd_mysql_mycnf -e bsd_mysql_conf_backup=true
```

* Configure rc.conf

Review the list of options *bsd_mysql_rcconf*. Default for version 80 is *mysql_args: "--skip-new
--log-error"*. MySQL should be enabled *bsd_mysql_enable: true*. The service should start,
initialize databases and create temporary root password. Optionally see the log
*/var/db/mysql/${hostname}.err*.

```bash
shell> ansible-playbook freebsd-mysql.yml -t bsd_mysql_rcconf -e bsd_mysql_conf_backup=true
```

* Test existence of files that contain temporary root password

This task is tagged *never* and shall be run on demand only to test the temporary root password

```bash
shell> ansible-playbook mysql.yml -t bsd_mysql_assert
```

If the test fails find the HOWTO sections in the message.

* Change root password

WARNING: Tasks secret.yml are not idempotent !

Run the tasks '-t bsd_mysql_secret' only once to change temporary
password for *bsd_mysql_login_user* (by default root). Run '-t
bsd_mysql_assert' first to see the status of the passwords.

Store temporary root password in the local file. Change the root
password to *bsd_mysql_secret*. This task is tagged *never* and shall
be run on demand only to change the temporary root password

```bash
shell> ansible-playbook mysql.yml -t bsd_mysql_secret
```

* Run check mode and show the differences

```bash
shell> ansible-playbook mysql.yml -CD
```

* Execute the playbook

```bash
shell> ansible-playbook mysql.yml
```

* Execute the playbook again. Test the playbook is idempotent

```bash
shell> ansible-playbook mysql.yml
```

When the server is running it's sufficient to select particular tasks
by tags to reconfigure any parameter. The role should be
idempotent. It's possible to repeatedly run the whole role if
necessary.


## Troubleshooting

Enable *bsd_mysql_debug_classified*. Warning: the passwords will be displayed!


## Ansible lint

Use the configuration file *.ansible-lint.local* when running
*ansible-lint*. Some rules might be disabled and some warnings might
be ignored. See the notes in the configuration file.

```bash
shell> ansible-lint -c .ansible-lint.local
```


## References

* [MySQL Reference Manual](https://dev.mysql.com/doc/refman/8.0/en/)
* [Installing MySQL on FreeBSD](https://dev.mysql.com/doc/mysql-linuxunix-excerpt/8.3/en/freebsd-installation.html)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.info)
