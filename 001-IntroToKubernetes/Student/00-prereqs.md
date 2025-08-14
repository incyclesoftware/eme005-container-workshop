# Challenge 0: Pre-requisites - Ready, Set, GO! 

**[Home](../README.md)** - [Next Challenge >](./01-containers.md)

## Introduction

A smart cloud solution architect always has the right tools in their toolbox. 

## Description

### Enabling virtualization extensions on your hardware

Ensure that virtualization technology (e.g., Intel VT-x or AMD-V) is enabled in your computer's BIOS or UEFI settings. This will vary from computer to computer, so more specific instructions cannot be provided. This step can be skipped for now, but if any subsequent steps result in error messages such as `Error code: Wsl/InstallDistro/Service/RegisterDistro/CreateVm/HCS/HCS_E_SERVICE_NOT_AVAILABLE`, you may need to return to this step before proceeding.

### Setting up Windows Subsystem for Linux (WSL)

First, we need to install WSL and set up an Ubuntu Linux distribution. WSL allows you to run a Linux environment directly on Windows without the overhead of a virtual machine.

If you already have a WSL installation with an Ubuntu image running, you can skip this step.

Open an **administrator** command prompt and run these commands.
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

After completing those steps, reboot your PC. Then, re-open an **administrator** command prompt and run:
```
wsl --install
```
This command installs WSL with the default Ubuntu distribution.

Once Ubuntu is installed, it will prompt you to create a local username and password. Use whatever values you want for this.

### Installing Node, Docker, Minikube, Kubectl, and Helm

Once you have a command prompt, make sure you're in your home directory:
```
cd ~
```
This changes to your user's home directory.

#### Install NodeJS
The applications we'll be containerizing are NodeJS applications. We won't be writing any code, but being able to run the applications will be useful so we can see how they *should* behave before we containerize them.

Update your package lists and upgrade existing packages:
```
sudo apt update && sudo apt upgrade -y
```
This ensures your system has the latest package information and upgrades all installed packages.

```
sudo apt install nodejs
```

#### Setting up package dependencies

Install necessary dependencies for adding Docker's repository:
```
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```
These packages allow apt to use HTTPS for secure package downloads, provide SSL certificates, and install curl for downloading files.

#### Installing Docker

Install Docker:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
```

#### Restart WSL
After making these changes, restart WSL from a Windows command prompt:
```
wsl --shutdown
```
This completely shuts down the WSL environment to apply the changes.

#### Verify Docker installation

Then reopen WSL:
```
wsl
```
This opens your WSL environment again.

Verify Docker installation by checking the version:
```
docker version
```
This shows the installed Docker client and server versions.

Test Docker by running a simple hello-world container:
```
docker run --rm hello-world
```
This pulls and runs the hello-world image, which prints a message confirming Docker is working correctly. The `--rm` flag automatically removes the container when it exits.

#### Installing Kubernetes CLI (kubectl)

kubectl is the command-line tool for interacting with Kubernetes clusters:

Install:
```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg # allow unprivileged APT programs to read this keyring

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list   # helps tools such as command-not-found to work correctly

sudo apt-get update
sudo apt-get install -y kubectl
kubectl version
```
These commands update the package lists with the new repository, install kubectl, and verify the installation. It's okay if `kubectl` gives an error message about not being able to talk to a Kubernetes cluster; we haven't set up minikube yet!

#### Installing Minikube

Minikube is a tool that lets you run Kubernetes locally:

Download and install the latest Minikube package:
```
cd ~
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo apt install ./minikube_latest_amd64.deb -y
rm minikube_latest_amd64.deb
```
These commands download the Minikube Debian package, install it, and remove the downloaded file.

Start Minikube and enable the metrics server addon:
```
minikube start
minikube addons enable metrics-server
minikube stop
minikube start
```
This command starts a local Kubernetes cluster.

Set Minikube to share the docker daemon with your docker installation:
```
eval $(minikube docker-env).
```

You can check the status of Minikube by running `minikube status`. You should see something that looks like this:

> minikube
> type: Control Plane
> host: Running
> kubelet: Running
> apiserver: Running
> kubeconfig: Configured

#### Installing Helm

Helm is a package manager for Kubernetes that helps you manage Kubernetes applications:

Download and run the Helm installation script:
```
cd ~
curl -fsSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 -o get_helm.sh
chmod +x get_helm.sh
./get_helm.sh
rm get_helm.sh
```

At the end of this, if you can run the command `kubectl get pod -n kube-system` and see a list of pods, we've successfully set up a local cluster we can use for the subsequent exercises.
