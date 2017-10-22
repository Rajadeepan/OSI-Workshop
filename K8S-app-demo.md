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
  kubectl describe pods
``` 
#### Getting inside the container 
```bash
  kubectl exec –it [podname]  -- /bin/bash
```
#### Delete the pod
```bash
	kubectl delete pod [podname]
```

#### Scale the replicas : 
```bash
  kubectl scale deployment [deployment name] --replicas=10
```  
#### Install heapster :
```bash 
  kubectl apply -f  https://raw.githubusercontent.com/kubernetes/kops/master/addons/monitoring-standalone/v1.6.0.yaml
```
#### install cadvisor:
```bash
  sudo apt-get install cadvisor
```
#### create the deployment using autoscaledeployment.yaml
```bash
  kubectl create –f autoscaledeployment.yaml
```  
#### Autoscale the deployment :
```bash
  kubectl autoscale deployment  autoscaledeployment --cpu-percent=10 --min=1 --max=10
``` 
#### Exec into the pod and run the load
```bash
  for i in 1 2 3 4; do while : ; do : ; done & done
```  
  Watch kubectl get hpa to see the increase in the cpu utilization.
  Replicas of the pod increase when the load increases.
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
