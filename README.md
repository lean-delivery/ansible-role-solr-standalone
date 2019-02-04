Solr installation
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-solr-standalone/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-solr-standalone.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-solr-standalone)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-solr-standalone/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-solr-standalone)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.solr__standalone-blue.svg)](https://galaxy.ansible.com/lean_delivery/solr_standalone)
![Ansible](https://img.shields.io/ansible/role/d/30080.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F30080%2F&query=$.min_ansible_version)
## Summary

This role:
  - Installs Solr standalone on Centos 7, Ubuntu or Windows host.
  - Configures SSL for Solr 7.x
  - Configures Authentication for Solr 7.x
  - Configures Solr
  - Supported Solr versions: 6.x - 7.x. The latest tested is 7.6.0

For additional configuration, such as master or slave mode use roles:
  - solr-master (lean-delivery.ansible-role-solr-master)
  - solr-slave (lean-delivery.ansible-role-solr-slave)
  - solr-cloud (lean-delivery.ansible-role-solr-cloud)
  - to integrate SAP-Hyrbis and solr with hybris extras (lean-delivery.ansible-role-solr-hybris-config)

Requirements
------------
  - Minimal Version of the ansible for installation: 2.5
  - **Java 8** [![Build Status](https://travis-ci.org/lean-delivery/ansible-role-java.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-java)
  - **Supported OS**:
    - CentOS
      - 7
    - Ubuntu
    - Debian
    - Windows
      - "Windows Server 2008"
      - "Windows Server 2008 R2"
      - "Windows Server 2012"
      - "Windows Server 2012 R2"
      - "Windows Server 2016"
      - "Windows 7"
      - "Windows 8.1"
      - "Windows 10"

[Prepared Windows System](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html)

## Role Variables
  - `solr_version` - matches available version on https://archive.apache.org/dist/lucene/solr/. Tested versions 5.3-7.6.x
    default: `7.6.0`
  - `solr_url` - root url to download solr
    default: `http://archive.apache.org/dist/lucene/solr`
  - `solr_distr_url` - url to zip file
    default: `{{ solr_url }}/{{ solr_version }}/solr-{{ solr_version }}.zip`
  - `override_dest_main_path` - root directory to store solr folder
    default: `/opt`
    default: `C:\Solr`
  - `override_dest_solr_path` - solr folder path
    default: `{{ dest_main_path }}/solr-{{ solr_version }}`
    default: `{{ dest_main_path }}\solr-{{ solr_version }}`
  - `solr_change_default_password` - to change default password to solr user (will be solr_auth_pass)
    default: `True`
  - `solr_auth_configure` - Enable authentication
    default: `True`
  - `solr_auth_type` - authentication type
    default: `basic`
  - `solr_auth_user` - default solr user
    default: `solrserver`
  - `solr_auth_pass` - default solr user password
    default: `server123`
  - `solr_authentication_opts` - solr authentication options
    default: `-Dbasicauth={{ solr_auth_user }}:{{ solr_auth_pass }}`
  - `solr_insh_default` - solr in.sh folder
    default: `/etc/default/solr.in.sh`
  - `solr_java_xms` - heap size
    default: `512m`
  - `solr_java_xmx` - heap size
    default: `512m`
  - `solr_master_enable_jmx` - enable jmx on solr
    default: `false`
  - `solr_additional_opts` - solr options
    default: `-Xss256k`
  - `solr_user` - os user to run solr service
    default: `solr`
  - `solr_group` - os group for user
    default: `solr`
  - `solr_port` - port for solr start
    default: `8983`
  - `solr_service_name` - solr service name
    default: `solr`
  - `solr_base_path` - path to solr base
    default: `/var/solr`
  - `solr_home` - path to SOLR_HOME
    default: `{{ solr_base_path }}/data`
  - `solr_with_systemd` - to run solr as a service
    default: `True`
  - `solr_logs_dir` - path to store logs
    default: `{{ solr_base_path }}/logs`
  - `solr_wait_for_zk` - timeout to reconnect to zookeeper (in seconds)
    default: `30`
  - `solr_client_timeout` - ZooKeeper client timeout (for SolrCloud mode)
    default: `15000`
  - `solr_timezone` - timezone for solr server
    default: `UTC`
# https://lucene.apache.org/solr/guide/7_1/enabling-ssl.html
  - `solr_ssl_configure` - configure SSL
    default: `True`
  - `solr_ssl_key_size` - certificate key size
    default: 4096
  - `override_solr_ssl_key_store_path` - directory to store keystore
    default: `{{ dest_solr_path }}/server/solr`
    default: `{{ dest_solr_path }}\server\solr`
  - `solr_ssl_key_store_name` - keystore name. If file with such name exists in role folder/files - it will be used as keystore.
    default: `solr-ssl.keystore.jks`
  - `override_solr_ssl_key_store` - path to solr keystore.
    default: `{{ solr_ssl_key_store_path }}/{{ solr_ssl_key_store_name }}`
    default: `{{ solr_ssl_key_store_path }}\{{ solr_ssl_key_store_name }}`
  - `solr_ssl_key_store_password` - keystore password
    default: `123456`
  - `override_solr_ssl_trust_store` - path to trust keystore
    default: `{{ solr_ssl_key_store_path }}/{{ solr_ssl_key_store_name }}`
    default: `{{ solr_ssl_key_store_path }}\{{ solr_ssl_key_store_name }}`
  - `solr_ssl_trust_store_password` - trusted keystore password
    default: `123456`
  - `solr_ssl_need_client_auth` - Client Authentication Settings
    default: `false`
  - `solr_ssl_want_client_auth` - Client Authentication Settings
    default: `false`
  - `solr_ssl_key_store_type` - keystore type
    default: `JKS`
  - `solr_ssl_trust_store_type` - trusted keystore path
    default: `JKS`
  - `solr_ssl_check_peer_name` - Setting this to false can be useful to disable these checks when re-using a certificate on many hosts
    default: `true`
  - `solr_ssl_certificate_provider` - only for Linux os. https://docs.ansible.com/ansible/latest/openssl_certificate_module.html
    default: `selfsigned`
  - `solr_ca_domain` - certificate domain name
    default: `example.com`
  - `override_local_cert_file_path` - path to private cert
    default: `/etc/pki/tls/private`
    default: `/etc/ssl/private`
  - `solr_local_pkey_file_name` - private cert name
    default: `{{ ansible_hostname }}.ca-pkey.pem`
  - `override_local_cert_file_path` - path to public cert
    default: `/etc/pki/tls/certs`
    default: `/etc/ssl/certs`
  - `solr_local_cert_file_name` -public cert name
    default: `{{ ansible_hostname }}.ca-cert.pem`
  - `solr_set_limits` - to set limits
    default: `True`
  - `solr_open_files_limit` - linux open files limit value
    default: `65000`
  - `solr_max_processes_limit` - linux max processes limit value
    default: `65000`
# Windows variables
  - `solr_win_temp_dir` - temporary directory
    default: `C:\Windows\Temp`
  - `solr_win_ssl_subj` - CSR subject
    default: `/C=BY/ST=Minsk/L=Minsk/O=O/OU=IT/CN={{ solr_ca_domain }}`

Example Inventory
----------------
[solr]
solr.example.com

[solrwin]
solrwin.example.com

[solrwin:vars]
ansible_user=admin
ansible_password=password
ansible_connection=winrm
ansible_winrm_server_cert_validation=ignore

Example Playbook
----------------

```yml
- name: Install and Configure Solr
  hosts: solr
  vars:
    solr_change_default_password: False
  roles:
    - role: lean-delivery.java
    - role: lean-delivery.solr
```

License
-------

Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>
