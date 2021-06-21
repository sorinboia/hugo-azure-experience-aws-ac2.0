+++
title = "Nginx Kubernetes Ingress Installation"
menuTitle = "Nginx K8s Ingress Installation"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

We are going to use the Nginx installation manifests based on the [Nginx Ingress Controller installation guide](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/).
For simplicity - we have already prepared an installation script.  
1. Run the command bellow:  

```
./files/4ingress/ingress_install.sh
```
{{< output >}}
Starting Nginx Ingress Install
Cloning into 'kubernetes-ingress'...
remote: Enumerating objects: 36045, done.
remote: Counting objects: 100% (3722/3722), done.
remote: Compressing objects: 100% (831/831), done.
remote: Total 36045 (delta 3166), reused 2902 (delta 2890), pack-reused 32323
Receiving objects: 100% (36045/36045), 50.39 MiB | 25.75 MiB/s, done.
Resolving deltas: 100% (19933/19933), done.
Note: checking out 'v1.11.1'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 32745366 Release 1.11.1 (#1514)
namespace/nginx-ingress created
serviceaccount/nginx-ingress created
clusterrole.rbac.authorization.k8s.io/nginx-ingress created
clusterrolebinding.rbac.authorization.k8s.io/nginx-ingress created
clusterrole.rbac.authorization.k8s.io/nginx-ingress-app-protect created
clusterrolebinding.rbac.authorization.k8s.io/nginx-ingress-app-protect created
secret/default-server-secret created
configmap/nginx-config created
Warning: networking.k8s.io/v1beta1 IngressClass is deprecated in v1.19+, unavailable in v1.22+; use networking.k8s.io/v1 IngressClassList
ingressclass.networking.k8s.io/nginx created
customresourcedefinition.apiextensions.k8s.io/virtualservers.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/virtualserverroutes.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/transportservers.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/policies.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/globalconfigurations.k8s.nginx.org created
globalconfiguration.k8s.nginx.org/nginx-configuration created
customresourcedefinition.apiextensions.k8s.io/aplogconfs.appprotect.f5.com created
customresourcedefinition.apiextensions.k8s.io/appolicies.appprotect.f5.com created
customresourcedefinition.apiextensions.k8s.io/apusersigs.appprotect.f5.com created
deployment.apps/nginx-ingress created
configmap/nginx-config configured
service/nginx-ingress created
Install finished
Save this random number for later 34f2b9f6
{{< /output >}}
  
2. Expose the Nginx Ingress Dashboard.
```
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: dashboard-nginx-ingress
  namespace: nginx-ingress
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
    name: http
  selector:
    app: nginx-ingress
EOF
```

3. Check what we did so far is actually working:

```
kubectl get svc --namespace=nginx-ingress
```
{{< output >}}
NAME                      TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
dashboard-nginx-ingress   LoadBalancer   10.0.114.225   20.90.252.195   80:30766/TCP                 38s
nginx-ingress             LoadBalancer   10.0.114.203   20.90.248.2     80:31470/TCP,443:31908/TCP   3m36s
{{< /output >}}



Note the EXTERNAL-IP of the "dashboard-nginx-ingress". This is the IP address that we are going to use in order to view the Nginx Dashboard.  
Browse to the following location and verify you can see the dashboard: `http://<DASHBOARD-EXTERNAL-IP>/dashboard.html`

Note the EXTERNAL-IP of the "nginx-ingress". This is the hostname/IP address that we are going to use in order to publish the Arcadia web application.  
Browse to the following location and verify that you receive a 404 status code: `http://<INGRESS-EXTERNAL-IP>/`  

{{% notice warning %}}
Please note that it might take some time for the DNS names to become available.
{{% /notice %}}

4. Save the EXTERNAL-IPs as env variables for later use
```
export dashboard_nginx_ingress=$(kubectl get svc dashboard-nginx-ingress --namespace=nginx-ingress | tr -s " " | cut -d' ' -f4 | grep -v "EXTERNAL-IP")
export nginx_ingress=$(kubectl get svc nginx-ingress --namespace=nginx-ingress | tr -s " " | cut -d' ' -f4 | grep -v "EXTERNAL-IP")
```