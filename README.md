freebsd-mysql
=============

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mysql.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mysql)

[Ansible role](https://galaxy.ansible.com/vbotka/ansible-freebsd-mysql/). Install and configure MySQL at FreeBSD.

Tested with FreeBSD 10.3 at [digitalocean.com](https://cloud.digitalocean.com)


Requirements
------------

No requiremenst.


Variables
---------

TBD.


Workflow
--------

1) Change shell to /bin/sh.

```
> ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
> ansible-galaxy install vbotka.ansible-freebsd-mysql
```

3) Fit variables.

```
~/.ansible/roles/vbotka.ansible-freebsd-mysql/vars/main.yml
```

4) Create playbook and inventory.

```
> cat ~/.ansible/playbooks/mysql.yml
---
- hosts: server
  become: yes
  become_method: sudo
  roles:
    - role: vbotka.ansible-freebsd-mysql
```

```
> cat ~/.ansible/hosts
[server]
<SERVER-IP-OR-FQDN>

[server:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_python_interpreter=/usr/local/bin/python2
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure mysql.

```
ansible-playbook ~/.ansible/playbooks/mysql.yml
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
