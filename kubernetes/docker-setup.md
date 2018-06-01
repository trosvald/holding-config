#### Docker Setup

We need to install docker version specify in Rancker Kubernetes Engine docs.

```
- Add docker repo into our node(s)

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"
```

Update our repository
```
sudo apt-get update
```
Check docker available in our repository
```
sudo apt-cache policy docker-ce

docker-ce:
Installed: 17.03.2~ce-0~ubuntu-xenial
Candidate: 18.03.1~ce-0~ubuntu
Version table:
      18.03.0~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.12.1~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.12.0~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.09.1~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.09.0~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.06.2~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.06.1~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.06.0~ce-0~ubuntu 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
  *** 17.03.2~ce-0~ubuntu-xenial 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
         100 /var/lib/dpkg/status
      17.03.1~ce-0~ubuntu-xenial 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
      17.03.0~ce-0~ubuntu-xenial 500
         500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
```

As suggested by docs we need minimum docker versions **1.12 1.13.1 17.03.2**

Install docker :
```
sudo apt-get install -y docker-ce=17.03.2~ce-0~ubuntu-xenial
```
