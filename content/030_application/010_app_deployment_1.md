+++
title = "App deployment"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. Deploy the app
```
kubectl apply -f files/4ingress/1arcadia_delpoy.yaml
```

{{< output >}}
deployment.apps/arcadia-db created
service/arcadia-db created
deployment.apps/arcadia-frontend created
service/arcadia-frontend created
deployment.apps/arcadia-login created
service/arcadia-login created
deployment.apps/arcadia-stock-transaction created
service/arcadia-stock-transaction created
deployment.apps/arcadia-stocks created
service/arcadia-stocks created
deployment.apps/arcadia-users created
service/arcadia-users created
{{< /output >}}

2. Check that all is deployed and working as expected:  
```
kubectl get pods
```
{{< output >}}
NAME                                        READY   STATUS    RESTARTS   AGE
arcadia-db-696f979799-sdzb6                 1/1     Running   0          36s
arcadia-frontend-57ff4f888-v4ggh            1/1     Running   0          36s
arcadia-login-c995f8dcc-7gvvp               1/1     Running   0          35s
arcadia-stock-transaction-b996cddb9-xzckf   1/1     Running   0          34s
arcadia-stocks-dd58548f7-k7tg7              1/1     Running   0          34s
arcadia-users-768dddd844-8s75q              1/1     Running   0          33s
{{< /output >}}

```
kubectl get svc -owide
```
{{< output >}}
NAME                        TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)     AGE     SELECTOR
arcadia-db                  ClusterIP   10.0.136.204   <none>        27017/TCP   2m56s   app=arcadia-db
arcadia-frontend            ClusterIP   10.0.30.220    <none>        80/TCP      2m56s   app=arcadia-frontend
arcadia-login               ClusterIP   10.0.201.149   <none>        80/TCP      2m56s   app=arcadia-login
arcadia-stock-transaction   ClusterIP   10.0.248.28    <none>        80/TCP      2m56s   app=arcadia-stock-transaction
arcadia-stocks              ClusterIP   10.0.160.135   <none>        80/TCP      2m56s   app=arcadia-stocks
arcadia-users               ClusterIP   10.0.211.57    <none>        80/TCP      2m56s   app=arcadia-users
kubernetes                  ClusterIP   10.0.0.1       <none>        443/TCP     28m     <none>
{{< /output >}}

{{% notice info %}}
The application is not accessible yet. We will deploy the NGINX Ingress in the following sections.
{{% /notice %}}