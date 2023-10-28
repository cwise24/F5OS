# Files 

**0-get_token_tenants** playbook uses basic auth to get token. Will show tenant info and make custom list of tenant name and mgmt-ip

**1-get-images** playbook will show available images on rSeries to create tenants 

**2-create-tenant** playbook will create a tenant based on values in *create_tenant_vars* and uses the Jinja template in *j2/create_tenant*. This playbook will deploy and set tenant to running. Will check until status is running.

**10-del-tenant** playbook will delete tenant provided in the url (line 28). It is currently linked to tenant name from *create_tenant_vars*

**11-upload-image** playbook will upload image from MyF5 provided link to rSeries. Paste copied link from MyF5 to line 8.

**40-sys-health** playbook will gather system health and write to file health.json

**show-vlan** playbook will show VLANs on system and update VLAN description on vlan 10 using *vlan_name.json*. Can also be used to create a vlan using *create-vlan.json*

```
  - name: Change Vlan desc 
    ansible.builtin.uri:
      url: 'https://{{ ansible_host }}:{{ ansible_port }}/restconf/data'
      return_content: true 
      method: PATCH 
      headers:
        Content-Type: application/yang-data+json
        X-Auth-Token: "{{ token.x_auth_token }}"
      body_format: json
      body: "{{ lookup('ansible.builtin.file', 'create-vlan.json') }}"
      validate_certs: false
      status_code: 204
    register: vlan_name
 ```