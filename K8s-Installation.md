<p align="center">
<h2> Steps for Installing Kubernetes </h2>
</p>

##### 1.Docker Installation:
 
   ### 1.1 Install using the repository
      
	       1.Update the apt package index:
```bash	     
    		$ sudo apt-get update
```	 
         2.  Install Docker
```bash	          
	      	$ apt-get install -y docker.io
```
	       3. Verify docker installation
```bash         
		     $ sudo docker run hello-world
```
##### 2. Installing Kubelet and Kubeadm : 
	       1. SSH into the machine and become root if you are not already
``` 
			$ sudo -i
```   
		2. Install latest version of kubeadm  
```bash 		
	 	# apt-get update && apt-get install -y apt-transport-https
```		
	3. 
```bash	
		curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
```		
	4. #  
```
		cat <<EOF >/etc/apt/sources.list.d/kubernetes.list 
		deb http://apt.kubernetes.io/ kubernetes-xenial main 
		EOF
```
	5. update
		# apt-get update
	6. Installing kubelet and kubeadm
		# apt-get install -y kubelet kubeadm

##### 3. Create cluster : 
1. Intalling kubeadm on your master node
	a. Initializing master node
	 ```
		# kubeadm init 
        ```
	b. exit from the root user
	```
		$ exit
	```
	c To start using the cluster need to run (as a regular user)
	```
		$ mkdir -p $HOME/.kube
		$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
		$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
        ```
	
	d To create a POD network install weave network
```
		$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```		
		
	f.  Verify all components are running
		$ kubectl get pods  --all-namespaces
```
NAMESPACE     NAME                                       READY     STATUS    RESTARTS   AGE
kube-system   etcd-ip-172-31-29-159                      1/1       Running   0          2m
kube-system   kube-apiserver-ip-172-31-29-159            1/1       Running   0          2m
kube-system   kube-controller-manager-ip-172-31-29-159   1/1       Running   0          2m
kube-system   kube-dns-2425271678-jv13r                  3/3       Running   0          2m
kube-system   kube-proxy-bns8z                           1/1       Running   0          2m
kube-system   kube-scheduler-ip-172-31-29-159            1/1       Running   0          2m
kube-system   weave-net-tlf8x                            2/2       Running   0          1m
 ```	
	g.  $ kubectl get nodes
NAME               	STATUS    AGE       VERSION
ip-172-31-29-159   	Ready        6m           v1.7.1 
 
##### Steps For installing Kubenetes on Worker Node

### 1.Docker Installation:	
   
   1.1 Install using the repository
      
	1.Update the apt package index:
	     
    		$ sudo apt-get update
	
	2.  install
		$ apt-get install -y docker.io

  	3. Verify docker installation
		$ sudo docker run hello-world

### 2. Installing Kubelet and Kubeadm : 
	1. SSH into the machine and become root if you are not already
		$ sudo -i
	2. Install latest version of kubeadm  
	 	$ apt-get update && apt-get install -y apt-transport-https
	3.  
	```
		$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
	```
	4.
	```
		cat <<EOF >/etc/apt/sources.list.d/kubernetes.list 
		deb http://apt.kubernetes.io/ kubernetes-xenial main 
		EOF
	```
	5. update
		$ apt-get update
	6. Installing kubelet and kubeadm
		$ apt-get install -y kubelet kubeadm
### 3. Join node to the master
       	1.SSH to the machine 
     	2.Become root (e.g. sudo su -) 
 	3.Run the command that was output by kubeadm init. For example:
```	
	$ kubeadm join --token <token> <master-ip>:<master-port>
```
	4.To get the token of master is received on executing command  “kubeadm init” in Master



kubeadm join --token 77b91b.0fa5009c2b0b227a 172.31.20.158:443
