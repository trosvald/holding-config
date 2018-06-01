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

#### Configure direct LVM mode manually
The procedure below creates a logical volume configured as a thin pool to use as backing for the storage pool. It assumes that you have a spare block device at **/dev/sdX** with enough free space to complete the task. The device identifier and volume sizes may be different in your environment and you should substitute your own values throughout the procedure. The procedure also assumes that the Docker daemon is in the **stopped** state.

1. Identify the block device you want to use. The device is located under **/dev/** (such as **/dev/sdc**) and needs enough free space to store the images and container layers for the workloads that host runs. A solid state drive is ideal.

2. Stop docker service
```
$ sudo systemctl stop docker
```

3. Install the following packages:
  - **RHEL / CentOS** : _device-mapper-persistent-data_, _lvm2_, and all dependencies.
  - **Ubuntu / Debian** : _thin-provisioning-tools_, _lvm2_, and all dependencies.

4. Create a physical volume on your block device from **step 1**, using the pvcreate command. Substitute your device name for **/dev/xvdf**.
> **WARNING !!** : The next few steps are destructive, so be sure that you have specified the correct device!
>
```
$ sudo pvcreate /dev/sdc
Physical volume "/dev/sdc" successfully created.
```

5. Create a ```docker``` volume group on the same device, using the ```vgcreate``` command.
```
$ sudo vgcreate docker /dev/sdc
Volume group "docker" successfully created
```

6. Create two logical volumes named ```thinpool``` and ```thinpoolmeta``` using the ```lvcreate``` command. The last parameter specifies the amount of free space to allow for automatic expanding of the data or metadata if space runs low, as a temporary stop-gap. These are the recommended values.
```
$ sudo lvcreate --wipesignatures y -n thinpool docker -l 95%VG
Logical volume "thinpool" created.
```
```
$ sudo lvcreate --wipesignatures y -n thinpoolmeta docker -l 1%VG
Logical volume "thinpoolmeta" created.
```
7. Convert the volumes to a thin pool and a storage location for metadata for the thin pool, using the ```lvconvert``` command.
```
$ sudo lvconvert -y \
--zero n \
-c 512K \
--thinpool docker/thinpool \
--poolmetadata docker/thinpoolmeta
```
>WARNING: Converting logical volume docker/thinpool and docker/thinpoolmeta to thin pool's data and metadata volumes with metadata wiping. THIS WILL DESTROY CONTENT OF LOGICAL VOLUME (filesystem etc.) Converted docker/thinpool to thin pool.
>
8. Configure autoextension of thin pools via an ```lvm``` profile. create directory if needed.
```
$ sudo vi /etc/lvm/profile/docker-thinpool.profile
```
9. Specify ```thin_pool_autoextend_threshold``` and ```thin_pool_autoextend_percent``` values.

  ```thin_pool_autoextend_threshold``` is the percentage of space used before ```lvm``` attempts to autoextend the available space (100 = disabled, not recommended).

  ```thin_pool_autoextend_percent``` is the amount of space to add to the device when automatically extending (0 = disabled).

  The example below adds ```20%``` more capacity when the disk usage reaches ```80%```.
  ```
  activation {
    thin_pool_autoextend_threshold=80
    thin_pool_autoextend_percent=20
}
  ```
  Save the file
10. Apply the LVM profile, using the ```lvchange``` command.
```
$ sudo lvchange --metadataprofile docker-thinpool docker/thinpool
Logical volume docker/thinpool changed.
```
11. Enable monitoring for logical volumes on your host. Without this step, automatic extension does not occur even in the presence of the LVM profile.
```
$ sudo lvs -o+seg_monitor
```
12. If you have ever run Docker on this host before, or if ```/var/lib/docker/``` exists, move it out of the way so that Docker can use the new LVM pool to store the contents of image and containers.
```
$ mkdir /var/lib/docker.bk
$ mv /var/lib/docker/* /var/lib/docker.bk
```
If any of the following steps fail and you need to restore, you can remove ```/var/lib/docker``` and replace it with ```/var/lib/docker.bk```.
13. Edit ```/etc/docker/daemon.json``` and configure the options needed for the ```devicemapper``` storage driver. If the file was previously empty, it should now contain the following contents:
```
{
    "storage-driver": "devicemapper",
    "storage-opts": [
      "dm.thinpooldev=/dev/mapper/docker-thinpool",
      "dm.use_deferred_removal=true",
      "dm.use_deferred_deletion=true"
    ]
}
```
14. Start Docker.

  **systemd**:
```
$ sudo systemctl start docker
```
  **service**:
```
$ sudo service docker start
```
15. Verify that Docker is using the new configuration using ```docker info``` .
```
$ docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.03.1-ce
Storage Driver: devicemapper
 Pool Name: docker-thinpool
 Pool Blocksize: 524.3 kB
 Base Device Size: 10.74 GB
 Backing Filesystem: xfs
 Data file:
 Metadata file:
 Data Space Used: 19.92 MB
 Data Space Total: 102 GB
 Data Space Available: 102 GB
 Metadata Space Used: 147.5 kB
 Metadata Space Total: 1.07 GB
 Metadata Space Available: 1.069 GB
 Thin Pool Minimum Free Space: 10.2 GB
 Udev Sync Supported: true
 Deferred Removal Enabled: true
 Deferred Deletion Enabled: true
 Deferred Deleted Device Count: 0
 Library Version: 1.02.135-RHEL7 (2016-11-16)
<output truncated>
```
If Docker is configured correctly, the ```Data file``` and ```Metadata``` file is blank, and the pool name is ```docker-thinpool```.

16. After you have verified that the configuration is correct, you can remove the ```/var/lib/docker.bk``` directory which contains the previous configuration.
```
$ rm -rf /var/lib/docker.bk
```
