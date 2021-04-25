Satellite installation in a disconnected setup
==============================================


Description
-----------

This role allows a quick installation of Satellite server with default SSL in a disconnected setup.

**Note**: The ansible playbooks and role require a configured ansible environment where the ansible nodes are reachable and are properly set up to have an IP address and a working package manager.


Features
--------
* Server deployment


Supported Satellite Versions
----------------------------

Tested with Red Hat Satellite 6


Supported Distributions
-----------------------

*RHEL/CentOS 7.7

*RHEL/CentOS 7.8



Requirements
------------

**Controller**
* Ansible version: 2.8+

**Node**
* Supported Satellite version ( see above )
* Supported distribution (see above )
* RHEL7 and Satellite 7 Binary DVD Iso either mounted as CDROM or copied into the node

Limitations
-----------

**External signed CA**
Work in progress to have an external SSL certificate for authorization.Meanwhiloe the current role can be configured with its default internal SSL certificate.

Usage
=====

Example inventory file where satellite ISO disk is mounted as a cdrom and rhel ISO is copied as ISO.It can be a combination of both.
```ini
[sat_server]
satellite.example.com

[sat_server:vars]
disk_location:
  - name: satellite
    disk_loc: /dev/sr1
    mount_loc: /media/sat6
  - name: rhel
    mount_loc: /media/rhel7-server
    disk_loc: /dev/sr0
sat_role_path: /etc/ansible/roles
sat_loc: Dubai
sat_org: RedHat
sat_username: myadminuser
sat_password: mysecretpassword
```

Example playbook to setup the Satellite server using its credentials from an  [Ansible Vault](http://docs.ansible.com/ansible/latest/playbooks_vault) file:

```yaml
---
- name: Playbook to configure Satellite server
  hosts: sat_server
  become: true
  vars_files:
  - playbook_sensitive_data.yml

  roles:
  - satellite_install_disconnected
```

Once executed , the satellite server will be available to browse at the satellite_hostname variable ( subject to host resolution ) or the IP address 


Variables
=========


Variable | Description 
-------- | ----------- 
`disk_location.name` | Specifies the name of the ISO (string)
`disk_location.disk_loc` | The location of the attched CDROM ISO file or the copied ISO file (string)
`disk_location.mount_loc` | The location where you want the files to be mounted (string)
`sat_role_path` | The role path of the installed satellite role (stringt)
`sat_loc` | The Location to be used during Satellite Installation (string)
`sat_org` | The Organization name that will be used to bootstrap the Satellite with. (string)
`sat_username` | The username of the admin user used to login to the satellite
`sat_password` | The password of the admin username


Authors
=======

Awez Sonde

