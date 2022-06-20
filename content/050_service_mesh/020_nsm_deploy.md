+++
title = "Nginx Service Mesh deployment"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++



1. Deploy all Nginx components in the Kubernetes environment

```
./files/binaries/nginx-meshctl deploy  --tracing-backend zipkin --sample-rate 1 --disable-auto-inject
```
{{< output >}}
Deploying NGINX Service Mesh...
All resources created. Testing the connection to the Service Mesh API Server...
Connected to the NGINX Service Mesh API successfully.
NGINX Service Mesh is running.
{{< /output >}}

2. All components have been deployed in the "nginx-mesh" namespace, run the following command and validate that all pods are running
```
kubectl get pods -n nginx-mesh
```
{{< output >}}
NAME                                  READY   STATUS    RESTARTS   AGE
grafana-7c6c88b959-ch69c              1/1     Running   0          89s
nats-server-6d7b6779fb-z649q          2/2     Running   0          89s
nginx-mesh-api-7864df964-q95hr        1/1     Running   0          89s
nginx-mesh-metrics-559b6b7869-d2scr   1/1     Running   0          89s
prometheus-8d5fb5879-dwfkr            1/1     Running   0          89s
spire-agent-h9427                     1/1     Running   0          89s
spire-server-0                        2/2     Running   0          89s
zipkin-7698dbfc5c-sm9w6               1/1     Running   0          89s
{{< /output >}}

