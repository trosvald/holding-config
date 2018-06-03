### Config for api access in kubernetes 

It's pretty 'simple' - once cargo is done you want to grab these from the master:
```
/etc/kubernetes/ssl/admin-key.pem
/etc/kubernetes/ssl/ca.pem
/etc/kubernetes/ssl/admin.pem
```
The user can then run:
```
kubectl config set-cluster default-cluster --server=https://${MASTER} \
    --certificate-authority=/path/to/ca.pem

kubectl config set-credentials default-admin \
    --certificate-authority=/path/to/ca.pem \
    --client-key=/path/to/admin-key.pem \
    --client-certificate=/path/to/admin.pem

kubectl config set-credentials default-admin \
    --certificate-authority=/path/to/ca.pem \
    --client-key=/path/to/admin-key.pem \
    --client-certificate=/path/to/admin.pem

kubectl config set-context default-system --cluster=default-cluster --user=default-admin
kubectl config use-context default-system
```
