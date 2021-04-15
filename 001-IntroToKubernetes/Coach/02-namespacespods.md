# Challenge 2: Namespaces and pods

[< Previous Challenge](./01-containers.md) - **[Home](../README.md)** - [Next Challenge >](./03-k8sdeployment.md)

## Notes

- `kubectl create ns my-ns`
- `kubectl run my-server --image=nginx --namespace my-ns`

- `kubectl get ns`
- `kubectl get pod --namespace my-ns`
- `kubectl delete ns my-ns`

### Part 2: RBAC Setup

1. **Create a service account:**
```bash
kubectl create serviceaccount my-service-account -n my-ns
```

2. **Create a Role (namespace-scoped):**
```bash
kubectl create role pod-reader --verb=get,list --resource=pods -n my-ns
```

3. **Create a RoleBinding:**
```bash
kubectl create rolebinding pod-reader-binding --role=pod-reader --serviceaccount=my-ns:my-service-account -n my-ns
```

4. **Test the permissions:**
```bash
# Test if the service account can list pods in my-ns
kubectl auth can-i list pods --as=system:serviceaccount:my-ns:my-service-account -n my-ns

# Test if the service account can list pods in default namespace (should be "no")
kubectl auth can-i list pods --as=system:serviceaccount:my-ns:my-service-account -n default

# Test if the service account can create pods (should be "no")
kubectl auth can-i create pods --as=system:serviceaccount:my-ns:my-service-account -n my-ns
```

### Bonus: ClusterRole and ClusterRoleBinding

1. **Create a ClusterRole (cluster-scoped):**
```bash
kubectl create clusterrole cluster-pod-reader --verb=get,list --resource=pods
```

2. **Create a ClusterRoleBinding:**
```bash
kubectl create clusterrolebinding cluster-pod-reader-binding --clusterrole=cluster-pod-reader --serviceaccount=my-ns:my-service-account
```

3. **Test cluster-wide permissions:**
```bash
# Now the service account should be able to list pods in any namespace
kubectl auth can-i list pods --as=system:serviceaccount:my-ns:my-service-account -n default
kubectl auth can-i list pods --as=system:serviceaccount:my-ns:my-service-account -n kube-system
```

### Verification Commands

- **Check all resources in namespace:** `kubectl get all -n my-ns`
- **View RBAC resources:** `kubectl get roles,rolebindings -n my-ns`
- **View ClusterRoles:** `kubectl get clusterroles | grep cluster-pod-reader`
- **Check permissions:** `kubectl auth can-i <verb> <resource> --as=system:serviceaccount:my-ns:my-service-account -n my-ns`
