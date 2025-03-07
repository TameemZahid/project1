# Docker & Docker Compose Installation on Ubuntu 22.04


**Machine specs on which these commands are tested**

Ram:  4GB

CPU:  2

Storage:  25GB


**Update the machine**

`sudo apt update -y`



**Run the following command to uninstall all the conflicting packages**

`for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done`




**Setup docker’s apt repository**

// Add Docker's official GPG key:

```
sudo apt-get update

sudo apt-get install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc
```


**Add the repository to Apt sources:**
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
  https://download.docker.com/linux/ubuntu $(./etc/os-release && echo \
  "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \ 
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```


`sudo apt-get update`




**Install the docker packages**

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y`






**Verify the installation is successful**

`sudo docker run hello-world`




**To manage docker as a non root user**

1. Craete docker group
`sudo groupadd docker`

2. Add your user to the docker group
`sudo usermod -aG docker $USER`

3. Run following command to activate changes to group
`newgrp docker`

4. Now run docker command without sudo
`docker run hello-world`

