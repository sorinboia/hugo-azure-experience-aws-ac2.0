+++
title = "Nginx Service Mesh deployment"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++



1. Deploy all Nginx components in the Kubernetes environment

```
kubectl apply -f files/5service_mesh/aks-kublet-webhook.yaml
./files/binaries/nginx-meshctl deploy --registry-server "sorinboiaf5" --image-tag 0.9.0 --sample-rate 1 --disable-auto-inject
```
{{< output >}}
Deploying NGINX Service Mesh Control Plane in namespace "nginx-mesh"...
Created namespace "nginx-mesh".
W0502 08:20:37.942849    7607 warnings.go:70] apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
Created SpiffeID CRD.
W0502 08:20:38.076108    7607 warnings.go:70] admissionregistration.k8s.io/v1beta1 ValidatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 ValidatingWebhookConfiguration
Waiting for SPIRE to be running...done.
Deployed Spire.
Deployed NATS server.
Created traffic policy CRDs.
Deployed Mesh API.
Deployed Metrics API Server.
Deployed Prometheus Server nginx-mesh/prometheus.
Deployed Grafana nginx-mesh/grafana.
Deployed tracing server nginx-mesh/zipkin.
All resources created. Testing the connection to the Service Mesh API Server...
Connected to the NGINX Service Mesh API successfully.
Verifying that all images were pulled...done.
NGINX Service Mesh is running.
{{< /output >}}

2. All components have been deployed in the "nginx-mesh" namespace, run the following command and validate that all pods are running
```
kubectl get pods -n nginx-mesh
```
{{< output >}}
NAME                                  READY   STATUS    RESTARTS   AGE
grafana-7f76c8b894-2flrg              1/1     Running   0          71s
nats-server-65dcd7b59b-k9k9f          1/1     Running   0          73s
nginx-mesh-api-b84969c57-bb56p        1/1     Running   0          73s
nginx-mesh-metrics-5c4dd47b89-x4xfm   1/1     Running   0          73s
prometheus-8d5fb5879-d8fz9            1/1     Running   0          72s
spire-agent-679lw                     1/1     Running   0          119s
spire-agent-72xj2                     1/1     Running   0          119s
spire-server-0                        2/2     Running   0          119s
zipkin-68dcc9f5b5-lmbhq               1/1     Running   0          71s
{{< /output >}}

