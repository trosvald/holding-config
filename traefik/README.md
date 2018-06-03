# Traefik deplyment notes

## Deploy traefik as services on host

New internal project for replacing existing external load balancer ```HAProxy```
```
- Old HAProxy Node
Hostname : lb01.mpti.co.id
IP Address : 192.168.11.11/24
```
```
- New Traefik Load Balancer
Hostname : ext-lb.mpti.co.id
IP Address : 192.168.11.14/24
```

### Installing traefik
We only need to install Traefik and set it up, and to add an entry to our hosts file.
#### Download binary
Go to GitHub and [download the latest stable release of Traefik](https://github.com/containous/traefik/releases/latest) for your architecture, and make it executable. As of now, it’s ```1.6.2```.

```
$ wget https://github.com/containous/traefik/releases/download/v1.6.2/traefik_linux-amd64
$ mv traefik_linux-amd64 traefik
$ sudo chmod u+x traefik
```

#### Create config file
Create a file in the same folder as traefik called ```traefik.toml``` with the following content :
```
[web]
address = ":8080"
ReadOnly = true

[kubernetes]
# Kubernetes server endpoint
endpoint = "http://controller.example.com:8080"
namespaces = ["default","kube-system"]
```

The above configuration will tell Traefik to only serve content from the two namespaces “default” and “kube-system”. If you want Traefik to serve all namespaces, simply remove the line.

The ```[web]``` section tells traefik to serve a dashboard on port 8080. This is handy and will present all the services traefik is currently serving.

The ```[kubernetes]``` section describes how traefik should connect to your kubernetes kube-apiserver. In this example the kube-apiserver is listening on port 8080 at controller.example.com. If you use SSL (and listen on port 8443) you can define cert and token in these two files:

```
/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
/var/run/secrets/kubernetes.io/serviceaccount/token
```

> NOTE: Even if you don’t use a token or certificate, you need these two files. In that case, just created the two files without any content.

#### Start Traefik
Now start Traefik either manually
```
./traefik -c traefik.toml
```

or, if you want Traefik to run as a service, you can use this as a template
```
[Unit]
Description=Traefik proxy server
Documentation=https://github.com/containous/traefik

[Service]
ExecStart=/root/traefik/traefik \
  -c /root/traefik/traefik.toml
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```
