# F5OS

Ansible calls to r10900 in Seattle Lab


# Connections 

You can use either: 
 - https://<f5_fqdn>/api/data
 - https://<f5_fqdn>:<f5_port>/restconf/data

*You can send a standard API call with user/password-based authentication (basic auth), and then store the token for subsequent API calls. The X-Auth-Token has a lifetime of fifteen minutes and can be renewed a maximum of five times before you need to authenticate again using basic auth. The renewal period begins at the ten-minute point, where the API will start sending a new X-Auth-Token in the response for the next five minutes. If your API calls fail to start using the new token by the 15-minute point, API calls will start returning 401 Not Authorized.*
