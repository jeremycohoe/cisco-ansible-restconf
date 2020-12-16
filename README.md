
Example 1:
```
- name: Test New RESTCONF Modules
  hosts: csr1000v.lab.cisco.com
  gather_facts: no
  connection: httpapi

  vars: 
    ansible_connection: httpapi
    ansible_httpapi_port: 443
    ansible_httpapi_use_ssl: yes
    ansible_network_os: restconf
    ansible_user: admin
    ansible_httpapi_password: *******
    ansible_httpapi_validate_certs: false
    ansible_become: true
    ansible_become_method: enable

  tasks: 
    - name: restconf_get 
      restconf_get:
        content: config
        output: json
        path: /data/Cisco-IOS-XE-native:native/username
      register: cat9k_rest_config

    - debug: var=cat9k_rest_config
```

Example 2:

```
$ cat group_vars/restconf_routers.yml 
---
# Connectivity parameters
ansible_connection: "httpapi"
ansible_network_os: "restconf"
ansible_httpapi_use_ssl: true
ansible_httpapi_validate_certs: false
ansible_httpapi_port: 9443
ansible_httpapi_restconf_root: "/restconf"  # default
ansible_user: "developer"
ansible_password: "C1sco12345"

# Infrastructure parameters
new_interfaces:
  interface:
    - name: "Loopback42518"
      type: "iana-if-type:softwareLoopback"
      enabled: true
      ietf-ip:ipv4:
        address:
          - ip: "192.0.2.42"
            netmask: "255.255.255.255"
...
```

Example 3:

```
$ cat hosts.yml 
---
all:
  children:
    restconf_routers:
      hosts:
        latest:
          ansible_host: "ios-xe-mgmt-latest.cisco.com"  # IOS-XE 16.11.01a
        stable:
          ansible_host: "ios-xe-mgmt.cisco.com"  # IOS-XE 16.9.3
```
