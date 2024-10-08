======================================
vbotka.freebsd_mysql 2.6 Release Notes
======================================

.. contents:: Topics


2.6.2
=====

Release Summary
---------------
Maintenance and bugfix update.

Major Changes
-------------
- Update bsd_mysql_role_version

Minor Changes
-------------
- Update python 3.11 in .travis.yml
- Format meta/main.yml

Bugfixes
--------
- Delete changelog/#CHANGELOG-v2.6.rst#

Breaking Changes / Porting Guide
--------------------------------


2.6.1
=====

Release Summary
---------------
Ansible 2.17 update.

Major Changes
-------------
* Add supported 14.1

Minor Changes
-------------
* Update README.
* Update sanity.yml
* Update debug
* Add var bsd_mysql_role_version


2.6.0
=====

Release Summary
---------------
Ansible 2.16 update.

Major Changes
-------------
* Supported FreeBSD: 13.3, 14.0
* Supported MySQL: 5.7, 8.0, 8.1, 8.2, 8.3

Minor Changes
-------------
* Add changelog.
* Update README, list, meta, and travis
* Update debug format.

Bugfixes
--------
* Fix Ansible lint.
* Set owner=bsd_mysql_secret_local_file_owner of
  bsd_mysql_secret_local_dir. Otherwise the test
  'bsd_mysql_secret_local_file is exists' will fail.

Breaking Changes / Porting Guide
--------------------------------
* Default bsd_mysql_install=false
* Default al_supported_versions_override=[]
* Default +bsd_mysql_directories=[]
* Default +bsd_mysql_files=[]
* bsd_mysql_patches_list changed to bsd_mysql_patches_dict
