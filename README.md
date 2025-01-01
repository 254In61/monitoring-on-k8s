# Prometheus

Deploying Prometheus on your MicroK8s home cluster to monitor the cluster and running pods involves the following steps:

** Steps below take place in the worker-node, the-eagle

## Deployment

- MicroK8s provides a simple way to set up Prometheus with its observability addons.

1. Enable the microk8s prometheus addon:

  $ microk8s enable prometheus    .. This deprecated.

  $ microk8s enable observability

2. Confirm the deployment:

- Check namespaces : $ kubectl get namespace
- Get pods within the observability namespace : $ kubectl get pods -n observability
- Enable metrics-server : $ microk8s enable observability


3. Verify Prometheus Setup
- Confirm that Prometheus is collecting metrics: $ kubectl get svc -n observability

     ** Look for the Prometheus service***

4. Expose the Prometheus UI:
- Using a NodePort to enable outside-the-cluster reachability since no loadbalancer is deployed.
- $ kubectl apply -f manifests/prometheus-service.yaml
- Confirmation: $ kubectl get svc -n observability

## Access
- Access Prometheus from your laptop: http://<node-ip>:30090

In this case : http://192.168.1.100:30090/

# Grafana
## Deployment
1. Deploy storage : $ kubectl apply -f manifests/grafana-storage.yaml


2. Deploy deployment : $ kubectl apply -f manifests/grafana-deployment.yaml
   
   Confirm pod is up and running: $ kubectl get pods -n devops-tools
 
3. Deploy the svc for routing : $ kubectl apply -f manifests/grafana-service.yaml

   - Using a NodePort to enable outside-the-cluster reachability since no loadbalancer is deployed.

   Confirm: $ kubectl get svc -n devops-tools

## Access
- Open http://<node-ip>:31000 in your browser.

  In this case : http://192.168.1.100:31000/

- Default credentials:
  
  Username: admin | Password: admin



