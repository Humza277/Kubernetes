# How to Run Hello World Kubernetes 

minikube start 

kubectl get nodes

kubectl get all 

kubectl create -f helloworld.yaml

kubectl get all 

kubectl expose deployment helloworld --type=NodePort

minikube service helloworld 

# get namespaces
kubectl get namespaces ''

# create namespace
kubectl create namespace ''

# delete namespace
kubectl delete namespace ''

# namespace to a deployment 
-n namespace-''
When deploying a resource like a deployment to a specific namespace, adding this flag will make the resource exist in the namespace

#delete all pods
kubectl delete --all pods --namespace=default


# delete all deployments
 kubectl delete --all deployments --namespace=default
 
# delete all services
 kubectl delete --all services --namespace=default

#get all info
kubectl get all

#get deployments 
kubectl get deployment

#get service
kubectl get service

#get pods 
kubectl get pod


# Adding labels to a pod 

kubectl label pod/helloworld app=helloworldapp --overwrite   === add new label while pod is in progress 
    po = pod 
    
# REMOVE LABEL
kubectl label pod/helloworld app- 

#Selecting only certain labels
kubectl get pods --selector env=production 

#multiple labels 
kubectl get pods --selector dev-lead=karthik,env=production 

# to look for everything that isnt what you want add the ! to the command 
kubectl get pods --selector dev-lead!=karthik,env=staging 

# using IN operator 
kubectl get pods -l 'release-version in (1.0,5.0)' --show-labels
kubectl get pods -l 'release-version notin (1.0,5.0)' --show-labels

#delete by labels
kubectl delete pods -l dev-lead=karthik 


#application health checks 
kubectl describe pod/helloworld-deployment-with-bad-readiness-probe-789bb655f-7gtvg


#to have record of changes made to deployment 
kubectl create -f helloworld-black.yaml --record 

#application updates while live 
kubectl set image deployment/navbar-deployment helloworld=karthequian/helloworld:blue

#rollback to last version 
kubectl rollout undo deployment/navbar-deployment

#rollback to a specific version
kubectl rollout undo deployment/navbar-deployment --to-revision=1

# basic troubleshooting 
use the describe command to give a breakdown of the deployment/pod

kubectl describe deployment bad-helloworld-deployment 
kubectl describe pod/bad-helloworld-deployment 

logs 
uses the logs command to get the logs of a deployment/pod
kubectl logs helloworld-deployment-7f4fc74478-d76xn

exec
go inside the mod to determin the problem 
kubectl exec -it helloworld-deployment /bin/bash
    ps -ef = lists processes running inside the pod 
    
if multi containers in a pod use: -c
kubectl exec -it helloworld-deployment -c helloworld /bin/bash

#Enabling addons
minikube addons list

to enable a addon, use:
    minikube addons enable ' Name Here '
    
to look at the dashboard use
    minikube dashboard

# Config Maps 
kubectl create configmap logger --from-literal=log_level=debug

to find configmaps that have been made 
    kubectl get configmaps 
    
to look inside a logger in YAML format 
    kubectl get configmap/logger -o yaml 
    
# Application Secrets 
to create a secret use
    kubectl create secret generic apikey --from-literal=api_key=1234567
    
to view all of the secrets 
    kubectl get secrets
    
to view a specific key use
    kubectl get secret apikey -o yaml
    
# Jobs
Jobs are a construct that run a pod once then stop - the output of a job is kept around until you remove it 

To edit a cronjob use:
    kubectl edit cronjobs/hellocron
    
# Deploying Kubernetes to Production 
most common way to install kube is the kubeadm tool 

Install steps 
    Initially provision the master host with Docker and the Kubernetes distribution 
    Run kubeadm init, which starts kubeadm, provisions the Kubernetes control plane, and provides join token  
    Run kubeadm join with join token on each worker node. The workers will join the cluster 

Install Pod Network - Look at Flannel and Weave Net as starting possibilities 

Working in AWS infrastructure use - kops 
    automates the K8 cluster provisioning in AWS 
    Deploys high-availability masters
    Permits upgrading with kube-up 
    Uses a state-sync model for dry runs and automatic idempotency 
    Generates configuration files for AWS CloudFormation and Terraform configuration 
    Supports custom K8 add-ons 
    Uses a manifest based configuration 

Look at AEKS on AWS 

Managed K8 vs Self Install 
Managed Kubernetes offerings are popular choices for K8 in the cloud 
Spend less time configuring the K8 Control Plane, and more time on your applications

# Name Spaces 
K8 supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces
How would you use them:
    Roles and responsibilities in an enterprise
    Partitioning landscapes: dev vs test vs. prod
    Customer partitioning for non-multi-tenant scenarios - if your a consulting company and you have multiple customers you can use this feature
    Application partitioning 
    
Enterprise Roles and Responsibilities 
    Typically, teams operate independently but have some shared interfaces and APIs to communicate with each other 
    Namespaces prevent teams from confusing services and deployments that might not belong to them 
    
Tips
    Plan how you want to manage your enterprise in a K8 environment. Setting standards up front will help with long-term infrastructure management 
    Don't simply transpose existing on-premises controls and procedures onto the new environment. This could bring your existing infrastructure baggage to the new environment 
    
    
# Monitoring and Logging  
Logging platforms like Kibana with Elasticsearch with logs being shipped to them from pods using Fluentd or Filebeat/logstash

What you want to monitor 
    node health 
    Health of Kubernetes 
    Application health and metrics 

cAdvisor
    open source resource usage collector that was build for containers 
    Auto-discovers all containers in the given node and collects CPU, memory, filesystem and network usage statistics
    Provides the overall machine usage by analyzing the root container on the machine 




