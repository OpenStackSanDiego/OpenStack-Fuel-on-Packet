# OpenStack-Fuel-on-Packet


## Overview

Starting up an OpenStack private cloud can be difficult from getting all the necessary hardware, setting up the physical networking, and installing/configuring the software. To simplify the steps and get you up and running with your first cloud, we've put together this workshop. This workshop deploys a multi-node OpenStack cloud upon VirtualBox using Mirantis Fuel. 

| |
|----------|
|OpenStack VM-1, VM-2, VM-3... |
|OpenStack Controllers, Compute-1, Compute-2, Compute-3... |
|  Fuel-master VM, Fuel-slave-1 VM, slave -2 VM, slave -3, VM | 
|                  VirtualBox Hypervisor                            |
|     Bare Metal Server 32GB, 120TB, 4 core Xeon (CentOS 7)          |



## Prerequisites

For this lab, you'll need a desktop/laptop (Windows, Mac, or Linux) with an SSH client and a web browser.

You can download PuTTY, an SSH client for Windows at:

https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

Install the Windows package since you'll need one of the PuTTY utilities (putty and puttygen).

## Bare Metal Service Account

For this workshop, you'll be using physical servers provided by Packet. Specifically, you'll be using one of their 'Type 1' servers, a 32 GB, 120GB x 2 SSD (RAID-1), 4 core Xeon server. Packet provides their servers by the hour. With code SDOPENSTACK, you'll get $25 of free credit. The 'Type 1' is $0.40/hour so the $25 credit will get you 62 hours of CPU use. Be sure to shut down the server at the end of the workshop!

Sign up at www.packet.net and use code SDOPENSTACK. You'll need to provide a credit card or PayPal account to get the $25 in free credit.

## Startup a Bare Metal Server

Packet has instructions on how to startup your first server. 
You'll want to deploy a 'Type 1' server. Pick whichever region you'd like (New Jersey or California).

https://help.packet.net/quick-start/deploy-a-server

After creating a project make sure to use the following settings for deploying the server

Hostname: Fuel

Project:  Your Project Name

Config:  Type I

OS:  CentOS 7

Location: Parsippany, NJ 

If you are connecting from a Windows machine, you can use these instructions on how to generate SSH keys.

https://help.packet.net/quick-start/generate-ssh-keys

Make sure you upload your new key into Packet before you start the server!

It'll take about 8 minutes for the new server to start up.

## Log into the Bare Metal Server

Using PuTTY (Windows) or ssh (Mac/Linux), connect to the new bare metal, physical server that was deployed. Refer to step #5 in the Packet guide above if you need help. You'll be logging as as 'root' with no password required (SSH keys used instead of the password).

When connecting using PuTTY on Windows, use the following instructions to login using the SSH key generated above.

* Click Connection > Data in the left-hand navigation pane and set the Auto-login username to root.

* Click Connection > SSH > Auth in the left-hand navigation pane and configure the private key to use by clicking Browse under Private key file for authentication.

* Navigate to the location where you saved your private key earlier, select the file, and click Open.* 

## Download and Run Install Scripts

Once the you're logged in as root execute the following commands. The git command downloads the scripts to setup VirtualBox and Fuel. The "setup-fuel.sh" installs VirtualBox and downloads the Fuel scripts.

* yum -y install git
* git clone https://github.com/OpenStackSanDiego/openstack-fuel-virtualbox-packet
* cd openstack-fuel-virtualbox-packet
* sh setup-fuel.sh   (Enter "yes" when asked "replace READ.md?")

## Startup Fuel Servers

With VirtualBox installed, the nodes that will run OpenStack can be started up. The "launch" command starts up the virtual servers which will be the nodes (controller and compute) that will make up the OpenStack cloud.

* sh launch_16GB.sh

Be patient, this may take a while when you see the following in your ssh console.

        Waiting for product VM to install. Please do NOT abort the script...
        
At the conclusion, these scripts will have started a number of virtual servers within VirtualBox. These will become the nodes of the OpenStack cloud.

This command will list the running virtual machines within VirtualBox. Append it with '-l' to get the details. You should see the Fuel master and several slaves.

* VBoxManage list vms

## Log into Fuel

VirtualBox sets up a number of networks inside the physical bare metal servers (vboxnet0, vboxnet1, vboxnet2). In order to connect from your local laptop to these virtual networks running in the server we setup some port forwarding (using socat) with the following command:

* sh enable-port-forwarding.sh 

You can now log into the Fuel console (admin/admin): http://<public_IP_of_baremetal>:8000

**WARNING** - you should login to the Fuel console **immediately** after completion of the script. Otherwise bad guys may get in and play havoc with your setup!

## Use Fuel to setup your first OpenStack cloud

From here you can proceed with the cloud installation instructions available at:

https://docs.openstack.org/developer/fuel-docs/userdocs/fuel-user-guide/create-environment/start-create-env.html

Be sure to use your IP address (see above) to login rather than the IP listed in the guide.

## Shutting it all down

From the Packet Application website, select the bare metal server and mark it for deletion.

You are welcome to keep the server running but remember that you will keep being charged until you delete it via the Packet web app.

