<p align="center">
<h2> Steps for Installing Kubernetes </h2>
</p>

## 1.Docker Installation:
 
   ####  Install using the repository
	1. SSH into the machine and become root if you are not already
``` 
	    $ sudo -i
```      
	2. Update the apt package index:
```bash	     
    	    $ sudo apt-get update
```	 
	3.  Install Docker
```bash	      
	    $ apt-get install -y docker.io
```
	4. Verify docker installation
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
	    $ apt-get update && apt-get install -y apt-transport-https
```		
	3. Add the GPG key for the official Docker repository to the system
```bash	
	    $ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
```		
	4.   Add the Docker repository
```
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
	5. Update the package database with the Docker packages from the newly added repo
```bash
	      $ apt-get update
```		
	6. Installing kubelet and kubeadm
```bash	
	      $ apt-get install -y kubelet kubeadm kubectl
```		

##### 3. Create cluster :  
 #### Installing kubeadm on your master node

	a. Initializing master node
```bash	
	      $ kubeadm init 
```
	b. Save the join command to run on slave node 
```bash
	kubeadm join --token b019d9.1c2bceadba43a96a 10.140.0.2:6443 --discovery-t
	oken-ca-cert-hash sha256:0ff9a5cc627d07d038aa959fb736e2999da0f535de0bd04e69a
	916c43e5c53a3
```
	c. Exit from the root user
```bash
	      $ exit
```
	d. To start using the cluster need to run (as a regular user)
```bash
	      $ mkdir -p $HOME/.kube
	      $ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
	      $ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
	e. To create a POD network install weave network
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
```	
	NAME               	STATUS    AGE       VERSION
	ip-172-31-29-159   	Ready        6m           v1.7.1 
 ```
## Steps For installing Kubenetes on Worker Node

### 1.Docker Installation:	
   
   #### Install using the repository

	1. SSH into the machine and become root if you are not already
```bash
		$ sudo -i
```
	2.Update the apt package index:
```bash		     
    	   $ sudo apt-get update
```	
	3.  Install docker
```bash		
		$ apt-get install -y docker.io
```
	4. Verify docker installation
```bash		
		$ sudo docker run hello-world
```

### 2. Installing Kubelet and Kubeadm : 

	1. SSH into the machine and become root if you are not already
```bash
		$ sudo -i
```	
	2. Install latest version of kubeadm  
```bash	
	 	$ apt-get update && apt-get install -y apt-transport-https
```		
	3.  Add the GPG key for the official Docker repository to the system
```bash
		$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
```
	4. Add the Docker repository
```
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list 
deb http://apt.kubernetes.io/ kubernetes-xenial main 
EOF
```
	5. Update
```bash	
		$ apt-get update
```		
	6. Installing kubelet and kubeadm
```bash	
		$ apt-get install -y kubelet kubeadm kubectl
```		
### 3. Join node to the master

  	1.SSH to the slave machine 
  	2.Become root (e.g. sudo su -) 
  	3.Run the command that was output by kubeadm init. For example:
```bash	
	$ kubeadm join --token <token> <master-ip>:<master-port> --discovery-token-ca-cert-hash <hash value>
```
	4.To get the token of master is received on executing command  “kubeadm init” in Master
```
kubeadm join --token b019d9.1c2bceadba43a96a 10.140.0.2:6443 --discovery-t
oken-ca-cert-hash sha256:0ff9a5cc627d07d038aa959fb736e2999da0f535de0bd04e69a
916c43e5c53a3
```
