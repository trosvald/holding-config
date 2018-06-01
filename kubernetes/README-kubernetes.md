## Kubernetes Infrastructure

### Nodes Setup
5 nodes cluster with specific ip(s) and roles detail
```
oso-master.mpti.co.id         192.168.11.21/24      [controller,etcd,worker]
oso-infra.mpti.co.id          192.168.11.22/24      [controller,worker]
oso-n0.mpti.co.id             192.168.11.23/24      [controller,worker]
oso-n1.mpti.co.id             192.168.11.24/24      [controller,worker]
oso-n2.mpti.co.id             192.168.11.25/24      [controller,worker]
```

### Host Resources & Disk Layout

Node : oso-master.mpti.co.id
```
- 16GB RAM, 4 vCPU
/dev/sda          80Gi
/dev/sdb          100Gi         docker-storage
```

Node : oso-infra.mpti.co.id
```
- 12GB RAM, 4 vCPU
/dev/sda          80Gi
/dev/sdb          100Gi         docker-storage
```

Node : oso-n0.mpti.co.id, oso-n1.mpti.co.id, oso-n2.mpti.co.id
```
- 10GB RAM 4 vCPU
/dev/sda          80Gi
/dev/sdb          200Gi         glusterfs
/dev/sdc          100Gi         docker-storage
```
