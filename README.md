# Prometheus

Deploying Prometheus on your MicroK8s home cluster to monitor the cluster and running pods involves the following steps:

** Steps below take place in the worker-node, the-eagle

## Step 1 : Enable MicroK8s Addons
- MicroK8s provides a simple way to set up Prometheus with its observability addons.

1.1) Enable the prometheus addon:

  $ microk8s enable prometheus    .. This deprecated.

  $ microk8s enable observability

1.2) Confirm the deployment:

- Check namespaces : $ kubectl get namespace

- Get pods within the observability namespace : $ kubectl get pods -n observability

- Enable metrics-server : $ microk8s enable observability


## Step 2 : Verify Prometheus Setup
2.1) Confirm that Prometheus is collecting metrics: $ kubectl get svc -n observability

     ** Look for the Prometheus service***

2.2) Expose the Prometheus UI:
- Use a NodePort or an ingress rule to access it from your laptop since no loadbalancer is to be deployed.
- $ kubectl apply -f manifests/prometheus-service.yaml

Confirmation: $ kubectl get svc -n observability

2.3) Access Prometheus from your laptop: http://<node-ip>:30090


In this case : http://192.168.1.100:30090/

# Grafana
## Deployment
1. Deploy storage : $ kubectl apply -f manifests/grafana-storage.yaml


2. Deploy deployment : $ kubectl apply -f manifests/grafana-deployment.yaml

   Confirm pod is up and running: $ kubectl get pods -n devops-tools
 
3. Deploy the svc for routing : $ kubectl apply -f manifests/grafana-service.yaml

   Confirm: $ kubectl get svc -n devops-tools

## Access
- Open http://<node-ip>:31000 in your browser.

- Default credentials:
  
  Username: admin | Password: admin



