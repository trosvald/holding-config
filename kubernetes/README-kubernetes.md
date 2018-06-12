## Kubernetes Infrastructure

### Nodes Setup
6 nodes cluster with specific ip(s) and roles detail

- 3 masters,etcd
- 6 worker node

**NODE LIST**
```
k8s-m0.mpti.co.id             192.168.11.21/24      [master,etcd,node]
k8s-m1.mpti.co.id             192.168.11.22/24      [master,etcd,node]
k8s-m2.mpti.co.id             192.168.11.23/24      [master,etcd,node]
k8s-n0.mpti.co.id             192.168.11.24/24      [node,glusterfs]
k8s-n1.mpti.co.id             192.168.11.25/24      [node,glusterfs]
k8s-n1.mpti.co.id             192.168.11.26/24      [node,glusterfs]
```

### Host Resources & Disk Layout

Node : k8s-m```[0-2]```.mpti.co.id
```
- Ubuntu 16.04 LTS
- 10GB RAM, 4 vCPU
/dev/sda          40Gi
/dev/sdb          80Gi         docker-storage
```

Node : k8s-n```[0-2]```.mpti.co.id
```
- Ubuntu 16.04 LTS
- 10GB RAM, 4 vCPU
/dev/sda          40Gi
/dev/sdb          80Gi         docker-storage
/dev/sdc          200Gi        glusterfs
```
