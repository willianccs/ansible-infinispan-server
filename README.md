Infinispan Server
=========

Ansible Galaxy Role for Infinispan Server.

Tested platform are:

* Ubuntu Linux 18.04
* CentOS 7

Requirements
------------

No, requirements.

Role Variables
--------------

```yaml
infinispan_server_version: "10.1.0.Final"

infinispan_server_user: "ispn"
infinispan_server_install_dir: "/opt/"

infinispan_server_config_file_template: ""    # infinispan.xml template path if needed

jdk_version: "11"    # e.g. 11, 1.8.0

systemd_environment_file_template: ""    # systemd environment-file template path if needed
```

Facts
--------------

```yaml
infinispan_server_minor_version: 10.1
```

Dependencies
------------

No, dependencies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
---
- name: install infinispan server
  hosts: infinispan_servers
  roles:
    - name: infinispan-server
      vars:
        systemd_environment_file_template: "systemd/infinispan-server.j2"
        infinispan_server_config_file_template: "server/conf/infinispan.xml.j2"
```

License
-------

Copyright &copy; 2019 kazuhira-r

Licensed under the [Apache License, Version 2.0][Apache]
 
[Apache]: http://www.apache.org/licenses/LICENSE-2.0
