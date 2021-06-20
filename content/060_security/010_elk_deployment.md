+++
title = "ELK deployment"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++


1. Deploy ELK in order to be able to visualize and analyze the traffic going through the Nginx App Protect web application firewall

```
kubectl apply -f files/6waf/elk.yaml
```

2. In order to connect to our ELK pod, we will need to find the public address of this service:  

```
kubectl get svc elk-web
```

{{< output >}}
NAME      TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                                        AGE
elk-web   LoadBalancer   10.0.113.59   20.90.252.56   5601:32277/TCP,9200:32278/TCP,5044:31677/TCP   42s
{{< /output >}}
3. Verify that ELK is up and running by browsing to: `http://[ELK-EXTERNAL-IP]:5601/`.  

