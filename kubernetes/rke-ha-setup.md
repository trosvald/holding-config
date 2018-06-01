#### High Availability Installation
> This document is based on rancher setup from [rancher docs ](https://rancher.com/docs/rancher/v2.x/en/installation/ha-server-install/#option-b-certificate-signed-by-a-recognized-certificate-authority)

This set of instructions creates a new Kubernetes cluster that’s dedicated to running Rancher in a high-availability (HA) configuration. This procedure walks you through setting up a 5-node cluster using the Rancher Kubernetes Engine (RKE). The cluster’s sole purpose is running pods for Rancher. The setup is based on:

- Layer 4 Loadbalancer (TCP)
- NGINX Ingress controller with SSL termination (HTTPS)

![alt text](https://rancher.com/docs/img/rancher/ha/rancher2ha.svg "topology")

#### Overview
1. Provision Linux Hosts
2. Configure Load Balancer
3. Configure DNS
4. Download RKE
5. Download Config File Template
6. Configure Nodes
7. Configure Certificates
8. Configure FQDN
9. Backup Your YAML File
10. Run RKE
11. Backup Config File
12. For those using a certificate signed by a recognized CA:
  - Remove Default Certificates
