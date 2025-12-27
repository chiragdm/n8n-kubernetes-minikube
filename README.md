# Deploying n8n using Kubernetes on Minikube

This guide explains how to deploy n8n using the provided Kubernetes manifests on a Minikube cluster.

## Prerequisites

Before you begin, ensure you have the following installed:

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Helm](https://helm.sh/docs/intro/install/) (optional, if using Helm charts)

## Steps to Deploy

1. **Start Minikube**

   Start your Minikube cluster if it is not already running:

   ```bash
   minikube start
   ```

2. **Apply the PostgreSQL Deployment**

   Deploy the PostgreSQL database using the `postgres-deployment.yaml` manifest:

   ```bash
   kubectl apply -f postgres-deployment.yaml
   ```

   Verify that the PostgreSQL pod is running:

   ```bash
   kubectl get pods
   ```

3. **Apply the n8n Deployment**

   Deploy n8n using the `n8n-deployment.yaml` manifest:

   ```bash
   kubectl apply -f n8n-deployment.yaml
   ```

   Verify that the n8n pod is running:

   ```bash
   kubectl get pods
   ```

4. **Set Up /etc/hosts**

   To access n8n using `n8n.local.com`, first retrieve the Minikube IP:

   ```bash
   minikube ip
   ```

   Then add the following entry to your `/etc/hosts` file, replacing `<minikube-ip>` with the IP address returned by the command above:

   ```
   <minikube-ip> n8n.local.com
   ```

   This ensures that `n8n.local.com` resolves to your Minikube cluster.

5. **Access n8n**

   To access the n8n service, you can use Minikube's service tunneling feature:

   ```bash
   minikube service n8n-service
   ```

   This will open the n8n UI in your default web browser.

## Notes

- Ensure that the `postgres-deployment.yaml` and `n8n-deployment.yaml` files are correctly configured for your environment.
- You may need to adjust resource limits or environment variables in the manifests based on your requirements.

## Troubleshooting

- If the pods are not running, check the logs for more details:

  ```bash
  kubectl logs <pod-name>
  ```

- Ensure that Minikube has sufficient resources allocated (CPU, memory, disk). You can adjust these settings when starting Minikube:

  ```bash
  minikube start --cpus=4 --memory=8192
  ```

## Cleanup

To delete the deployments and free up resources, run:

```bash
kubectl delete -f n8n-deployment.yaml
kubectl delete -f postgres-deployment.yaml
```