# freebsd_mysql

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mysql.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mysql)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mysql/) FreeBSD. Install and configure MySQL.


## Requirements

See *defaults/main.yml*

## Variables

Review *defaults/main.yml* and examples in *vars/main.yml*

- MySQL version less then 5.7 needs file *bsd_mysql_secret_local_file* with root password
- By default the server is disabled *bsd_mysql_enable: False*


## Workflow

1) Change shell to /bin/sh

```
shell> ansible dbserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' \
       -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role

```
shell> ansible-galaxy install vbotka.freebsd_mysql
```

3) Fit variables

```
shell> editor vbotka.freebsd_mysql/vars/main.yml
```

4) Create playbook and inventory

```
shell> cat mysql.yml

- hosts: dbserver
  roles:
    - vbotka.freebsd_mysql
```

```
shell> cat hosts
[dbserver]
<SERVER1-IP-OR-FQDN>
<SERVER2-IP-OR-FQDN>
[dbserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=yes
ansible_become_user=root
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python3.7
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure MySQL

* Review variables

Set *bsd_mysql_debug: true*

```
shell> ansible-playbook mysql.yml -t bsd_mysql_debug
```

* Create directories

```
shell> ansible-playbook mysql.yml -t bsd_mysql_directories
```

* Test sanity

```
shell> ansible-playbook mysql.yml -t bsd_mysql_sanity
```

* Install packages

```
shell> ansible-playbook mysql.yml -t bsd_mysql_packages
```

* Apply patches

Review patches in the *file* directory and fit the variable
*bsd_mysql_patches* to your needs. Default v80 is secure
initialization (--initialize) and output of *mysql_create_auth_tables*
to console.

```
shell> ansible-playbook mysql.yml -t bsd_mysql_patch
```

* Configure mycnf.yml

```
shell> ansible-playbook mysql.yml -t bsd_mysql_mycnf
```

* Configure rc.conf

Review the list of options *bsd_mysql_rcconf*. Default for version 80
is *mysql_args: "--skip-new --log-error"*. MySQL should be enabled
*bsd_mysql_enable: true*. The service should start, initialize
databases and create root password. Optionaly see the log
*/var/db/mysql/${hostname}.err*.

```
shell> ansible-playbook mysql.yml -t bsd_mysql_rcconf
```

* Test existence of files that contain temporary root password

This task is tagged *never* and shall be run on demand only to test
the temporary root password.

```
shell> ansible-playbook mysql.yml -t bsd_mysql_assert
```

If the test fails find the HOWTO sections in the message.

* Change root password

Store temporary root password in the local file. Change the root
password to *bsd_mysql_secret*. This task is tagged *never* and shall
be run on demand only to change the temporary root password.

```
shell> ansible-playbook mysql.yml -t bsd_mysql_secret
```

* Run check mode and show the differences

```
shell> ansible-playbook mysql.yml -CD
```

* Execute the playbook

```
shell> ansible-playbook mysql.yml
```

* Execute the playbook again. Test the playbook is idempotent.

```
shell> ansible-playbook mysql.yml
```


## References

- [Installing MySQL on FreeBSD](https://dev.mysql.com/doc/refman/5.7/en/freebsd-installation.html)

- [Guide On How To Install MySQL On FreeBSD](http://www.xfiles.dk/guide-on-how-to-install-mysql-on-freebsd/)

## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.link)
