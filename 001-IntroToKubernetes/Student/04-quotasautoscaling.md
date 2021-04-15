# Challenge 4: Resource Quotas and Autoscaling

[< Previous Challenge](./03-k8sdeployment.md) - **[Home](../README.md)** - [Next Challenge >](./05-helm.md)

## Introduction

Now that we have our applications running in Kubernetes, it's time to learn about resource management and scaling. In this challenge, you'll explore how to set resource limits at the namespace level and automatically scale your applications based on demand. These are critical concepts for managing resources efficiently and ensuring your applications can handle varying workloads.

## Description

Resource quotas help prevent any single namespace from consuming all available cluster resources, while Horizontal Pod Autoscaler (HPA) automatically adjusts the number of pods in a deployment based on observed CPU utilization or other select metrics.

In this challenge has two distinct parts. You will:

### Part 1:
- **Create a Resource Quota** for a namespace:
	- Create a new namespace called `quota-test`
	- Define a ResourceQuota that limits the namespace to:
		- Maximum 2 pods
		- Maximum 1 CPU core total
		- Maximum 512Mi memory total
    - **Hint** You can do this with either `kubectl` (`kubectl create quota`) directly or by using a YAML file. Remember, you can generate a YAML file with kubectl with the flags `--dry-run=client -o yaml`.
	- Apply the quota to your namespace
	- Test the quota by trying to deploy more resources than allowed (use an Nginx pod, just like we did in Exercise #2)
        - Example command: `kubectl run test-pod1 --image=nginx --overrides='{ "apiVersion": "v1", "spec": { "containers": [ { "name": "my-pod", "image": "nginx", "resources": { "limits": { "cpu": "100m", "memory": "128Mi" }, "requests": { "cpu": "100m", "memory": "128Mi" } } } ] } }' -n quota-test`

### Part 2:
- **Set up Horizontal Pod Autoscaler (HPA)** for the content-web deployment:
	- **NOTE:** Make sure your existing `content-web` deployment has resource requests defined (CPU and memory)
	- Create an HPA that targets the `content-web` deployment (`kubectl autoscale deployment`)
	- Configure the HPA to:
		- Scale between 1 and 5 replicas
		- Target 20% average CPU utilization
	- Test the autoscaler by generating load on your application and observing the scaling behavior
    - **Hint** You can run a pod and generate traffic against your application
        - Start a busybox pod: `kubectl run -it --rm load-generator --image=busybox /bin/sh`
        - Run a loop accessing the application: `while true; do wget -q -O- http://content-web:3000; done`
    - **Note** It may take a few minutes for scaling down to occur when CPU usage drops.

## Success Criteria

1. You have created a namespace with a ResourceQuota that limits pods, CPU, and memory.
2. You can demonstrate that the ResourceQuota prevents deploying resources beyond the defined limits.
3. You have configured an HPA for the `content-web` deployment that scales based on CPU utilization.
4. You can show the HPA scaling the number of replicas up when load increases and down when load decreases.
5. You understand how to monitor and troubleshoot both ResourceQuotas and HPAs using `kubectl` commands.

## Learning resources

- Resource Quotas: https://kubernetes.io/docs/concepts/policy/resource-quotas/
- Horizontal Pod Autoscaler: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
- Managing Resources for Containers: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
- HPA Walkthrough: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/
