
# Keycloak-Kubernetes

This tutorial shows how to deploy a Keycloak to Kubernetes cluster.

## Keycloak Deployment

There are many ways through which we can deploy Keycloak.

- Deployment with Keycloak distribution file.

- Deployment with Docker.

- Deployment with Kubernetes.

# Deploy Keycloak cluster locally
For local development I will use kubeadm . 


#Start and bootstrap kubeadm :
Create EC2 instance for Master node(T2.Medium instance) and for Worker node(T2.micro instance). 
Minimum Requirements for Master: 2 CPU, 4 GB RAM, t2.Medium, ubuntu (20.04) Worker node: 2 CPU, 4 GB RAM, t2.Medium, ubuntu (20.04).
           
#  To create kubeadm cluster we are using Shortscript :

- On master and worker node:

      {
      apt-get update

      apt-get install apt-transport-https

      apt install -y curl docker.io

      systemctl enable docker

      curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

      sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

      apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
      sudo swapoff -a 
      }


# Only on master node:
      
      { 
      kubeadm init 

      mkdir -p $HOME/.kube

      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

      sudo chown $(id -u):$(id -g) $HOME/.kube/config

      kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

      kubeadm token create --print-join-command
      }
 For information on how to create a cluster with kubeadm you can refer this link also https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/ 
# Keycloak Kubernetes Deployment:

We can deploy Keycloak on Kubernetes cluster by creating deployment, service, ingress yaml’s for it.

# 1.Deployment.yaml
- Once the yaml for deployment is created, we need to apply the deployment using below command.

      kubectl apply -f deployment.yaml

- To check if the deployment is created or not, run below command.

      kubectl get deployments

# 2.service.yaml

We need to create service file to expose Keycloak application. By default Keycloak work on port 8080.     
Since Keycloak by default work on port 8080, I have defined the target port as 8080.

- Once the yaml for service is created, we need to apply the deployment using below command.

      kubectl apply -f service.yaml  
- To check if the service is created or not, run below command.

      kubectl get svc     
# 3. Ingress.yaml
-   Once the yaml for ingressis created, we need to apply the deployment using below command.

        kubectl apply -f ingress.yaml

- To check if the ingress is created or not, run below command.

      kubectl get ingress       
