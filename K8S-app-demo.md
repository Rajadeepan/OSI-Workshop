## Deployment 
  #### Use the [deployment.yaml](/Kubernetes/Yaml/deployment.yaml)  file to create the deployment
```bash 
kubectl create -f deployment.yaml
```
 #### List the deployments:
```bash  
kubectl get deployment
```  
#### List the replicaset
```bash 
kubectl get rs
```               
#### List the pods
```bash
kubectl get pods
```                
####  Details of the pod
```bash
kubectl describe pod    (OR) kubectl describe pod [pod-name]
``` 
#### Getting inside the container 
```bash
kubectl exec -it [podname]  -- /bin/bash
```
#### Delete the pod
```bash
kubectl delete pod [podname]
```

#### Scale the replicas : 
```bash
kubectl scale deployment nginx-deployment --replicas=10
```
## Upgrade the container image
####    Edit deployment.yaml change the image name to "nginx:1.7.10" 
```bash    
kubectl apply -f deployment.yaml    
```
#### Verify the container image has upgraded
```bash
kubectl get rs
NAME                          DESIRED   CURRENT   READY     AGE
nginx-deployment-5fd4f7cd66   1         1         1         3m
nginx-deployment-6f88768b67   0         0         0         3m

kubectl describe pod [podname]
```
## Horizontal Pod Autoscalar
#### Install heapster :
```bash 
 kubectl apply -f  https://raw.githubusercontent.com/kubernetes/kops/master/addons/monitoring-standalone/v1.6.0.yaml
```
#### install cadvisor:
```bash
 sudo apt-get install cadvisor
```
#### create the deployment using [autoscaledeployment.yaml](/Kubernetes/Yaml/deployment.yaml)
```bash
 kubectl create -f autoscaledeployment.yaml
```  
#### Horizontally Autoscale the deployment :
     This will create a autoscalar with target cpu utilization set to 10 percent and the number
     of replicas between 1 to 10
```bash
 kubectl autoscale deployment  autoscaledeployment --cpu-percent=10 --min=1 --max=10
``` 
#### Check Horizontal Pod Autoscalar :
```bash
kubectl get hpa
	
NAME                  REFERENCE                        TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
autoscaledeployment   Deployment/autoscaledeployment   0% / 10%   1         10        1          1m
```
#### Exec into the pod and run the load
  Running this command will increase the cpu utilization
```bash
kubectl exec -it [podname]  -- /bin/bash
```	
```bash
 for i in 1 2 3 4; do while : ; do : ; done & done
```  
Check the value of increase in the cpu utilization in a different window.   Also watch the number of replicas of the pod increase when the load increases.
```bash  
 watch kubectl get hpa

 NAME                  REFERENCE                        TARGETS      MINPODS   MAXPODS   REPLICAS   AGE
 autoscaledeployment   Deployment/autoscaledeployment   138% / 10%   1         10        4          18m
```
  Delete the pod which is stressed the replica count will come down.

## PodDisruptionBudget: 
#### Create a poddisruptiobudget using [poddisruptionbudget.yaml](/Kubernetes/Yaml/poddisruptionbudget.yaml) with maxUnavailable as 1
```bash
 kubectl create -f poddisruptionbudget.yaml
 
 kubectl get poddisruptionbudget
 NAME      MIN AVAILABLE   MAX UNAVAILABLE   ALLOWED DISRUPTIONS   AGE
 pdb       N/A             1                 0                     3m
```
#### Drain one of slave node:
```bash
 kubectl drain raj-slave2 --ignore-daemonsets
``` 
  Watch the deployment and pod, to see pods  are deleted.

## Affinity :
#### Create the deployment using [redisaffinity.yaml](/Kubernetes/Yaml/redisaffinity.yaml)
```bash
 kubectl create -f  redisaffinity.yaml
```  
#### Check which node the pod got scheduled
```bash
kubectl get pod -o wide
	
NAME                           READY     STATUS    RESTARTS   AGE       IP           NODE
redis-cache-7bf845dcfb-2dkx9   1/1       Running   0          1m        10.47.0.10   raj-slave2	
```
#### Create the deployment using [webaffinity.yaml](/Kubernetes/Yaml/webaffinity.yaml)
```bash
kubectl create -f webaffinity.yaml
```       
#### Check which node the pod got scheduled
```bash
NAME                           READY     STATUS    RESTARTS   AGE       IP           NODE
redis-cache-7bf845dcfb-2dkx9   1/1       Running   0          5m        10.47.0.10   raj-slave2
web-server-f8677f95-xzj5c      1/1       Running   0          1m        10.47.0.1    raj-slave2
```
  Both of the pods are created on the same node.
