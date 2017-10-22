## Create Deployment 
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
  kubectl describe pod    OR kubectl describe pod [pod-name]
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
check the value of increase in the cpu utilization in a different window.   Also watch the number of replicas of the pod increase when the load increases.
```bash  
  watch kubectl get hpa

  NAME                  REFERENCE                        TARGETS      MINPODS   MAXPODS   REPLICAS   AGE
  autoscaledeployment   Deployment/autoscaledeployment   138% / 10%   1         10        4          18m
```
  Delete the pod which is stressed the replica count will come down.

### PDB: 
#### Create a poddisruptionbudgets with maxUnavailable as 1
```bash
 kubectl create –f pdb.yaml
``` 
#### Drain one of slave node:
```bash
 kubectl drain raj-slave2 --ignore-daemonsets
``` 
  watch the deployment  and pod to see how they are deleted.

### Affinity :
#### Create the deployment redis-cache
```bash
          Kubectl create –f redisaffinity.yaml
```          
#### Create the deployment web-server
```bash
       Kubectl create –f webaffinity.yaml
```       
  Check which nodes the pods are scheduled
  Both of the pods are in the same node.
