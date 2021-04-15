# Challenge 5: Helm

[< Previous Challenge](./04-quotasautoscaling.md) - **[Home](../README.md)** 

## Introduction

There are many ways to make dinner... you can buy fresh vegetables and meat, combine it with eggs and water, season it with herbs and spices and mix it all together with cooking utensils into pots and pans and cook it in your stove, range or grill.

**OR**

You can buy a microwave dinner, peel back the foil, heat it up for 5 minutes in the microwave and start eating.

Helm is the microwave dinner of the Kubernetes world, allowing you to package entire deployments that can be installed with a single command.

## Description

In this challenge you will be installing Helm locally and in your cluster and then creating a Helm "Chart" for the FabMedical application and then using Helm to quickly deploy different version of that application.

### Without Helm
- We already experienced what deploying without Helm is like in our previous exercises -- create a service, create a deployment, create an autoscaler... the list goes on. 

### With Helm
- You can create a brand new Helm chart with `helm create`... but it includes a lot of extra "stuff" that may be daunting. The `Resources/Challenge 5` directory contains a pared-down, incomplete Helm chart that we're going to expand to do all the wonderful stuff we want to do.

Useful Helm commands:
- `helm install`
- `helm uninstall`
- `helm ugprade`

## Success Criteria

1. `helm version` shows that you have it installed locally.
1. You've created a Chart to install the Fabmedical application.
	- Content web deployment/service
	- Content API deployment/service
	- Any other things you want to include! HPA, resource quota, etc.
	- You've parameterized anything you think is valuable
1. The application installs and runs correctly.
1. You can upgrade a release 
1. You can uninstall a release

## Learning Resources

-	<https://www.digitalocean.com/community/tutorials/an-introduction-to-helm-the-package-manager-for-kubernetes>
-	<https://docs.microsoft.com/en-us/azure/aks/quickstart-helm>
-	<https://helm.sh/>
-	<https://helm.sh/docs/intro/install/>
-	<https://docs.microsoft.com/en-us/azure/aks/kubernetes-helm>
-	<https://docs.microsoft.com/en-us/azure/azure-app-configuration/integrate-kubernetes-deployment-helm>