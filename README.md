# Prometheus

Deploying Prometheus on your MicroK8s home cluster to monitor the cluster and running pods involves the following steps:

** Steps below take place in the worker-node, the-eagle

## Step 1 : Enable MicroK8s Addons
- MicroK8s provides a simple way to set up Prometheus with its observability addons.
1.1) Enable the prometheus addon:
  $ microk8s enable prometheus    .. This deprecated.
  $ microk8s enable observability

1.2) Confirm the deployment:
  - Check namespaces :
  $ kubectl get namespace
NAME              STATUS   AGE
default           Active   22d
devops-tools      Active   4d10h
kube-node-lease   Active   22d
kube-public       Active   22d
kube-system       Active   22d
observability     Active   25m

- Get pods within the observability namespace
 $ kubectl get pods -n observability
NAME                                                     READY   STATUS    RESTARTS      AGE
alertmanager-kube-prom-stack-kube-prome-alertmanager-0   2/2     Running   1 (24m ago)   24m
kube-prom-stack-grafana-649cb6588b-pb4fk                 3/3     Running   0             25m
kube-prom-stack-kube-prome-operator-7c7d5757db-7vlff     1/1     Running   0             25m
kube-prom-stack-kube-state-metrics-7c69654b66-c426g      1/1     Running   0             25m
kube-prom-stack-prometheus-node-exporter-8st6x           1/1     Running   0             25m
kube-prom-stack-prometheus-node-exporter-j5v76           1/1     Running   0             25m
loki-0                                                   1/1     Running   0             24m
loki-promtail-gctrm                                      1/1     Running   0             24m
loki-promtail-jwjbd                                      1/1     Running   0             24m
prometheus-kube-prom-stack-kube-prome-prometheus-0       2/2     Running   0             24m
tempo-0                                                  2/2     Running   0             24m

- Enable metrics-server
 $ microk8s enable observability
Infer repository core for addon observability
Addon core/observability is already enabled

## Step 2 : Verify Prometheus Setup
2.1) Confirm that Prometheus is collecting metrics:
 $ kubectl get svc -n observability
** Look for the Prometheus service***

2.2) Expose the Prometheus UI:
- Use a NodePort or an ingress rule to access it from your laptop
- Since no loadbalancer is to be deployed.
- REF: manifests/prometheus-service.yaml
 $ kubectl apply -f manifests/prometheus-service.yaml

Confirmation: 
 $ kubectl get svc -n observability
 !
 !
 prometheus-svc           3m8s                NodePort    10.152.183.210   <none>        9090:30090/TCP
 !

2.3) Access Prometheus from your laptop:
http://<node-ip>:30090
In this case : http://192.168.1.100:30090/

# Grafana
## Deployment
1. Deploy storage
   $ kubectl apply -f manifests/grafana-storage.yaml
persistentvolume/grafana-pv created
persistentvolumeclaim/grafana-pvc created

2. Deploy deployment
  $ kubectl apply -f manifests/grafana-deployment.yaml
deployment.apps/grafana-deployment created

 Confirm pod is up and running:
  $ kubectl get pods -n devops-tools
 NAME                                  READY   STATUS    RESTARTS   AGE
 grafana-deployment-7d967f9fd5-wzzrc   1/1     Running   0          7m22s

3. Deploy the svc for routing

$ kubectl apply -f manifests/grafana-service.yaml
service/grafana-svc created

Confirm:
 $ kubectl get svc -n devops-tools
NAME          TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
grafana-svc   NodePort   10.152.183.59    <none>        3000:31000/TCP   19m

## Access
- Open http://<node-ip>:31000 in your browser.

- Default credentials:
Username: admin
Password: admin



