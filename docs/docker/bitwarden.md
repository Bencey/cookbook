Bitwarden is one of the most fastest and safest password managers, Installing it is super easy aswell. This guide will teach you how to install bitwarden using docker compose



### Prerequisites 

* [Docker](https://bencey.co.nz/2020/08/16/DockerInstall/) 
* [Docker Compose](https://docs.docker.com/compose/install/)
* Installation Key And ID From [https://bitwarden.com/host/](https://bitwarden.com/host/)


### Installation


Firstly you will need to download the bitwarden script to your machine by running the following commands depending on your Operating System


#### Linux / Mac

```
curl -Lso bitwarden.sh https://go.btwrdn.co/bw-sh \
    && chmod +x bitwarden.sh
```


#### Windows

```
Invoke-RestMethod -OutFile bitwarden.ps1 `
    -Uri https://go.btwrdn.co/bw-ps
```



You now need to start the installation by running the script using the following commands depending on your Operating System


#### Linux / Mac

```
./bitwarden.sh install
```

```
.\bitwarden.ps1 -install
```


Once you have run the commands you will be prompted to enter some information, 

First it will ask for your installation ID and key which you should of created earlier located at [https://bitwarden.com/host](https://bitwarden.com/host).

Next it will ask you some questions about SSL Certificates for this step I told bitwarden to generate a self-signed SSL Certificate

Complete the rest of the questions to your personal liking


### Running Bitwarden

Now that you have installed bitwarden you can start the program by running the following  command depending on your machine

#### Linux / Mac

```
./bitwarden.sh start
```


#### Windows

```
.\bitwarden.ps1 -start
```


After that you can verify that bitwarden is running correctly by using the following command

```
docker ps
```


Now you will be able to access bitwarden on the localhost (If you are running bitwarden from the same machine) using localhost:80 or localhost:443 Then it will prompt you to create a user account 