+++
title = "Remove configuration"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. In order to delete the resources created during this workshop, run the commands below:   

```
kubectl delete --all svc --namespace=nginx-mesh
kubectl delete --all svc --namespace=nginx-ingress
kubectl delete --all svc --namespace=default
cd terraform
terraform destroy --auto-approve
```
&nbsp;&nbsp;
  
