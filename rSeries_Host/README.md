# Files 

**0-get_token_tenants** playbook uses basic auth to get token. Will show tenant info and make custom list of tenant name and mgmt-ip

**1-get-images** playbook will show available images on rSeries to create tenants 

**2-create-tenant** playbook will create a tenant based on values in *create_tenant_vars* and uses the Jinja template in *j2/create_tenant*. This playbook will deploy and set tenant to running. Will check until status is running.

**3-interface** playbook will show all interfaces. Last play with use of **j2/state_int.j2** can disable an interface. The example is interface 2. To run this playbook to view interfaces:
```
ansible-playbook rSeries_Host/3-interface.yaml --skip-tags disable_int2
```

**4-dns** playbook will show current dns configs. Last task is to add a DNS server. To omit adding a server and to only view current configuration, run the playbook as shown below:
```
ansible-playbook rSeries_Host/4-dns.yaml --skip-tags add
```
**5-ntp** playbook to view existing ntp servers, add multiple from *vars* and change time zone setting. To skip adding servers and change time zone run:
```
ansible-playbook rSeries_Host/5-ntp.yaml --skip-tags ntp_servers
```
**7-lag** playbook will create a LAG interface and attach physical interfaces to the LAG. Makes use of *j2/lag_create* and *j2/lag_inter* template files.

**8-del_lag** playbook will remove specific interfaces from LAG and then delete LAG interface.

**10-del-tenant** playbook will delete tenant provided in the url (line 28). It is currently linked to tenant name from *create_tenant_vars*

**11-upload-image** playbook will upload image from MyF5 provided link to rSeries. Paste copied link from MyF5 to line 8.

**13-vlan_interface** playbeook will attach VLAN to interface. Under the *vars:* section define list of VLANs or empty list. Uses *j2/vlan_interface.j2* to template payload.

Empty:
```
 vars:
    interface: "2.0"
    trunked_vlans: []
```
List of VLANs:
```
 vars:
    interface: "2.0"
    trunked_vlans:
      - 440
      - 901
```
**40-sys-health** playbook will gather system health and write to file *[inventory_hostname]_health.json* in the health directory.

**41-component** playbook will show all components, status, serials etc. Also will output to file *[inventory_hostname]_compenent.json* in the health directory. 

**42-qkview** playbook will generate qkview and keep checking until file is created. After qkview is created the playbook will list all qkviews in file utilities and then download newly created qkview locally(in the *health* directory). To execute this playbook (no download)
```
ansible-playbook rSeries_Host/42-qkview.yaml --skip-tags file
```

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