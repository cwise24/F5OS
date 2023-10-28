# Files 

*0-get_token_tenants* playbook uses basic auth to get token. Will show tenant info and make custom list of tenant name and mgmt-ip

*1-get-images* playbook will show available images on rSeries to create tenants 

*2-create-tenant* playbook will create a tenant based on values in *create_tenant_vars* and uses the Jinja template in *j2/create_tenant*. This playbook will deploy and set tenant to running. Will check until status is running.

*10-del-tenant* playbook will delete tenant provided in the url (line 28). It is currently linked to tenant name from *create_tenant_vars*

*11-upload-image* playbook will upload image from MyF5 provided link to rSeries. Paste copied link from MyF5 to line 8.