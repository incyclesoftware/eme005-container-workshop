# Challenge 2: Namespaces, Pods, and RBAC

[< Previous Challenge](./01-containers.md) - **[Home](../README.md)** - [Next Challenge >](./03-k8sdeployment.md)

## Introduction

Let's start by playing with namespaces and pods within our Minikube environment. We'll also explore Kubernetes Role-Based Access Control (RBAC) to understand how permissions work within namespaces.

## Description

A namespace is a container in which our workloads can be grouped. Things like RBAC, quotas, and so forth can be defined at the namespace level to provide access and utilizations limits on resources within a namespace. Most of our future exercises will take place in a "default" namespace, because it's not really important for a workshop environment, but we should still play with namespaces a little bit!

A pod is the smallest runnable unit within a Kubernetes cluster. A pod really just represents an instance of a container image; it's effectively equivalent to the `docker run` command we were playing with earlier, it's just running inside a node on a cluster instead of locally. Future exercises will deal with more complex orchestrations of pods.

Role-Based Access Control (RBAC) is a security mechanism that regulates access to Kubernetes resources based on the roles assigned to users or service accounts. Roles define what actions can be performed on specific resources within a namespace, while ClusterRoles define permissions across the entire cluster.

For this challenge, let's create a namespace, run a pod in it, and explore RBAC concepts:

**Part 1: Namespaces and Pods**
- Create a namespace named `my-ns`. You can use a YAML template or the Kubectl CLI directly (`kubectl create ns...`)
- Run a pod named `my-server` in the `my-ns` namespace using Nginx. `kubectl run` is going to be the easiest.

**Part 2: RBAC - Roles and Service Accounts**
- Create a service account named `my-service-account` in the `my-ns` namespace
- Create a Role that allows getting and listing pods within the `my-ns` namespace
- Create a RoleBinding that connects the service account to the role
- Test the permissions by trying to access resources as the service account (**Hint**: `kubectl auth can-i` )
- **Bonus**: Create a ClusterRole and ClusterRoleBinding to see the difference in scope 

## Success Criteria
- You have created a namespace named `my-ns`
- Validate that the namespace exists. (`get` and `describe`)
- You have run an Nginx web server pod in `my-ns` 
- Validate that the pod exists and is running. (`get` and `describe`)
- You have created a service account named `my-service-account` in the `my-ns` namespace
- You have created a Role that allows getting and listing pods in `my-ns`
- You have created a RoleBinding that connects the service account to the role
- You can demonstrate that the service account has the specified permissions
- **Bonus**: You understand the difference between Role/RoleBinding (namespace-scoped) and ClusterRole/ClusterRoleBinding (cluster-scoped)
- You have deleted your pod, namespace, and RBAC resources at the end. **Hint**: Deleting a namespace will delete everything in it, including pods and RoleBindings!

## Learning resources
- Namespaces: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
- Pods: https://kubernetes.io/docs/concepts/workloads/pods/
- RBAC: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- Service Accounts: https://kubernetes.io/docs/concepts/security/service-accounts/
- Using RBAC Authorization: https://kubernetes.io/docs/reference/access-authn-authz/rbac/