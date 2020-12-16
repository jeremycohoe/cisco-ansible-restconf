
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
