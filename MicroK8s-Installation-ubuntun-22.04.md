# Install microk8s on Ubuntu 22.04 

**Machine specs** 

Ram:  4GB 

CPU:  2 

Storage:  25GB 

 

**Update the machine** 

`sudo apt update -y` 

 

**Install microk8s** 

`sudo snap install microk8s --classic --channel=1.32` 

 

**Join user to the group** 

`sudo usermod -a -G microk8s $USER` 

`mkdir -p ~/.kube` 

`chmod 0700 ~/.kube` 

 

**Re-enter the session for the group update to take place** 

`exit`             // logout and login again 

 

**Run this command and wait for few seconds to see status** 

`microk8s status --wait-ready` 

 

**Set alias** 

`alias kubectl='microk8s kubectl'` 

 

**Access the kubernetes** 

`kubectl get nodes`

 

 

# Adding a worker node on microk8s cluster 

**Update the machine** 

`sudo apt update -y` 

 

**Install microk8s on worker node** 

`sudo snap install microk8s --classic --channel=1.32` 

 

**Run this command on master node** 

`microk8s add-node` 

// This will return some joining instructions which should be executed on the MicroK8s instance that you wish to join to the cluster 

 

**Run this command on node you wish to join as a worker** 

> [!Note]
> Both machines should be on the same network 

`sudo microk8s join <join command given by “microk8s add-node” command>`  

 

**Run this command on Master node to check if node is joined successfully**  

`microk8s kubectl get nodes` 

**Set Alias** 

`alias kubectl='microk8s kubectl'` 

 

**To stop microk8s** 

`microk8s stop` 
