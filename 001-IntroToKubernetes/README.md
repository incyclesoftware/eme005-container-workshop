# Intro To Kubernetes
## Introduction
This intro level course will help you get hands-on experience with Docker, Kubernetes and Helm. Kubernetes has quickly gone from being the shiny new kid on the block to the defacto way to deploy and orchestrate containerized applications.

This course starts off by covering containers, what problems they solve, and why Kubernetes is needed to help orchestrate them.  You will learn all of the Kubernetes jargon (pods, services, and deployments, oh my!).  By the end, you should have a good understanding of what Kubernetes is and how to orchestrate workloads within it.

## Learning Objectives
In this course you will solve a common challenge for companies migrating to the cloud. You will take a simple multi-tiered web app, containerize it, and deploy it to an Minikube cluster. Once the application is in Kubernetes, you will learn how to tweak all the knobs and levers to scale, manage and monitor it.

1. Containerize an application
2. Understand key Kubernetes management areas: scalability, upgrades and rollbacks, storage, networking, package management and monitoring

## Challenges
- Challenge 0: **[Pre-requisites - Ready, Set, GO!](Student/00-prereqs.md)**
   - Prepare your workstation to work with Docker containers and Minikube
- Challenge 1: **[Got Containers?](Student/01-containers.md)**
   - Package the "FabMedical" app into a Docker container and run it locally.
- Challenge 2: **[Pods and Namespaces](Student/02-namespacepods.md)**
   - Learn about creating namespaces and pods
- Challenge 3: **[Your First Deployment](Student/03-k8sdeployment.md)**
   - Pods, Services, Deployments: Getting your YAML on! Deploy the "FabMedical" app to your Minikube cluster. 
- Challenge 4: **[Scaling and High Availability](Student/04-quotasautoscaling.md)**
   - Flex Kubernetes' muscles by scaling pods. Create quotas and observe how Kubernetes responds to resource limits.
- Challenge 5: **[Helm](Student/05-helm.md)**
   - Install Helm tools, customize a sample Helm package to deploy FabMedical and use the Helm package to redeploy FabMedical.
   

## Repository Contents
- `../Coach/Guides`
  - [Lecture presentation](Coach/Lectures.pptx) with short presentations to introduce each challenge.
- `../Coach/Solutions`
   - Example solutions to the challenges (If you're a student, don't cheat yourself out of an education!)
- `../Student/Resources`
   - FabMedial app code and sample templates to aid with challenges
