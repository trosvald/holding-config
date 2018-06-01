####  Provision Linux Hosts
Before you install Rancher, confirm you meet the host requirements. Provision 5 new Linux hosts using the requirements below.

##### Host requirements
**Operating System Requirements**

- Ubuntu 16.04 (64-bit)
- Red Hat Enterprise Linux 7.5 (64-bit)
- RancherOS 1.3.0 (64-bit)ß

**Hardware Requirements**

Hardware requirements scale based on the size of your Rancher deployment. Provision each individual node according to the requirements.

| Deployment Size| Clusters      | Nodes     |vCPUs|RAM|
| ---------------|---------------| ---------|-----------|------------|
| Small          | Up to 10      | Up to 50 |2|4GB|
| Medium         | Up to 100     | Up to 500|8|32GB|
| Large          | Over 100      | Over 500 |Contact rancher|Contact rancher|

**Software Requirements**

**Docker**
> **Note** :
If you are using **RancherOS**, make sure you switch the Docker engine to a supported version using ```sudo ros engine switch docker-17.03.2-ce```
>

_Supported Versions_
- ```1.12.6```
- ```1.13.1```
- ```17.03.02```

**Port Requirements**

Open the following ports on your Linux hosts.

| Protocol| Port range     | Purpose    |
| ---------------|:-------------:| ---------|
| Small          | Up to 10      | Up to 50 |
| Medium         | Up to 100     | Up to 500|
| Large          | Over 100      | Over 500 |
