# Challenge 4: Resource Quotas and Autoscaling

[< Previous Challenge](./03-k8sdeployment.md) - **[Home](../README.md)** - [Next Challenge >](./05-helm.md)

## Instructor Guide - Solution Commands

### Prerequisites
First, ensure the metrics server is enabled in Minikube:
```bash
minikube addons enable metrics-server
```

### Part 1: Resource Quota Setup

1. **Create the namespace:**
```bash
kubectl create namespace quota-test
```

2. **Create ResourceQuota using kubectl:**
```ba \
  --namespace=quota-test
```

3. **Verify the quota:**
```bash
kubectl describe quota compute-quota -n quota-test
```

4. **Test the quota by trying to exceed limits:**
```bash
# Create first pod - should succeed

kubectl run test-pod1 --image=nginx --overrides='{ "apiVersion": "v1", "spec": { "containers": [ { "name": "my-pod", "image": "nginx", "resources": { "limits": { "cpu": "100m", "memory": "128Mi" }, "requests": { "cpu": "100m", "memory": "128Mi" } } } ] } }' -n quota-test

# Create second pod - should succeed
kubectl run test-pod2 --image=nginx --overrides='{ "apiVersion": "v1", "spec": { "containers": [ { "name": "my-pod", "image": "nginx", "resources": { "limits": { "cpu": "100m", "memory": "128Mi" }, "requests": { "cpu": "100m", "memory": "128Mi" } } } ] } }' -n quota-test

# Try to create third pod - should fail due to pod count limit
kubectl run test-pod3 --image=nginx --overrides='{ "apiVersion": "v1", "spec": { "containers": [ { "name": "my-pod", "image": "nginx", "resources": { "limits": { "cpu": "100m", "memory": "128Mi" }, "requests": { "cpu": "100m", "memory": "128Mi" } } } ] } }' -n quota-test

# Or try to exceed CPU quota
kubectl run test-pod3 --image=nginx --overrides='{ "apiVersion": "v1", "spec": { "containers": [ { "name": "my-pod", "image": "nginx", "resources": { "limits": { "cpu": "800m", "memory": "128Mi" }, "requests": { "cpu": "800m", "memory": "128Mi" } } } ] } }' -n quota-test

```

### Part 2: Horizontal Pod Autoscaler Setup

1. **Create HPA for content-web using kubectl:**
```bash
kubectl autoscale deployment content-web --cpu-percent=20 --min=1 --max=5
```

2. **Verify HPA was created:**
```bash
kubectl get hpa
kubectl describe hpa content-web
```

### Part 3: Testing and Monitoring

1. **Monitor HPA status:**
```bash
kubectl get hpa
kubectl describe hpa content-web-hpa
```

2. **Generate load to test autoscaling:**
```bash
# Get the service URL
minikube service content-web --url

# In a separate terminal, generate load using wget or curl
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh
# Inside the pod:
while true; do wget -q -O- http://content-web:3000/; done
```

3. **Watch the scaling in action:**
```bash
kubectl get pods -w
kubectl get hpa -w
```

4. **Monitor resource usage:**
```bash
kubectl top pods
kubectl top nodes
```

### Verification Commands for Students

- **Check ResourceQuota status:** `kubectl describe quota -n quota-test`
- **Check HPA status:** `kubectl get hpa` and `kubectl describe hpa content-web`
- **Monitor pods:** `kubectl get pods -w`
- **Check resource usage:** `kubectl top pods`
- **View HPA events:** `kubectl describe hpa content-web | grep Events -A 10`

### Cleanup Commands
```bash
kubectl delete hpa content-web
kubectl delete namespace quota-test
kubectl delete pod load-generator --ignore-not-found=true
```