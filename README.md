freebsd_mysql
=============

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mysql.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mysql)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mysql/) FreeBSD. Install and configure MySQL.


Requirements
------------

No requiremenst.


Variables
---------

TBD. Review defaults and examples in vars.

- MySQL version less then 5.7 needs file *bsd_mysql_secret_local_file* with root password.
- By default the server is disabled *bsd_mysql_enable: False*


Workflow
--------

1) Change shell to /bin/sh.

```
# ansible dbserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
# ansible-galaxy install vbotka.freebsd_mysql
```

3) Fit variables.

```
# editor vbotka.freebsd_mysql/vars/main.yml
```

4) Create playbook and inventory.

```
# cat mysql.yml

- hosts: dbserver
  roles:
    - vbotka.freebsd_mysql
```

```
# cat hosts
[dbserver]
<SERVER1-IP-OR-FQDN>
<SERVER2-IP-OR-FQDN>
[dbserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_python_interpreter=/usr/local/bin/python2.7
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure mysql.

```
# ansible-playbook mysql.yml
```
		

References
----------

- [Installing MySQL on FreeBSD](https://dev.mysql.com/doc/refman/5.7/en/freebsd-installation.html)

- [Guide On How To Install MySQL On FreeBSD](http://www.xfiles.dk/guide-on-how-to-install-mysql-on-freebsd/)

License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


Author Information
------------------

[Vladimir Botka](https://botka.link)
