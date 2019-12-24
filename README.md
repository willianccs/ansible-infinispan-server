Infinispan Server
=========

Ansible Galaxy Role for Infinispan Server.

Tested platform are:

* Ubuntu Linux 18.04
* CentOS 7

Requirements
------------

No, requirements.

Install
-------

```shell
$ ansible-galaxy install git+https://github.com/kazuhira-r/ansible-infinispan-server
```

`requirements.yml`

```
---
- src: https://github.com/kazuhira-r/ansible-infinispan-server
  version: master
  name: kazuhira-r.infinispan-server
```


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
    - name: ansible-infinispan-server
    # - name: kazuhira-r.infinispan-server
      vars:
        systemd_environment_file_template: "systemd/infinispan-server.j2"
        infinispan_server_config_file_template: "server/conf/infinispan.xml.j2"
```

Configuration File Template Example
-----------------------------------

`templates/server/conf/infinispan.xml.j2`

```xml
<infinispan
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="urn:infinispan:config:{{ infinispan_server_minor_version }} https://infinispan.org/schemas/infinispan-config-{{ infinispan_server_minor_version }}.xsd
                            urn:infinispan:server:{{ infinispan_server_minor_version }} https://infinispan.org/schemas/infinispan-server-{{ infinispan_server_minor_version }}.xsd"
        xmlns="urn:infinispan:config:{{ infinispan_server_minor_version }}"
        xmlns:server="urn:infinispan:server:{{ infinispan_server_minor_version }}">

   <cache-container name="default" statistics="true">
      <transport cluster="${infinispan.cluster.name}" stack="${infinispan.cluster.stack:tcp}"/>
   </cache-container>

   <server xmlns="urn:infinispan:server:{{ infinispan_server_minor_version }}">
      <interfaces>
         <interface name="public">
            <inet-address value="${infinispan.bind.address:127.0.0.1}"/>
         </interface>
      </interfaces>

      <socket-bindings default-interface="public" port-offset="${infinispan.socket.binding.port-offset:0}">
         <socket-binding name="default" port="${infinispan.bind.port:11222}"/>
         <socket-binding name="memcached" port="11221"/>
      </socket-bindings>

      <security>
         <security-realms>
            <security-realm name="default">
               <!-- Uncomment to enable TLS on the realm -->
               <!-- server-identities>
                  <ssl>
                     <keystore path="application.keystore" relative-to="infinispan.server.config.path"
                               keystore-password="password" alias="server" key-password="password"
                               generate-self-signed-certificate-host="localhost"/>
                  </ssl>
               </server-identities-->
               <properties-realm groups-attribute="Roles">
                  <user-properties path="users.properties" relative-to="infinispan.server.config.path"/>
                  <group-properties path="groups.properties" relative-to="infinispan.server.config.path" />
               </properties-realm>
            </security-realm>
         </security-realms>
      </security>

      <endpoints socket-binding="default" security-realm="default">
         <hotrod-connector name="hotrod"/>
         <rest-connector name="rest"/>
      </endpoints>
   </server>
</infinispan>
```

License
-------

Copyright &copy; 2019 kazuhira-r

Licensed under the [Apache License, Version 2.0][Apache]
 
[Apache]: http://www.apache.org/licenses/LICENSE-2.0
