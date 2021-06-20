+++
title = "Accessing the Nginx Controller"
menuTitle = "Nginx Controller Access"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. The Nginx Controller has already been deployed with the terraform declaration, we need to find the public IP address.

```
cd terraform
terraform refresh
export controller_ip=$(terraform output controller_ip | cut -d'"' -f2)
curl -k -c cookie.txt -X POST --url "https://$controller_ip/api/v1/platform/login" --header 'Content-Type: application/json' --data '{"credentials": {"type": "BASIC","username": "admin@nginx.com","password": "Admin2021"}}'
export controller_apikey=$(curl -k -sb cookie.txt -c cookie.txt https://$controller_ip/api/v1/platform/global | jq .currentStatus.agentSettings.apiKey | tr -d '"')

terraform output controller_ip
cd ..
```

2. Browse (using `HTTPS`) to the IP address of the Controller and verify you have access:

{{% notice %}}
Username (email): admin@nginx.com  
Password: Admin2021
{{% /notice %}}
