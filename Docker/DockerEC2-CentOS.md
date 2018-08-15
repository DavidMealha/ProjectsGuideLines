# How to run Docker containers on CentOS/RHEL 7

[More Info](https://docs.docker.com/install/linux/docker-ce/centos/#install-docker-ce)

## Install Docker

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce
```

yum install --setopt=obsoletes=0 docker-ce