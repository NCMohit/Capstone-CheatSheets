## Installation

pacman -S minikube / apt-get install minikube

minikube start		-	start
				--vm-driver=<driver>	to use specific virtualization driver
minikube stop		-	stop

minikube status		-	status of services/containers

# Kubectl commands

kubectl get nodes
kubectl get replicaset
kubectl get pod
		-o wide		-	for more info output
kubectl get deployment
		-o yaml		-	get genereated config
kubectl get services

## Deployment

kubectl create deployment <depl-name> --image=<image_name>	-	create deployment
kubectl edit deployment <depl-name>	-	edit deployment

kubectl exec -it <pod_name> -- bin/bash		-	enter live environment of pod
kubectl delete deployment <depl-name>	-	delete deployment

kubectl logs <pod_name>		-	get pod logs

## Create custom config for deployments

kubectl apply -f config-file.yaml	-	start a depl from config
kubectl delete -f config-file.yaml	-	delete a depl created using config

# Config yaml format

apiVersion: apps/v1
kind: Deployment
metadata:
	name: <depl-name>
	labels:
		app: <app-name>
spec:
	replicas: <num_of_replicas>
	selector:
		matchLabels:
			app: <app-name>
	template:
		metadata:
			labels:
				app: <app-name>
		spec:
			containers:
			- name: <app-name>
				image: <image-name>
				ports:
				- containerPort: <port-num>

## Services

kubectl describe service <service-name>		-	details of service

kubectl apply -f service-config.yaml	-	Create service using config

# Create Service using yaml file

apiVersion: v1
kind: Service
metadata:
 	name: <service-name>
spec:
	selector:
		app: <app-name>
	ports:
		- protocol: TCP
		  port: <external-port>
		  targetPort: <container-port>
















