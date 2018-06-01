#### High Availability Installation
> This document is based on rancher setup from [rancher docs ](https://rancher.com/docs/rancher/v2.x/en/installation/ha-server-install/#option-b-certificate-signed-by-a-recognized-certificate-authority)
>
This set of instructions creates a new Kubernetes cluster that’s dedicated to running Rancher in a high-availability (HA) configuration. This procedure walks you through setting up a 5-node cluster using the Rancher Kubernetes Engine (RKE). The cluster’s sole purpose is running pods for Rancher. The setup is based on:

- Layer 4 Loadbalancer (TCP)
- NGINX Ingress controller with SSL termination (HTTPS)

![alt text](https://rancher.com/docs/img/rancher/ha/rancher2ha.svg "topology")

#### Overview
Provision 5 Linux hosts to serve as your Kubernetes cluster.
> a. [Provision Linux Hosts](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-01.md)
>
Configure your load balancer to have a highly available single point of entry to your Rancher cluster.
> b. [Configure Load Balancer](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-02.md)
>
Make your setup accessible using a DNS name by configuring the DNS to point to your loadbalancer.
> c. [Configure DNS](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-03.md)
>
Rancher Kubernetes Engine (RKE) is a fast, versatile Kubernetes installer that you can use to install Kubernetes on your Linux hosts.
> d. [Download RKE](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-04.md)
>
**RKE** uses a ```.yml``` config file to install and configure your Kubernetes cluster. Download one of our config file templates to get started.
> e. [Download Config File Template](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-05.md)
>
Configure the **Nodes** section of the template.
> f. [Configure Nodes](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-06.md)
>
Configure the **Certificates** part of the template too.
> g. [Configure Certificates](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-07.md)
>
You guessed it. Configure the **FQDN** part of the template.
> h. [Configure FQDN](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-08.md)
>
After you’ve completed configuration of the config file template, back the config file up in a safe place. You can reuse this file for upgrades later.
> i. [Backup Your YAML File](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-09.md)
>
Run RKE to deploy Rancher to your cluster.
> j. [Run RKE](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-10.md) 
>
During installation, RKE generates a config file that you’ll use later for upgrades. Back it up to a safe location.
> k. [Backup Config File](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-11.md)
>
If you chose Option B as your SSL option, log into the Rancher UI and remove the certificates that Rancher automatically generates. [Remove default certificate](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-12.md)
> l. [For those using a certificate signed by a recognized CA:](https://github.com/trosvald/holding-config/blob/master/kubernetes/rke/rke-ha-12.md)
