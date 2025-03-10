# Checkmk Installation and Client Integration Guide (Ubuntu 22.04 LTS)

## Part 1: Checkmk Installation

**Host Machine: Your laptop**
**Remote Machine: The machine you accessed through SSH**
**Client Machine: The machines that have to be monitored**


> [!Note]
> Make sure TCP port 6556 is open on all client machines to allow the Checkmk server to fetch monitoring data.




**Run the below commands on the remote machine**

`sudo apt update && sudo apt upgrade -y`


**Download Checkmk Raw Edition**

`wget https://download.checkmk.com/checkmk/2.3.0p27/check-mk-raw-2.3.0p27_0.jammy_amd64.deb`


**Add the lines below into /etc/apt/sources.list file with sudo**

```
deb http://archive.ubuntu.com/ubuntu/ jammy main
deb http://archive.ubuntu.com/ubuntu/ jammy universe
deb http://archive.ubuntu.com/ubuntu jammy-security main
deb http://archive.ubuntu.com/ubuntu jammy-security universe
# deb http://archive.ubuntu.com/ubuntu/ jammy-updates main
# deb http://archive.ubuntu.com/ubuntu/ jammy-updates universe
```

**Install Checkmk**

`sudo apt install ./check-mk-raw-2.3.0p27_0.jammy_amd64.deb -y`


**Verify Checkmk Installation**

`omd version`


**Create Checkmk Site**

`sudo omd create osfp`

> [!IMPORTANT]
> Note down the username and password given by this command


**Start Checkmk Site**

`sudo omd start osfp`


**Enable Public Access to Web Interface**

`sudo omd config osfp set APACHE_TCP_ADDR 0.0.0.0`


**Access Checkmk from host machine’s browser**

`http://<public-ip>/osfp`




## Part 2: Checkmk Client (Agent) Installation and Host Integration


**Download Checkmk Agent (From Web Interface),In your Checkmk Web Interface, go to**

`Setup > Agents > Linux`

**Under Packaged Agents, download the Linux agent file**

`check-mk-agent_2.3.0p27-1_all.deb`


**From your host (laptop), transfer the agent file to the Client Machine (Remote) via SCP**

`scp -i <path-to-pem-file> check-mk-agent_2.3.0p27-1_all.deb <username>@<public-ip>:~`


**Install Agent on Remote Machine**

`sudo dpkg -i check-mk-agent_2.3.0p27-1_all.deb`


**Verify Agent Installation using the following command**

`check_mk_agent`


**Add Remote Machine as Host in Checkmk**

```
1. Open Checkmk Web Interface
2. In the left sidebar, go to: Setup > Hosts
3. Click Add Host (top-left corner).
4. Under the Basic settings section, write host name 
5. Under The network address section, check the IPv4 Address box and write the private IP of the remote machine
6. Click on Save and run service discovery and wait for a few seconds
7. Click on Activate All button on the top left side 
8. Now click on the yellow button on the top right side to Activate Changes and click on the red round arrow beside pencil to apply
```

**Verify Monitoring**

In the left sidebar, go to

`Monitor > All Hosts`

Please make sure your client (remote machine) is listed and data is being received.
