# Docker Swarm Setup


A Docker swarm is a group of devices (Either Physical or virtual) that are running docker and have been configured to "link" up together to form a cluster. Each device that is apart of the cluster is called a node, The tasks for the cluster are controlled by a swarm manager.

!!! note
    In my cluster (Swarm) I have the following

    Minimum of 3 swarm managers to ensure that there is an effective failover just incase a node "Fails"

    NFS that is accessible by all the nodes to share configs and data

    5 Devices total: 2 Raspberry Pis with 1gb RAM, 2 Raspberry Pis with 2gb RAM, and 1 PC with 4gb RAM

## Getting Started

This guide will  teach you how to install docker and setup swarm mode.

On each machine, you will need to install Docker using the official repository installation method.


Before docker is installed we will need to set up the repository and then install and update docker


### Docker Repo Setup
First, we will need to run ```sudo apt-get update``` to update the package list.

Then we install `apt-transport-https`, `ca-certificates`, `curl`, `gnupg-agent` and `software-properties-common` which allows `apt` to use a repository over https.

```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
``` 

Now we will need to add the official GPG key for docker by running ```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```

Then we can verify that we have the proper key which will have the key `9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88`

We can run the following command that searches the last 8 characters of the key `sudo apt-key fingerprint 0EBFCD88`


Now depending on the distribution of your machine, you will need to run a certain command. You can check the distribution by running `lsb_release -cs`. Please make sure that you run the command that matches your distribution

**x86_64 / amd64**
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

**armhf**
```
sudo add-apt-repository \
   "deb [arch=armhf] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```


**arm64**
```
sudo add-apt-repository \
   "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```


### Install the Docker Engine

Now you will need to update the package index by running
```
sudo apt-get update
```

and then install the latest version of docker and containerd by running

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```




## Setting up a docker swarm


A swarm is the simplest way to ensure redundancy so if a single device is to unexpectedly encounter an issue (Such as a total power failure) none of the services will be interrupted



Now we are ready to create a swarm. First, you will need to decide which Node (Device) You want to be the leader. On the leader node, you will need to run `docker swarm init` 


```
[ubuntu@node1 ~]# docker swarm init
Swarm initialized: current node (ID) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-token \
    IP:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

[ubuntu@node1 ~]#
```


so now what you will need to do is run the `docker swarm join` command on all of the nodes you wish to be connected to the swarm

